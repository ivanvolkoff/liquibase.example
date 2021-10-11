# Liquibase

###
Liquibase is a tool for tracking, managing and applying database schema changes. Liquibase supports JSON, YAML, XML, SQL and etc. formats. More information about Liquibase can be found [here](https://www.liquibase.org/)

## Dirigible
Dirigible are fully integrated with Liquibase. By changesets included in .changelog you can create, delete, update, insert tables and columns. You can even create rollback tags and rolback changes to desired point.
Below you can see example for working with liquibase in Dirigible.

Open Dirigible Database Perspective:


![image](/images/dirigible_start.png)

Here in SQL View  i will execute several queries to create table and make some entrie. 

```sql
CREATE TABLE TABLE_USERS (ID INT, FIRST_NAME VARCHAR(10), LAST_NAME VARCHAR(10), AGE INT, LAST_UPDATED TIMESTAMP);

INSERT INTO TABLE_USERS VALUES (1, 'MIKE','ORLANDO',29, '2016-06-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (2, 'JOHN','DOE',29, '2016-06-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (3, 'PETRA','STEVENSON',29, '2014-06-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (4, 'OLIVER','TWIST',29, '2017-06-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (5, 'TOM','SAWER',29, '2016-02-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (6, 'CHUCK','THOMES',29, '2019-06-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (7, 'PABLO','RAMIRES',29, '2016-06-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (8, 'DIRK','SHNIDER',29, '2021-06-22 19:10:25-07');
INSERT INTO TABLE_USERS VALUES (9, 'SUCRE','MACILEX',29, '2018-06-22 19:10:25-07');
```
![image](/images/sql.png)

Executing SQL scripts by pressing cmd+x(Mac OS), cntrl+x(Windows);
In Database expolrer on pressing refresh we can see our newly created table in Public schema.

![image](/images/databaseexplorer.png)

Table data can de seen by choosing "Show Content" on table. Results are shown in Result View.


Lets execute some .changelog file in our project.
Jump to Workbench perspective. Here you need to create file with **.changelog** extension.

```json

{
    "databaseChangeLog": [
         {
         "preConditions": [
                  {
                    "runningAs": {
                      "username": "SA"
                    }
                  }
                ]
              },
              {
                "changeSet": {
                  "id": "1",
                  "author": "iwolkow",
                  "changes": [
                    {
                      "createTable": {
                        "tableName": "person",
                        "columns": [
                          {
                            "column": {
                              "name": "id",
                              "type": "int",
                              "autoIncrement": true,
                              "constraints": {
                                "primaryKey": true,
                                "nullable": false
                              },
                              
                            }
                          },
                          {
                            "column": {
                              "name": "firstname",
                              "type": "varchar(50)"
                            }
                          },
                          {
                            "column": {
                              "name": "lastname",
                              "type": "varchar(50)",
                              "constraints": {
                                "nullable": false
                              },
                              
                            }
                          },
                          {
                            "column": {
                              "name": "state",
                              "type": "char(2)"
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              },
              {
                "changeSet": {
                  "id": "2",
                  "author": "iwolkow",
                  "changes": [
                    {
                      "addColumn": {
                        "tableName": "person",
                        "columns": [
                          {
                            "column": {
                              "name": "username",
                              "type": "varchar(8)"
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              },
              {
                "changeSet": {
                  "id": "3",
                  "author": "iwolkow",
                  "changes": [
                    {
                      "addLookupTable": {
                        "existingTableName": "person",
                        "existingColumnName": "state",
                        "newTableName": "state",
                        "newColumnName": "id",
                        "newColumnDataType": "char(2)",
                        
                      }
                    }
                  ]
                }
              }
            ]
          }
```

On saving the file DIrigible automatically detects and executes the .changelog file.
Switching back to DB perspective we can see that in our PUBLIC schema, are created two more tables:
- **Persons**,
 - **State** 
 
 and two more related to .changelog execution :
 - **DATABASECHANGELOGLOCK**

 - **DATABASECHANGELOG** , by choosing see content of this table in Result View are changesets
represented as entries:

![image](/images/changelog_table.results.png)













