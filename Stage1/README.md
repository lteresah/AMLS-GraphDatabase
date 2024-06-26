# Stage 1: Learning how to use Neo4j and getting _any_ database running
## 1.1: Creating a Neo4j account, Starting an AuraDB Instance, Exploring Tutorial Videos
To be able to follow along and run some of the code in this repository, it is highly recommended that you start your own, free Neo4j account and explore the tutorial videos in the **Neo4j Workspace**.  Rather than restating information that is already available, a [video](https://www.dropbox.com/scl/fi/vrilgctb5i738j1swkg98/StartingNeo4j.mov?rlkey=9o3db4bwy757o5k6hu76n5pbd&dl=0) has been created to help you get started. Since the video is (much) larger than 10 MB, it could not be uploaded to github, thus a link is provided.

## 1.2: Practice Building a Database with Neo4j Workspace
The application that you see when you "open" the AuraDB database is called the **Neo4j Workspace**, an easily accessible collection of dumbed down features available on the **Neo4j Desktop** application. The **Import** tab of the Workspace allows users to click and drag their mouse to create  nodes and relationships and define the database **model**. Users can then upload CSVs into the application which can then be used to populate node properties and define how they connect.  This is the method by which I put together the first version of the database.

### 1.2.1: Coming up with a MODEL/SCHEMA for the database
At this point of the experimental process, we had only been doing _manual_ precursor preparation and solubility tests, so the elements I was considering were:
* Objects:
  1. Ingredients purchased from a vendor
  2. Precursors
* Operations:
  1. Mix
  2. Heat
  3. Rest

I prioritized modeling the database in a way that effectively mapped the chronological events of the synthesis process from precursor preparation to sample creation (when we eventually got there).

#### Database Model Version 0:
 
My initial plan for the model consisted of nodes representing objects and relationships representing operations.  It would have looked something like this:
<p align="center">
<img  width="500" alt="FSLRModel1" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/1ac1748a-b465-4443-a365-aedf333f99ea">
</p>
 
#### Lesson 1.1: It is not a good idea to restrict the assignment of objects to nodes and operations to relationships.
I quickly scrapped the above idea. Doing this would cause a great amount of entropy in both domains. What about things that aren't exactly objects or operations? What if one person considers something an operation but another thinks its an object? Effecively navigating the database requires a solid understanding of the model, and the model above would make it very difficult to **query** data.

#### Lesson 1.2: Every node in the graph database needs to have its own unique identifier to reduce the risk of duplicate nodes.
If I decided to enforce barcoding of precursors in the lab, something that was already done with samples and chemicals, it would be natural to use the precursor's barcode as its unique identifier in the graph database. However, since each operation on the precursor creates a new node, that would require the precursor's barcode be _physically_ updated every time an operation is performed. This is unrealistic if done manually and still hard to do in an automated process.

#### Database Model Version 1:

After a lot of thought, I decided on a different model for the database, one that maximizes entropy in the node domain but restricts the relationship domain to representing the sequence of events (with a couple exceptions).  The new model looks very similar to but is different from the Fenning group's model and looked something like this:

<p align="center">
<img width="300" alt="FSLRModel1" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/ed927354-6721-4325-8b5f-16805bbce938">
</p>

The elements of this model include:
* Object Nodes:
  1. Group Members (_Not shown_)
  2. Ingredients
  3. Precursors
* Operation Nodes:
  1. Mix (Make Precursors)
  2. Heat
  3. Rest
* Relationships:
  1. INGREDIENT_OF (Denotes objects being used in an operation)
  2. CREATED (Denotes creation of an object node by an operation)
  3. WENT_TO (Denotes an object being acted on)
  4. EXECUTED (_Not shown_, indicates which user performed the operation)
  5. THEN (Tracks the chronological order of operations on a specific object)

Having found no _critical_ issue with this model, I proceeded to try and upload my excel file and build the database.

Note: the database model will undergo changes over time including the consolidation of a couple relationship types and the addition of nodes, but the general idea that the majority of information should be represented by nodes while the relationships enable tracking of event streams remains the same.

### 1.2.2: Figuring out a CSV structure that will work with the database

My data started off as a [_single_ excel file](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage1/FSLR-OriginalExcel.csv) containing columns indicating which ingredients I mixed, in what amount, the temperature of the hotplate when I heated the precursor, how long the precursor was heated, how long the precursor rested for, etc.  This, ultimately, could not be used to populate the database.

#### Lesson 1.3: The structure of and information in my excel file were not compatible with the graph database
1. First off, you cannot upload an excel file to the Neo4j Workspace. You have to save it as a csv.
2. Second, when working on the original excel file, I did not think to create a unique identifier for _every single operation_ that I put the precursor through, but, alas, this model requires that.
3. Third, the import function relies on the column names of the CSV to help it populate nodes and map relationships. Having everything in one excel sheet made the mapping process somewhat confusing. I found myself mixing and matching columns from different operations in order to achieve the model I wanted. I also foresaw that this format was not going to be compatible with a process that was not fixed (i.e. missing operations or flexible order).
4. Lastly, in order to track the order of operations on an object (make the THEN relationship), I needed to have a timestamp with higher resolution than just the date, because a precursor often goes through multiple operations in a single day.

#### The better excel/csv structure:

I decided that it was better to split my excel sheet into several sheets, each sheet representing an operation. Rather than creating a single repository for all the information you need to know about a precursor, something most scientists would do, I created logs for each operation type with at least one column indicating which object(s) that operation was related to. This made mapping the csvs to their corresponding nodes and relationships much less convoluted and did not restrict the order or number of operations on a single precursor.

While the downside of this method involves losing the convenience of having all the information of a precursor in one excel sheet, something very valuable to researchers, it enables flexibility. Also, if the database is working as intended, you can extract precursors alongside every operation they ever went through in chronological order, so the information is not actually lost.

My next step after splitting the excel sheet was to populate the information that I was missing (i.e. unique identifiers and timestamps for each operation).

#### Lesson 1.4: Unique identifiers (UIDs) really matter
The unique identifiers for ingredients, precursors, and samples were chosen to match the barcoding practice of chemicals at MIT.
- Chemicals/Ingredients: 01-######
- Precursors: 02-###### (we don't actually have a policy to label our precursors)
- Samples: 03-###### (we have a barcoding policy of samples in our lab that _differs_ from this, and we need to align before the graph database is officially implemented)
  
I haphazardly came up with a structure for operation UIDs that I could automatically generate with excel based on their properties. My hope was that if I included enough properties, I could minimize the risk of different operations sharing the same UID. This, in turn, would reduce the risk of unintentionally overwriting a node with a new, unrelated node. While this method does reduce that risk, the risk of creating duplicate nodes increases as you include more properties. If a property included in the UID were to be corrected or updated, this would prompt the system to create a _new_ node for that operation without deleting the old one.

Therefore, my solution is _not_ ideal, but I have not gotten the opportunity to think about a better solution besides the possibility of using Universal Unique ID Generators.

To complete the timestamps, I simply guessed what hour of the day I performed the operation and then added an arbitrary amount of minutes/seconds to the time in order to maintain the correct order of operations.

#### **Database CSVs Version 1:**

The first set of CSVs to be successfully turned into a graph database is contained in [this folder](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Stage1/CSVs_Version1).

Note: you will see the CSVs go through minor changes as I continue to explore the best way to implement the graph database, but the overall idea that each sheet should represent one operation type, with the exception of a couple object types having their own sheet, has remained the same.

## 1.3: Discovering the Limitations of Neo4j Workspace

### 1.3.1: What it is able to do
#### **Database Model Version 2:**

Below is part of the database that I was able to create using the Neo4j Workspace:

<p align="center">
<img width="403" alt="FSLR_Model_Version2" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/f4e2d3d5-b439-4f7b-91db-f8996a19a4e1">
</p>

The elements of this model include:
* Object Nodes:
  1. Group Members (_Not shown_)
  2. Ingredients
  3. Precursors
  4. Samples
* Operation Nodes:
  1. Mix (Make Precursors and Samples)
  2. Heat
  3. Rest
* Relationships:
  1. INGREDIENT_OF (Denotes objects being used in an operation)
  2. CREATED (Denotes creation of an object node by an operation)
  3. WENT_TO (Denotes an object being acted on)
  4. EXECUTED (_Not shown_, indicates which user performed the operation)
  5. THEN (_Not shown_, tracks the chronological order of operations on a specific object)

The first difference you will notice from **Version 1** is the addition of another "Mix" node which leads to the creation of a "Sample". By the time I got to this point of the exploration, the FSLR team had started to make samples, and this was included to accomodate the changes.

#### Lesson 1.5: You need to spend time defining a Neo4j Perspective if you want the Bloom app to be interpretable.

You will also notice that the nodes in the explore tab (Bloom app equivalent) are nicely colored and labeled. This was not automatically generated -- the user has to spend several minutes specifying how they want the nodes to appear and what properties show up when they hover over the nodes. Once they do this, they have created what Neo4j calls a **perspective**, which can be exported as a _.json_ file and shared with other users who wish to have the same settings.

#### Lesson 1.6: Exporting a database does not export the Perspective -- it needs to be exported and shared separately.

Although I learned this much later on, I feel it is best to place this fact here. If you wish to share your database by exporting it as a _.dump_ file, the Perspective is not saved. If you want to share your database as you see it on the Bloom app, you need to export both a snapshot of the database as well as the Perspective.

### 1.3.2: What it is not able to do

While I was able to implement the majority of my desired model using the Neo4j Workshop, I was unable to find a way to add the most important feature: a relationship (:THEN) that tracks the chronological order of operations on an object node. To do this, one would need a way to disinguish object nodes from operation nodes _and_ only apply the relationship to operations that act on the same object _and_ only on particular types of objects. This level of detail required two things that the Workshop is unable to do:
1. Designate multiple labels to a single node.
2. Execute advanced CYPHER queries.

At this point in time, it was optimal to abandon Neo4j Workshop and pivot to **Neo4j Desktop**. The reasons are as follows:
1. The touch-less data management system was never going to use Neo4j Workshop. It cannot rely on navigating an internet browser and manually uploading CSVs through a GUI.
2. The user needs to become familiar with the CYPHER language if they are seriously thinking about implementing a touch-less data management system with it.
3. The act of importing your entire dataset to the database every time you update the database is obviously a timely and costly operation and should be avoided.
