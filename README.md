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

Even with today's practices, principle investigators will default to emailing past lab members when they want access to that member's data, because the task of finding data in a meaningful form remains daunting. A naive solution would be to establish a clear set of guidelines in the lab which includes where to store data, how to name files, and what to name variables, but this solution relies on _human compliance_, asking humans to be extremely meticulous all the time, and anecdotal experience tells us that humans are not wired to maintain such high standards of organization.

With automation on the rise, the vision for a flexible, touchless data-management system adapted to academic R&D has been promoted. Many millions of dollars have been spent, with little consensus to date.


## Stage 0: Choosing the most suitable provider for practice
### **Winner: Neo4j**

The choice was between **Neo4j** and **Amazon Neptune**.

Neo4j was the provider of choice for David Fenning's group, whose [initial exploration](https://lsidarto.github.io/perovskite-graph-database/) of a graph database for their [PASCAL](https://pubs.rsc.org/en/content/articlelanding/2024/dd/d4dd00075g) system greatly inspired this work. Amazon Neptune is a service available through AWS and was frequently mentioned by the company contracted in 2024 to help AMLS build a data management system.

For the purposes of *learning* how to create a graph database for the first time, Neo4j was the clear choice because of these reasons:
1. It was the most well-known and well-established provider of a graph database at the time.
2. As a result, there existed extensive documentation as well as user-friendly tutorials.
3. At the time, Neo4j allowed for the creation of an *INDEFINITELY FREE* AuraDB cloud database which is limited in storage capacity, number of nodes, and number of relationships.
4. Amazon Neptune, on the other hand, was locked behind an AWS account and the free version was limited to a 1-month free trial.
