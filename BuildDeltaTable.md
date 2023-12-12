# Delta Table Creation Process

## This repository contains code for setting up a Delta table in the `schema_name` schema. The process involves creating and manipulating tables in a Databricks environment. Ensure that these steps are followed carefully, and note that the first step (`DROP TABLE schema_name.params_csv`) should only be executed if this process is performed one or more times before.

## Steps

### 1. Drop Initial Table (Execute only once)
```sql
DROP TABLE schema_name.params_csv;
```
### 2. Create External Table (schema_name.params_csv1)
#### Create an external table (schema_name.params_csv1) to read data from an external CSV file.

```sql
CREATE TABLE schema_name.params_csv1
USING CSV
OPTIONS (
  'path' 'your/params.csv/mount/point/location',
  'delimiter' '|',       
  'header' 'true',        
  'inferSchema' 'true'
);
```

### 3. Create Delta Table (schema_name.params_csv)
#### Create a Delta table (schema_name.params_csv) by selecting all data from the external table created in step 2.

```sql
CREATE TABLE schema_name.params_csv AS SELECT * FROM schema_name.params_csv1;
```

### 4. Drop External Table (schema_name.params_csv1)
#### Drop the external table (schema_name.params_csv1) as it is no longer needed.

```sql
DROP TABLE schema_name.params_csv1;
```

Refer to the documentaiton [Access Data In DB Notebook](DynamicParamsAllocation.md) for further steps.