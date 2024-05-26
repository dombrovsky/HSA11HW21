# HSA11HW21

Created table on master with following structure:
```
CREATE TABLE IF NOT EXISTS Users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birth_date DATE NOT NULL
);
```
## Scenario 1
1. `stop slave;`
2. Drop last column: `ALTER TABLE Users DROP COLUMN birth_date;`
3. `start slave;`
   
**Result:** Replication continues to sync data from master (insers, updates, deletes), except that slave doesn't have last column anymore.

## Scenario 2
1. `stop slave;`
2. Drop column in the middle: `ALTER TABLE Users DROP COLUMN last_name;`
3. `start slave;`
   
**Result:** Replication stops working. Errors in log on attempt to start slave:
```
[ERROR] Slave SQL for channel '': Column 2 of table 'mydb.Users' cannot be converted from type 'varchar(50(bytes))' to type 'date', Error_code: 1677
[ERROR] Error running query, slave SQL thread aborted. Fix the problem, and restart the slave SQL thread with "SLAVE START". We stopped at log 'mysql-bin.000003' position 7149.
```
