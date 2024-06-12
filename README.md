# AMLS-GraphDatabase
Originally created by Teresa Le during the months of April, May, and June 2024.

### Motivation (in a few bullet-points):
- Document the journey toward implementing a touchless data management system, exploring the possibility of using a graph database.
- Determine if a graph database is the *best* option for academic, early development, and R&D settings.
- Teach lab members about Neo4j and the nuances involved with graph databases.
- Create a place to upload all related code for quick and easy adaptation by the lab in the future.

## Introduction:
Maintaining a data management system in an academic setting is very challenging. Until very recently, the most common practice in experimental labs was _individual ownership_ of data in the form of ASCII files, excel sheets, and other file types _stored in a personal computer or drive_. With the adoption of shared cloud drives such as DropBox, the community has largely reduced the risk of data being lost in an individuals drive, but there remains the problem that most data are uninterpretable for several reasons:
1. There is often a lack of clear direction regarding where to store data within a drive, so individuals will create different folders for the same project.
2. There is no agreement on how to name files or what to call variables, so individuals will name the same variable different things, and filenames do not clearly indentify the files' contents.
3. Data in a scientific, academic setting goes through several stages and transformations.  While the raw output of a measurement and the results of the analysis are often acessible, the process from raw to final result is often lost.
4. Since most of the information needed to navigate data meaningfully is locked inside the head of the individual who created it, high turnover rates of researchers, postdocs, and undergraduates exacerbate the problems created by the above points.

Even with today's practices, principle investigators will default to emailing past lab members when they want access to that member's data, because the task of finding data in a meaningful form remains daunting. A naive solution would be to establish a clear set of guidelines in the lab which includes where to store data, how to name files, what to name variables, and enforcing lab notebooks, but this solution relies on _human compliance_. Anecdotal evidence tells us that humans are not wired to maintain such high standards of organization, and such guidelines are often ineffective.

With automation on the rise, the vision for a flexible, touchless data-management system adapted to academic R&D has been promoted. Many millions of dollars have been spent with very little consensus on a solution to date. For context, a data management system in the academic R&D setting requires these properties:
1. FLEXIBILITY: The experimental design and process in this setting is always evolving, and the system needs to be able to _easily_ accept new types of processes and datatypes without compromising existing data.
2. QUICK AND EASY: If experimenters perceive that using the system greatly slows down their work with very little in return, they will not use the system.
3. ADVANTAGEOUS: For the work needed to maintain the system, it must prove more useful than the current practices when it comes to accessing and parsing data.
4. CHEAP: Academic labs typically are unwilling to spend hundrers of thousands of dollars on a data management system.

For the first two reasons above, a relational database, the most common type of database, is not suitable for the academic R&D setting, especially in the field of materials synthesis and characterization.

Our lab’s own experience trying to create a relational database, the Intermolecular database (ca. 2014), resulted in the rebellion of postdocs who reverted back to Google Sheets to record information on a case-by-case basis for experiments that distributed workflows across multiple people and sites.

Recently, a new and more flexible type of database, the **Graph Database**, has been suggested. One [study](https://lsidarto.github.io/perovskite-graph-database/) by David Fenning's lab at UCSD explored the process of using a graph database alongside their [PASCAL](https://pubs.rsc.org/en/content/articlelanding/2024/dd/d4dd00075g) system. Fenning's team compared the performance of a Neo4j graph database and a tabular, relational database under the same experimental context and reported:
- For complex (8+) joining, graph databases were significantly faster than relational databases.
- Relational databases outperform a graph databases when querying without any joins (which in practice would never be used).
- Relational databases lack flexibility: if a process were to be added, the database schema would have to be updated and all the data reinstantiated.
- Irreducable amounts of duplicate data and empty cells in a relational database due to varying numbers of manufacturing steps per sample.
  
Ultimately, the Fenning lab did not implement a graph database (is this true?) due to the memory restrictions of the free Neo4j database and the inability to find a simple method for uploading to the database from multiple computers.

This project takes inspiration from the work of David Fenning's team and attempts to explore the implementation of a graph database for our own lab's material synthesis and characterization processes. A key difference between our work and theirs is the immediate focus on designing a touch-less data management system rather than comparing the differences between a relational and graph database, although future work does not exclude exploration of this topic. This means the questions we are trying to answer are:
1. Is this database FLEXIBLE enough for our lab? For context, our lab is still working on designing workflows and automating our processes. The workflows are _constantly_ changing and there is still some reliance on manual work and data logging.
2. Can we make the data management process QUICK and EASY?
3. If so, what does that PROCESS look like?

## Stage 0: Choosing the most suitable provider for practice
### **Winner: Neo4j**

The choice was between **Neo4j** and **Amazon Neptune**.

Neo4j was the provider of choice for David Fenning's group and the first company that shows up when you google draph databases. Amazon Neptune is a service available through AWS and was frequently mentioned by the company contracted in 2024 to help AMLS build a data management system.

For the purposes of *learning* how to create a graph database for the first time, Neo4j was the clear choice because of these reasons:
1. It was the most well-known and well-established provider of a graph database at the time.
2. As a result, there existed extensive documentation as well as user-friendly tutorials.
3. At the time, Neo4j allowed for the creation of an *INDEFINITELY FREE* AuraDB cloud database which is limited in storage capacity, number of nodes, and number of relationships.
4. Amazon Neptune, on the other hand, was locked behind an AWS account and the free version was limited to a 1-month free trial.

## Stage 1: Learning how to use Neo4j and getting _any_ database running
### 1.1: Creating a Neo4j account, Starting an AuraDB Instance, Exploring Tutorial Videos
### 1.2: Practice Building a Database with Neo4j Workshop
### 1.3: Discovering the Limitations of Neo4j Workshop

## Stage 2: Using Neo4j Desktop for more advanced queries

## Stage 3: Exploring the Neo4j Python API to remove dependency on Neo4j Desktop

## Stage 4: Imagining a python algorithm that accepts variable data from CSVs

