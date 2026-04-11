- If you want to do any custom changes you can use `Sql()` method to run raw sql statements.

| Action               | .NET CLI Command                  | Package Manager Console (PMC) |
| -------------------- | --------------------------------- | ----------------------------- |
| **Add Migration**    | `dotnet ef migrations add <Name>` | `Add-Migration <Name>`        |
| **Update Database**  | `dotnet ef database update`       | `Update-Database`             |
| **Remove Migration** | `dotnet ef migrations remove`     | `Remove-Migration`            |
| **List Migrations**  | `dotnet ef migrations list`       | `Get-Migrations`              |
| **Generate SQL**     | `dotnet ef migrations script`     | `Script-Migration`            |
| **Drop Database**    | `dotnet ef database drop`         | `Drop-Database`               |
