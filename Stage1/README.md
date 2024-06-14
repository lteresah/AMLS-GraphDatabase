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

**Database Model Version 0:**
 
My initial plan for the model consisted of nodes representing objects and relationships representing operations.  It would have looked something like this:
<p align="center">
<img  width="500" alt="FSLRModel1" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/1ac1748a-b465-4443-a365-aedf333f99ea">
</p>
 
#### Lesson 1.1: It is not a good idea to restrict the assignment of objects to nodes and operations to relationships.
I quickly scrapped the above idea. Doing this would cause a great amount of entropy in both domains. What about things that aren't exactly objects or operations? What if one person considers something an operation but another thinks its an object? Effecively navigating the database requires a solid understanding of the model, and the model above would make it very difficult to **query** data.

#### Lesson 1.2: Every node in the graph database needs to have its own unique identifier to reduce the risk of duplicate nodes.
If I decided to enforce barcoding of precursors in the lab, something that was already done with samples and chemicals, it would be natural to use the precursor's barcode as its unique identifier in the graph database. However, since each operation on the precursor creates a new node, that would require the precursor's barcode be _physically_ updated every time an operation is performed. This is unrealistic if done manually and still hard to do in an automated process.

**Database Model Version 1:**

After a lot of thought, I decided on a different model for the database, one that maximizes entropy in the node domain but restricts the relationship domain to representing the sequence of events (with a couple exceptions).  The new model looks very similar to but is different from the Fenning group's model and looked something like this:

<p align="center">
<img width="300" alt="FSLRModel1" src="https://github.com/lteresah/AMLS-GraphDatabase/assets/165841286/ed927354-6721-4325-8b5f-16805bbce938">
</p>

Having found no _critical_ issue with this model, I proceeded to try and upload my excel file and build the database.

### 1.2.2: Figuring out a CSV structure that will work with the database

My data started off as a [_single_ excel file]() containing columns indicating which ingredients I mixed, in what amount, the temperature of the hotplate when I heated the precursor, how long the precursor was heated, how long the precursor rested for, etc.  This, ultimately, could not be used to populate the database.

#### Lesson 1.3: The structure and information in my excel file were not compatible with the graph database
1. First off, you cannot upload an excel file to the Neo4j Workspace. You have to save it as a csv.
2. Second, when working on the original excel file, I did not think to create a unique identifier for _every single operation_ that I put the precursor through, but, alas, this model requires that.
3. Third, the import function relies on the column names of the CSV to help it populate nodes and map relationships. Having everything in one excel sheet made the mapping process somewhat confusing. I found myself mixing and matching columns from different operations in order to achieve the model I wanted. I also foresaw that this format was not going to be compatible with a process that was not fixed (i.e. missing operations or switching around the order).
4. Lastly, in order to track the order of operations on an object (make the THEN relationship), I needed to have a timestamp with higher resolution than just the date, because a precursor often goes through multiple operations in a single day.

**The better excel/csv structure:**

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

**CSV Structure Version 1:**

The CSVs will go through minor changes as I continue to explore the best way to implement the graph database, but the overall idea that each sheet should represent one operation type has remained the same, with the exception of a couple object types that have their own sheet.

### 1.2.3: Importing CSVs into Neo4j to build the database

## 1.3: Discovering the Limitations of Neo4j Workspace
