- When you have joins between multiple tables, make sure to use `.AsSplitQuery()`. Because if you didn't use it, you will have **Cartesian Explosion**.
	- When you use a single query with multiple joins, the database creates a "**Cartesian Product.**" It must create a unique row for every possible combination of your related data.
		- 1 (Product) × 5 (Images) × 10 (Variants) = **50 Rows of data.**
	- By using `.AsSplitQuery()`, we told the database: "Don't try to build that giant 50-row spreadsheet. Just give me three small, clean lists." **Now, for that same T-Shirt:**
		1. **Query 1:** Give me the Product info (**1 Row**).
		2. **Query 2:** Give me the 5 Images (**5 Rows**).
		3. **Query 3:** Give me the 10 Variants (**10 Rows**).