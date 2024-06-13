# Stage 1: Learning how to use Neo4j and getting _any_ database running
## 1.1: Creating a Neo4j account, Starting an AuraDB Instance, Exploring Tutorial Videos
To be able to follow along and run some of the code in this repository, it is highly recommended that you start your own, free Neo4j account and explore the tutorial videos in the **Neo4j Workspace**.  Rather than restating information that is already available, a [video](https://www.dropbox.com/scl/fi/vrilgctb5i738j1swkg98/StartingNeo4j.mov?rlkey=9o3db4bwy757o5k6hu76n5pbd&dl=0) has been created to help you get started. Since the video is (much) larger than 10 MB, it could not be uploaded to github, thus a link is provided.

## 1.2: Practice Building a Database with Neo4j Workspace
The application that you see when you "open" the AuraDB database is called the **Neo4j Workspace**, an easily accessible collection of dumbed down features available on the **Neo4j Desktop** application.

### 1.2.1: Using the import tab to upload CSVs and create a MODEL/SCHEMA for your database
Having previously had no experience working with a graph database and seeing that there was an option to import CSVs using the Workspace, I immediately attempted to upload my _one_ excel file I had been using and create a database based on that.

At this point of experimental process, we had only been doing _manual_ precursor preparation and solubility tests, so the elements I wanted to include in the database were limited to
* Objects:
  1. Ingredients purchased from a vendor
  2. Precursors
* Processes:
  1. Mix
  2. Heat
  3. Rest

## 1.3: Discovering the Limitations of Neo4j Workspace
