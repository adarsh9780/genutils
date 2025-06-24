```python
from nicegui import ui

# Persistent chat history container
chat_column = ui.column().classes('w-full max-w-2xl px-4')

# Main layout
with ui.column().classes('items-center justify-center min-h-screen'):
    ui.label("Where should we begin?").classes('text-2xl text-center my-10')
    chat_column  # Insert chat container into the layout

    def handle_submit(value: str):
        chat_column.add(
            ui.label(f'You said: {value}').classes('self-start bg-gray-100 rounded-xl p-3 my-1')
        )

    # Input field
    ui.input(placeholder='Ask anything...') \
        .props('rounded outlined') \
        .classes('w-full max-w-2xl px-4 mb-4') \
        .on('submit', handle_submit)

# Run the app
ui.run()
```
