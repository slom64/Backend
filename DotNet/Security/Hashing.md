
```csharp
var hashedPassword = new PasswordHasher<User>()
	.HashPassword(user, usernamePassword.Password);
user.PasswordHash = hashedPassword;
```

Why Microsoft did this (over-engineering):

| Reason                   | Explanation                                                   |
| ------------------------ | ------------------------------------------------------------- |
| **Custom salt per user** | Some hashers use `user.Id` or `user.Email` as additional salt |
