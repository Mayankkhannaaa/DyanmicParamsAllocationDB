### This documentation outlines a Spark code snippet that involves transforming a DataFrame (df) obtained from a Delta table (schema_name.params_csv). The transformation includes cleaning up the JSON_PARAMS column by replacing double double-quotes with a single double-quote. The resulting DataFrame (result_df) is then collected, and specific information is extracted for each row.

## 1. Reading the Params

```python
df = spark.sql("select * from schema_name.params_csv where TARGET_TABLE IN ('target_table_name1', 'target_table_name2')")
```
The above statemet executes a SQL query to select all columns from the Delta table <schema_name.params_csv/> where the TARGET_TABLE is either 'target_table_name1' or 'target_table_name2'. The TARGET_TABLE filter will vary from notebook to notebook.


## 2. Column Transformation
```python
result_df = df.withColumn(
    "JSON_PARAMS",
    regexp_replace(col("JSON_PARAMS"), '""', '"')
)
```

Cleans up the JSON_PARAMS column by replacing occurrences of double double-quotes with a single double-quote to make it a readable JSON.

## 3. Data Collection
```python
collected_df = result_df.collect()
```
Collects the transformed DataFrame into a local Python list to be able tro iterate through the target tables.

## 4. Iteration and Data Processing
The iteration before was on the basis of hard-coded target_table_list list in python, and explicit
declaration of variables.
```python
for each_target_table in target_table_list:
    enterprise_table_fields=[]
    enterprise_table_schema=''

    if each_target_table == 'target_table_name2':
        pk_columns_list = "pk_columns_list_of_target_table_name2_explicitly_declared"
        final_table_target_schema = loaded_json["final_table_target_schema_of_target_table_name2_explicitly_declared"]
    elif each_target_table == 'target_table_name1':
        pk_columns_list = loaded_json["pk_columns_list__of_target_table_name1_explicitly_declared"]
        final_table_target_schema = loaded_json["final_table_target_schema_of_target_table_name1_explicitly_declared"]
```

It is replaced by one single loop to read the data and initiate the driver code as well.
```python
for row_params in collected_df:
    enterprise_table_fields=[]
    enterprise_table_schema=''
    json_params = row_params['JSON_PARAMS']
    json_params = json_params.strip('"')
    each_target_table = row_params["TARGET_TABLE"]
    loaded_json = json.loads(json_params)
```
The if else tree is no more needed, present just for readability.
```python
    if each_target_table == 'target_table_name2':
        pk_columns_list = loaded_json["pk_columns_list"]
        final_table_target_schema = loaded_json["final_table_target_schema"]
    elif each_target_table == 'target_table_name1':
        pk_columns_list = loaded_json["pk_columns_list"]
        final_table_target_schema = loaded_json["final_table_target_schema"]
```
Iterates through each row in the collected DataFrame.
Extracts information such as JSON_PARAMS, TARGET_TABLE, and loads the JSON content using json.loads.
Processes data based on the value of TARGET_TABLE, extracting specific information like pk_columns_list and final_table_target_schema.
```