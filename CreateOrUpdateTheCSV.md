## To update/create the CSV, there are three columns, named as PIPELINE_NAME, TARGET_TABLE, JSON_PARAMS.
### The <PIPELINE_NAME/> is the name of the pipeline with which the notebook is assocaited with.

### The <TARGET_TABLE/> is the name of the table whose params are being updates in the JSON to be dynamically allocated without any explicit definition.

### The <JSON_PARMAS/> contains the params in key-value pair structure where the key specifies the name of the variable to be used in the DB notebook and its value. The structure of the JSON is generally a non-nested one, for example:
```json
{"key1": ["a", "b", "c", "d"],"key2":"random string value"}
```
As the structure will depend on the variables being used in a notebook, it is advised not to use
nested objects in the JSON as the code to read the top-level JSON is tested and presented in the
assocaited documentaion, to use it in this structure will be easier.

### 1. Add rows in the CSV with the above rules

### 2. Upload the CSV in an ADLS folder.

### 3. Refer to the documentation [Build the Delta Table](BuildDeltaTable.md) for further steps.