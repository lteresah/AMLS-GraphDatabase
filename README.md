# AMLS-GraphDatabase
Originally created by Teresa Le during the months of April, May, and June 2024.

### Motivation (in a few bullet-points):
- Document the journey toward implementing a touchless data management system, exploring the possibility of using a graph database.
- Determine if a graph database can be used in academic, early development, and R&D settings.
- Teach lab members about Neo4j and the nuances involved with graph databases.
- Create a place to upload all related code for quick and easy adaptation by the lab in the future.

## Table of Contents:
* [**Introduction**](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Introduction#introduction)
* [**Stage 0:** Choosing the most suitable provider for practice](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Stage0#stage-0-choosing-the-most-suitable-provider-for-practice)
* [**Stage 1:** Learning how to use Neo4j and getting _any_ database running](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Stage1#stage-1-learning-how-to-use-neo4j-and-getting-any-database-running)
  - 1.1: Creating a Neo4j account, Starting an AuraDB Instance, Exploring Tutorial Videos
  - 1.2: Practice Building a Database with Neo4j Workshop
  - 1.3: Discovering the Limitations of Neo4j Workshop
* [**Stage 2:** Using Neo4j Desktop for more advanced queries](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Stage2#stage-2-using-neo4j-desktop-to-run-advanced-queries)
  - 2.1: Introduction to Neo4j Desktop
  - 2.2: Recreating the Graph Database (+ additional desired features) using the Neo4j Browser
  - 2.3: Discussing the Limitations of this Method
* [**Stage 3:** Exploring the Neo4j Python API to remove dependency on Neo4j Desktop](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage3/README.md#stage-3-exploring-the-neo4j-python-api-to-remove-dependency-on-neo4j-desktop)
  - 3.1: Installing the Neo4j Python API
  - 3.2: Testing the waters with simple read and write queries
  - 3.3: Creating a python script that loads the entire FSLR database
* [**Stage 4:** Imagining a python algorithm that accepts variable data from CSVs](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Stage4#stage-4-creating-a-python-algorithm-that-accepts-varying-data-from-csvs)
  - 4.1: Basic Structure of the Script
  - 4.2: More Detailed Information on Node and Relationship Generation
  - 4.3: Missing Features, Lessons Learned, and Recommended Next Steps

