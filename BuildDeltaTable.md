# Delta Table Creation Process

## This repository contains code for setting up a Delta table in the `recon_beta2` schema. The process involves creating and manipulating tables in a Databricks environment. Ensure that these steps are followed carefully, and note that the first step (`DROP TABLE recon_beta2.params_csv`) should only be executed if this process is performed one or more times before.

## Steps

### 1. Drop Initial Table (Execute only once)
```sql
DROP TABLE recon_beta2.params_csv;
```
### 2. Create External Table (recon_beta2.params_csv1)
#### Create an external table (recon_beta2.params_csv1) to read data from an external CSV file.

```sql
CREATE TABLE recon_beta2.params_csv1
USING CSV
OPTIONS (
  'path' '/mnt/dbg2/dap_enterprise_scs_d/test/params.csv',
  'delimiter' '|',       
  'header' 'true',        
  'inferSchema' 'true'
);
```

### 3. Create Delta Table (recon_beta2.params_csv)
#### Create a Delta table (recon_beta2.params_csv) by selecting all data from the external table created in step 2.

```sql
CREATE TABLE recon_beta2.params_csv AS SELECT * FROM recon_beta2.params_csv1;
```

### 4. Drop External Table (recon_beta2.params_csv1)
#### Drop the external table (recon_beta2.params_csv1) as it is no longer needed.

```sql
DROP TABLE recon_beta2.params_csv1;
```