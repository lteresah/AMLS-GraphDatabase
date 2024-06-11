# AMLS-GraphDatabase
Originally created by Teresa Le during the months of April, May, and June 2024.

### Motivation (in a few bullet-points):
- Document the journey toward implementing a touchless data management system, exploring the possibility of using a graph database.
- Determine if a graph database is the *best* option for academic, early development, and R&D settings.
- Teach lab members about Neo4j and the nuances involved with graph databases.
- Create a place to upload all related code for quick and easy adaptation by the lab in the future.

## Introduction:
Maintaining a universal (or even lab-wide) data management system in an academic setting is very challenging. Until very recently, the most common practice in experimental labs was _individual ownership_ of data in the form of ASCII files, excel sheets, and documents stored in a personal computer or drive. Even with the adoption of shared drives such as DropBox, managing data created by different individuals remains a challenge, largely due to variance. Below are a couple of examples:
1. A lack of clear direction regarding where to store data within a drive (ex: individuals creating different folders for the same project)
2. No agreement on file names or variable names (ex: individuals naming the same variable different things, filenames that do not clearly identify their content)
3. Existence of analyzed or processed data associated with scripts located on another folder that cannot easily be traced back
4. High turnover rates of researchers, undergraduates, and sometimes graduate students exacerbating the problems created by the above points

Even with today's practices, principle investigators will default to emailing past lab members when they want access to that member's data, because the task of finding data in a meaningful form remains daunting. A naive solution would be to establish a clear set of guidelines in the lab which includes where to store data, how to name files, and what to name variables, but this solution relies on _human compliance_, asking humans to be extremely meticulous all the time, and anecdotal evidence tells us that humans are not wired to maintain such high standards of organization.

With automation on the rise, the vision for a flexible, touchless data-management system adapted to academic R&D has been promoted. Many millions of dollars have been spent with very little consensus on a solution to date. For context, a data management system in the academic R&D setting requires these properties:
1. FLEXIBILITY: The experimental design and process in this setting is always evolving, and the system needs to be able to _easily_ accept new types of processes and datatypes without compromising existing data.
2. QUICK AND EASY: If experimenters perceive that using the system greatly slows down their work with very little in return, they will not use the system.
3. ADVANTAGEOUS: For the work needed to maintain the system, it must prove more useful than the current practices when it comes to accessing and parsing data.
4. CHEAP: Academic labs typically are unwilling to spend hundrers of thousands of dollars on a data management system.

For the first two reasons above, a relational database, the most common type of database, is not suitable for the academic R&D setting, especially in the field of materials synthesis and characterization.

Our lab’s own experience trying to create a relational database, the Intermolecular database (ca. 2014), resulted in the rebellion of postdocs who reverted back to Google Sheets to record information on a case-by-case basis for experiments that distributed workflows across multiple people and sites.

Recently, a new and more flexible type of database, the **Graph Database**, has been suggested. One [study](https://lsidarto.github.io/perovskite-graph-database/) by David Fenning's lab at UCSD explored the process of using a graph database alongside their [PASCAL](https://pubs.rsc.org/en/content/articlelanding/2024/dd/d4dd00075g) system. Fenning's team compared the performance of a Neo4j graph database and a tabular database under the same experimental context and reported:
- For complex (8+) joining, graph databases were significantly faster than relational databases.
- Relational database outperforms a graph database when querying without any joins (which in practice would never be used).
- Lack of flexibility in the relational database: if a new step were to be added, the database schema would have to be updated and all the data reinstantiated.
- Irreducable amounts of duplicate data and empty cells in a relational database due to varying numbers of manufacturing steps per sample.
  
Ultimately, the Fenning lab did not implement a graph database (is this true?) due to the memory restrictions of the free Neo4j database and the inability to find a simple method for uploading to the database from multiple computers.

This project takes inspiration from the work of David Fenning's team and attempts to explore the implementation of a graph database for our own lab's material synthesis and characterization processes. A key difference between our work and theirs is the immediate focus on designing a touch-less data management system rather than comparing the differences between a relational and graph database, although future work does not exclude this possibility. This means the questions we are trying to answer are:
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
