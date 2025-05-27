```python
import ast
import builtins

# Define which AST node types are allowed
ALLOWED_NODE_TYPES = {
    'Module', 'Expr', 'Assign', 'AugAssign', 'For', 'While', 'If', 'Compare',
    'BoolOp', 'BinOp', 'UnaryOp', 'Call', 'Name', 'Load', 'Store', 'Constant',
    'List', 'Tuple', 'Dict', 'Subscript', 'Index', 'Slice', 'Attribute',
    'IfExp', 'Return'
}

# Whitelist of importable modules
ALLOWED_MODULES = {'pandas', 'numpy'}

# Optional: whitelist of allowed function names
ALLOWED_FUNCTIONS = {
    'len', 'range', 'min', 'max', 'sum', 'sorted',
    # pandas and numpy functions will be accessed via pd.<func> or np.<func>
}

# Blocked attributes to prevent introspection
BLOCKED_ATTRIBUTES = {'__globals__', '__code__', '__closure__', '__self__'}

class ASTWhitelist(ast.NodeVisitor):
    """
    AST visitor that raises an exception if disallowed nodes or patterns are found.
    """

    def generic_visit(self, node):
        node_name = type(node).__name__
        if node_name not in ALLOWED_NODE_TYPES and node_name not in {'Import', 'ImportFrom'}:
            raise ValueError(f"Disallowed AST node type: {node_name}")
        super().generic_visit(node)

    def visit_Import(self, node):
        # Allow only whitelisted modules
        for alias in node.names:
            mod = alias.name.split('.')[0]
            if mod not in ALLOWED_MODULES:
                raise ValueError(f"Import of module '{mod}' is not allowed.")
        # Continue validation of alias usage
        self.generic_visit(node)

    def visit_ImportFrom(self, node):
        # Allow only whitelisted modules
        mod = node.module.split('.')[0] if node.module else ''
        if mod not in ALLOWED_MODULES:
            raise ValueError(f"Import-from module '{mod}' is not allowed.")
        self.generic_visit(node)

    def visit_Attribute(self, node):
        # Prevent introspection on dangerous attributes
        if node.attr in BLOCKED_ATTRIBUTES:
            raise ValueError(f"Access to attribute '{node.attr}' is not allowed.")
        self.generic_visit(node)

    def visit_Call(self, node):
        # Ensure called function is allowed
        func = node.func
        if isinstance(func, ast.Name):
            if func.id not in ALLOWED_FUNCTIONS and func.id not in {'pd', 'np'}:
                raise ValueError(f"Call to function '{func.id}' is not whitelisted.")
        elif isinstance(func, ast.Attribute):
            # allow pd.* and np.*
            if (isinstance(func.value, ast.Name)
                    and func.value.id not in {'pd', 'np'}):
                raise ValueError(f"Calls on '{func.value.id}' are not allowed.")
        else:
            raise ValueError("Unsupported function call structure.")
        for arg in node.args:
            self.visit(arg)
        for kw in node.keywords:
            self.visit(kw.value)


def safe_execute(code_str: str, env: dict):
    """
    Parse, validate, compile, and execute code_str in env with module imports restricted to ALLOWED_MODULES.

    Raises:
        ValueError on disallowed syntax or imports.
    """
    tree = ast.parse(code_str, mode='exec')
    ASTWhitelist().visit(tree)
    compiled = compile(tree, filename='<user_code>', mode='exec')
    exec(compiled, env, env)
    return env

# Example usage:
if __name__ == '__main__':
    user_code = '''
import pandas as pd
from numpy import array

result = df[df['sub_lob_for_flash']=='REB']
arr = array([1,2,3])
result_df = result.groupby('geo')['exposure'].sum().nlargest(4)
'''
    import pandas as pd
    import numpy as np
    df = pd.DataFrame({
        'monthend_trend_date': ['2025-05-31','2025-05-31'],
        'sub_lob_for_flash': ['REB','REB'],
        'geo': ['A','B'],
        'exposure': [10,20]
    })
    env = {'pd': pd, 'np': np, 'df': df.copy()}

    safe_execute(user_code, env)
    print(env['result_df'])  # check the result
```
