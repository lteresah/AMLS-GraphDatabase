# Stage 2: Using Neo4j Desktop to run advanced queries
## 2.1: Introduction to Neo4j Desktop
[Neo4j Desktop](https://neo4j.com/download/) is an application that you have to download. Aside from that, it is a more advanced and capable version of the Neo4j Workshop. Some of its key features include:

- Access to tutorials and example projects
- Tools for managing separate projects
- The ability to create _local_ databases, which can be quickly deleted and recreated
- The ability to connect to cloud databases inside the application
- The option to Upload/Save CYPHER scripts for quick use
- The Graph Apps Tab which includes:
  - Neo4j Bloom (Same as the "Explore" Tab in the Neo4j Workshop)
  - Neo4j Browser (A more advanced version of the "Query" Tab in Neo4j Workshop)
  - Other apps I never used...
 
## 2.2: Recreating the graph database (+ additional desired features) using the Neo4j Browser
### 2.2.1: Setting up the project space

The first thing I did was start a Project called the "FSLR Database" and initialized a _local_ database called "FSLR Local" where I could practice querying without worrying about messing up the AuraDB cloud database.

A [link](https://www.dropbox.com/scl/fi/l4vekjhoiz3oqql8fozh0/Neo4jDesktop.mov?rlkey=30asjugj3uq7nz61bqn5k7zvg&dl=0) to a video recreating how I went about this process is provided.

### 2.2.2: Using Neo4j Workshop to generate the CYPHER script

The next thing I did was figure out how to import my data using CYPHER code instead of Neo4j Workshop's Import Tab. It is highly recommended that you spend some time exploring the language yourself, as there is no good way for me to impart to you all the knowledge I gained scouring the [CYPHER documentation](https://neo4j.com/docs/cypher-manual/current/introduction/). However, a good way to start is to take advantage of the "Generate Cypher Script" option in Neo4j Workshop's Import Tab to generate a script for the database you created.

<p align="center">
<img width="1000" alt="GenerateCypher" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/3cde4740-2f65-4181-9a81-0038ad9e1056">
</p>

Again, this method only allowed me to recreate the part of the database that I was able to implement using Neo4j Workshop, and it does _not_ immediately run when copied and pasted into the Neo4j Browser. 

The following modifications are required in order to make the generated code run in Neo4j Browser:

#### Lesson 2.1: You need to create readable, _download or raw_ links to CSVs if you want to use Neo4j Browser to upload data.

And you need to reference the links to the CSVs by defining them as parameters using syntax such as:
    
        :param name_of_paramater => "insert_download_link_here";

#### Lesson 2.2: The generated code is meant to be run as an _implicit transaction_, and the default transaction type in Neo4j Browser is _explicit_.

The parts of the code that can only be run as implicit transactions need to be commented out and/or replaced. Namely, the lines at the end of a query that read:

     IN TRANSACTIONS OF 10000 ROWS;

need to commented out and replaced by a semicolon.

See documentary on [implicit vs explicit transactions](https://neo4j.com/docs/cypher-manual/current/introduction/cypher-neo4j/#cypher-neo4j-transactions).


### 2.2.3: Figuring out how to implement the :THEN relationship

Once I was able to recreate what I built with the Neo4j Workshop, it was time to figure out how to implement what the Workshop could not do. The steps I took were as follows:

1. Added secondary labels called "Operation" to all the operation nodes by changing all instances of

          MERGE (n: name_of_operation { `UID_property_name`: row.`row_label_containing_UID` })    
      to
  
          MERGE (n: Operation:name_of_operation { `UID_property_name`: row.`row_label_containing_UID` })
  
      Additional labels were added for certain operations.

2. Searched the internet for ideas on how to implement the time dependent relationship (:THEN).

3. Modified the code found toward the bottom of this [webpage](https://community.neo4j.com/t/how-to-create-relationships-between-consecutive-nodes-based-on-date-property/29372/7) to fit the needs of our database.

4. Added the block of code to the end of the script to run after all other nodes and relationships have been created.

#### CYPHER Script Version 2, CSV Files Version 2, and Database Model Version 3

The script went through several iterations before reaching its final form ([Version 2.0](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage2/Cypher%20Scripts/Load_FSLR_V2P0GH.cypher)).

It is meant to be run with [Version 2 of the CSV files](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Stage2/CSVs_Version2). (Removed redundant information about **OTC (Over the Counter) Ingredients** (formerly **Ingredients**) from the **Mix** operation and stored the information in a separate CSV file. Minor changes to column names and major changes to number of columns in certain CSVs.)

The FSLR database model (Version 3) produced by this script using this [perspective](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage2/Cypher%20Scripts/FSLR%20Perspective.json) is pictured below: 

<p align="center">
<img width="640" alt="FSLR_Model_Version3" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/1e60464d-dfb9-4a21-a3c2-107bf787f169">
</p>

The elements of this model include:
* Object Nodes:
  1. Users (Renamed from "Group Members")
  2. OTC Ingredients (Renamed from "Ingredients")
  3. Precursors
  4. Samples
* Operation Nodes:
  1. Mix (Make Precursors and Samples)
  2. Heat
  3. Rest
* Relationships:
  1. CREATED (Denotes creation of an object node by an operation)
  2. WENT_TO (Denotes an object being acted on, INGREDIENT_OF was consolidated with this relationship)
  3. EXECUTED (Indicates which user performed the operation)
  4. THEN (Tracks the chronological order of operations on a specific object)

## 2.3: Discussing the Limitations of this Method

At this point, we are able to upload data (from CSVs) into the database using the Neo4j Browser and generate Version 3 of the FSLR Database Model. The model includes all of the relationships we want to include and every node (up to sample creation) of the FSLR process. This is a great first step towards achieving the data management system we envision, but it is far from the flexible, touch-less, fast, and easy system that we need.

The next stage(s) of the development period will focus on addressing some of the below issues:

1. Reliance on Neo4j Browser -- a touch-less data management system cannot be achieved if it requires the opening of a desktop app and copying/pasting code into the Neo4j Browser. Stage 3 of the development process will experiment with the Neo4j Python API in order to transfer the upload process from the Neo4j Browser to a Python client or script.

2. Lack of Flexibility -- the current model uses _hardcoded_ CYPHER scripts written specifically for the CSVs they were meant to work with. This means that other users performing the same operations but tracking different variables will find themselves unable to use the code to upload their data. Stage 4 of the development process will work on reading CSVs with python and generating node properties based on the content in the CSVs.
