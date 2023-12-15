# Delta Table Creation Process

## This repository contains code for setting up a Delta table in the `schema_name` schema. The process involves creating and manipulating tables in a Databricks environment. Ensure that these steps are followed carefully, and note that the first step (`DROP TABLE schema_name.params_csv`) should only be executed if this process is performed one or more times before.

## Steps

### 1. Drop Initial Table
```sql
DROP TABLE IF EXISTS schema_name.target_table_params_csv;
```
If updating records, follow this step too
```sql
DELETE FROM schema_name.params_csv where target_table in ('your_target_table')
```
### 2. Create External Table (schema_name.target_table_params_csv)
#### Create an external table (schema_name.target_table_params_csv) to read data from an external CSV file.

```python
target_table_schema_param='schema_name'

target_table_config = spark.read.format('csv').option('delimiter','|').option('header','true').option('inferSchema','true').load('/mnt/path/to/target/table/csv')

target_table_config.write.format("delta").saveAsTable("schema_name.target_table_params_csv")
```

### 3. Update Delta Table (schema_name.params_csv)
#### Update the Delta table (schema_name.params_csv) by selecting all data from the external table created in step 2.

```sql
INSERT INTO schema_name.params_csv SELECT * FROM schema_name.target_table_params_csv
```

Refer to the documentaiton [Access Data In DB Notebook](DynamicParamsAllocation.md) for further steps.