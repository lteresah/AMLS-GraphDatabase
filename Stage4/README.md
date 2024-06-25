# Stage 4: Creating a python algorithm that accepts varying data from CSVs

The motivation for this stage is to transition away from hardcoded scripts that only accept CSV files with a fixed structure, including specific column numbers and names. 

_Please note that I am still using CSVs because the data I currently have is stored in CSV format. The code takes these CSVs and converts them into a Python list. In the future, automated machines will only need to output a Python list with the necessary information for integration._

This code is designed with the following goals in mind:

1. **Data Input**: The script takes in data from the automated experiment as a Python list.
2. **Node and Relationship Generation**: It dynamically generates the appropriate nodes and relationships based on the type of operation.
3. **Flexible Node Properties**: Apart from a few required properties needed to identify and create nodes/relationships, the system allows node properties to be filled with any data.
4. **Real-Time Generation**: Nodes and relationships can be generated in real time or shortly after the experiment, without the need to re-upload the entire database.

## 4.1: Basic Structure of the Script

The [Jupyter Notebook](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage4/neo4jPythonAPI_DynamicCSV.ipynb) provided populates the database following these steps:

1. Reads in csv files from the internet and turns each into a pandas dataframe
2. Takes each pandas dataframe associated with an Operation Node, extracts the data from the able, and creates a chronologically ordered **Operation Log** which takes the form of a python list.

    The list has the following structure:
  
    | Operation Type (string) | Timestamp (datetime) | User (string) |  Operation ID (string) | Data (dict) |
    |:--------------:|:---------:|:----:|:-------------:|:----:|
    | 'Mix' |'2024-03-13T12:00:00'|'lteresa'| 'MIX-2024-03-13T12:00:00-lteresa:01-314024and01-334090at20C| {'Mix Timestamp': '2024-03-13T12:00:00',  'Executor': 'lteresa', 'Process Owner': 'dasb', 'Mix Operation ID': 'MIX-2024-03-13T12:00:00-lteresa:01-314024and01-334090at20C', 'Mix Temp (C)': 20, 'Dissolved': 'Y', 'New Object ID': '02-000001', 'New Object Type': 'Precursor', 'New Object Description': '1M CsCH3COO in HBr', ...}|
    | ... | ... | ... | ... | ... | 

3. Establishes connection to a Neo4j Database.
4. Performs a sophisticated process to create nodes and relationships in the database.

## 4.2: More Detailed Information on Node and Relationship Generation

## 4.3: Lessons Learned and Recommended Work for the Future
