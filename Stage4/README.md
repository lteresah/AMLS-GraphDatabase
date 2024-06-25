# Stage 4: Creating a python algorithm that accepts varying data from CSVs

The motivation of this stage is to move away from hardcoded scripts that only accept CSVs with a fixed structure, including column numbers and names. 

It should be noted I am still using CSVs only because the (real) data I have at the moment is stored in CSV form. You will see that the code written takes those CSVs and turns them into a python list, so that integration in the future by automated machines need only output a python list with the necessary information.

## 4.1: Basic Structure of the Script

The [Jupyter Notebook](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage4/neo4jPythonAPI_DynamicCSV.ipynb) provided follows these steps to generate the database:

* Reads in csv files from the internet and turns each into a pandas dataframe
* Takes each pandas dataframe associated with an Operation Node, extracts the data, and creates a single python list acting as a large **Operation Log** that is chronologically ordered.

  The list has the following structure:

| Operation Type (string) | Timestamp (datetime) | User (string) |  Operation ID (string) | Data (dict) |
|:--------------:|:---------:|:----:|:-------------:|:----:|
| 'Mix' |'2024-03-13T12:00:00'|'lteresa'| 'MIX-2024-03-13T12:00:00-lteresa:01-314024and01-334090at20C| {'Mix Timestamp': '2024-03-13T12:00:00',  'Executor': 'lteresa', 'Process Owner': 'dasb', 'Mix Operation ID': 'MIX-2024-03-13T12:00:00-lteresa:01-314024and01-334090at20C', 'Mix Temp (C)': 20, 'Dissolved': 'Y', 'New Object ID': '02-000001', 'New Object Type': 'Precursor', 'New Object Description': '1M CsCH3COO in HBr', ...}|
| ... | ... | ... | ... | ... | 

* Establishes connection to a Neo4j Database.
* Executes a complex process to generate nodes and relationshps into the database.

## 4.2: More Detailed Information on Node and Relationship Generation

## 4.3: Lessons Learned and Recommended Work for the Future
