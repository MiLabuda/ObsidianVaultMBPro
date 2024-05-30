
## Indexes

Indexes makes faster our SELECT while slowing down other CRUD requests
Indexes are crated for the specific db columns 

Most popular implementation of indexes are B tree (Balanced tree) which gives us logarithmic search-time, other type is HashTable


B-Tree Index
```
CREATE INDEX idx_column_name ON table_name (column_name);
```
Use case: default index, most popular, 

Hash Index
```
CREATE INDEX idx_column_name ON table_name USING HASH (column_name)
```
Use case: Implemented similarly to HashTable (whatever that means)

For index B-Tree, when we are adding one we are creating trees
Indexes duplicates our data which enlarges our databases quickly

With indexes we have faster read actions to write actions

For example for 1 mln records putting an extra index shortens time for select 42 times, but it is slowing the save time 3 times, its because when we are adding/updating records we also need to update the indexes

cardinality(higher when values are unique, lower when values are not unique)

INDEXES MIGHT TAKE A LOT OF MEMORY
We should add indexes for 'reference tables'
currencies, currenciesCodes are great examples of a good index column, but course is not good example of a column for index.\

all PRIMARY KEYs are being indexed automatically


## Transactions

Transactions allow us to group SQL statements in order to perform bulk operations

This way we can implement batch processing.
transaction might look like this
- update inventory
- add order to order history
- deduct money from the bank account

we can use it for limiting number of database conection

```
BEGIN TRANSACTION;
INSERT INTO ....
UPDATE .....

COMMIT;
SOME OTHER STATEMENTS
for example loging incorrect object
ROLLBACK;

```


## Triggers

Trigger is an instruction for some method to take place in response to some event
i.e insert, update or delete done on db1 can be done also on db2

```
CREATE OR REPLACE FUNCTION prevent_department_update()
RETURNS TRIGGER AS SS
BEGIN
	IF NEW.department.id <> OLD. department_Id THEN
		RAISE EXCENPTION 'Updating department is not allowed';
	END IF;
	RETURN NEW;
END;

SS LANGUAGE plpgsql

CREATE TRIGGER department_update_prevention
BEFORE UPATE ON employees
FOR EACH ROW
EXECUTE FUNCTION prevent_department_update();
```


## Access Control

row-level security - restricts what records are available for which user
