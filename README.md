```python
import os
import oracledb

# 1. Force settings directly in Python to rule out Environment Variable issues
os.environ['TNS_ADMIN'] = r'H:\tnsnames'
os.environ['KRB5_CONFIG'] = r'h:\kerberos\krb5.conf'
os.environ['KRB5CCNAME'] = r'C:\users\o739240\krb5cc_o739240'

print("--- DIAGNOSTICS ---")
print(f"TNS_ADMIN:   {os.environ.get('TNS_ADMIN')}")
print(f"KRB5_CONFIG: {os.environ.get('KRB5_CONFIG')}")
print(f"KRB5CCNAME:  {os.environ.get('KRB5CCNAME')}")

# Check if files actually exist
if not os.path.exists(os.path.join(os.environ['TNS_ADMIN'], 'sqlnet.ora')):
    print("!! WARNING: sqlnet.ora not found in TNS_ADMIN path !!")
if not os.path.exists(os.environ['KRB5CCNAME']):
    print("!! WARNING: Ticket cache file not found !!")

print("-------------------")

try:
    # externalauth=True is what triggers Kerberos
    conn = oracledb.connect(
        dsn="YOUR_TNS_ALIAS", 
        externalauth=True
    )
    print("Login Successful!")
    conn.close()
except oracledb.Error as e:
    error_obj = e.args[0]
    print("Login Failed!")
    print(f"Error Code: {error_obj.code}")
    print(f"Message:    {error_obj.message}")
```
