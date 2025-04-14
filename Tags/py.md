
if you have access to a code editor then run the following command to show items from db.

```
print([(user.id, user.username, user.password) for user in User.query.all()])
```

https://app.hackthebox.com/machines/Code