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

The next thing I did was figure out how to import my database using CYPHER code instead of Neo4j Workshop's Import Tab. It is highly recommended that you spend some time exploring the language yourself, as there is no good way to impart to you all the knowledge I gained scouring the [CYPHER documentation](https://neo4j.com/docs/cypher-manual/current/introduction/). However, a good way to start is to take advantage of the "Generate Cypher Script" option in Neo4j Workshop's Import Tab to generate a script for the database you created.

<p align="center">
<img width="1000" alt="GenerateCypher" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/3cde4740-2f65-4181-9a81-0038ad9e1056">
</p>

Again, this method only allows me to recreate the part of the database that I was able to implement using Neo4j Workshop, and it does _not_ immediately run when copied and pasted into the Neo4j Browser. 

The following modifications are required in order to make the generated code run in Neo4j Browser:

#### Lesson 2.1: You need to create readable, _download_ links to CSVs if you want to use Neo4j Browser to upload data.

And you need to reference the links to the CSVs by defining them as parameters using syntax such as:

    :param name_of_paramater => "insert_download_link_here";

#### Lesson 2.2: The generated code is meant to be run as an _implicit transaction_, and the default transaction type in Neo4j Browser is _explicit_.

The parts of the code that can only be run in implicit transactions need to be commented out and/or replaced. Namely, the line at the end of a query that iterates through the rows of a CSV

     IN TRANSACTIONS OF 10000 ROWS;

needs to commented out and replaced by a semicolon.
See documentary on [implicit vs explicit transactions](https://neo4j.com/docs/cypher-manual/current/introduction/cypher-neo4j/#cypher-neo4j-transactions).


### 2.2.3: Figuring out how to implement the :THEN relationship
