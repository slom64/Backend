
> [!Danger]
> By default EF will drop the column "with its current data" and create another new empty column with the new name. To stop this from happening remove lines of `AddColumn`, `DropColumn` and replace them with `RenameColumn(TableName, OldColumnName,NewColumnName)`. 


> [!Danger]
> Don't forget to change the `Down()` method also you should do the opposite of `Up()` method.

