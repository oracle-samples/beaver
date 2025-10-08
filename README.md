# Beaver (Oracle Conversion)

Converted version of the Beaver dataset for Oracle. This repository contains only data and usage notes. 

## Installation

No software installation is required for this repository.
Use the files directly with your existing Oracle tools and environment

## Documentation

### Dataset contents
`beaver_ddl.json` - 475 converted DDL objects (one JSON object per line)

`beaver_dml.json` - All converted DML statements in a single JSON file.

`beaver_nl2sql.jsonl` - All 209 natural-language to Oracle SQL pairs (one JSON object per line)

### How to use the data with Oracle
You can load the dataset in either of the following ways

#### 1) SQL client workflow (SQLcl / SQL*Plus )
* Extract the SQL text from each JSON record using your preferred tooling (for example, a JSON prorcessor or internal script)
* Apply DDLs from `beaver_ddl.jsonl` first to create the required objects, then apply DMLs from `beaver_dml.json` to populate the data
* Adjust schema / object names before execution to match your environment if needed and run the statements as standard SQL scripts

#### 2) Programatic workflow (your own script)
* Write a script (example, in Python) that:
    * Reads each json / jsonl record
    * Retrievs the SQL text 
    * Optionally transforms it (example, schema prefixes, data literal handling)
    * Executes statements against your Oracle database

Each line of `beaver_ddl.jsonl` is a JSON object that includes a field containing the DDL SQL.

* Extract that field with your preferred JSON tooling / process and execute with your Oracle client (example, SQLcl, Sql Developer Web)

* Or, write a python script to read each record, retrieve SQL text from the fiel, optionally transform it (eg. sche)

## Notes and Guidance
* For date/time strings, don't rely on session NLS. Detect date-like literals (like `2015-02-03 20:02:07`) in the DMLs and wrap with `TO_DATE(...,'YYYY-MM-DD HH24:MI:SS')` or use `TO_TIMESTAMP / TO_TIMESTAMP_TZ`

* Use `executemany` to run speed up bulk inserts and updates

* You may need to change schema names or add a prefix to table names to match your environment. Plan for consistent naming (for example, a staging prefix) before execution.

* Record every statement in a lightweight results file (table, SQL, status, and any error), and include a retry step that re-executes only the failures after you’ve fixed the underlying issue.


## Contributing

This project welcomes contributions from the community. Before submitting a pull request, please [review our contribution guide](./CONTRIBUTING.md)

## Security

Please consult the [security guide](./SECURITY.md) for our responsible security vulnerability disclosure process

## License

Copyright (c) 2025 Oracle and/or its affiliates.

Released under the Universal Permissive License v1.0 as shown at
<https://oss.oracle.com/licenses/upl/>.
