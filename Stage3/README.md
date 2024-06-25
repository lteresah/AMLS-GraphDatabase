# Stage 3: Exploring the Neo4j Python API to remove dependency on Neo4j Desktop

As stated at the end of the Stage 2 [README](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage2/README.md), the motivation to move away from the Neo4j Desktop app and transition to python is to make progress towards a true touch-less data management system. This system is not compatible with a step that requires the user to manually open a desktop app.

## 3.1: Installing the Neo4j Python API

This section is written with the assumption that the reader is familiar with python and will not provide any step-by-step guidance on how to do python related actions.

The scripts uploaded to the Stage 3 folder of this repository were designed to run in a **conda environment** with the following packages installed:
1. **python** version 3.12.3
2. **pandas** version 2.2.1
3. **neo4j** version 5.20.0

Instructions on how to install the neo4j python driver can be found [here](https://neo4j.com/docs/python-manual/current/install/).

Code was written and compiled using **Visual Studio Code** with the Jupyter, Neo4j, and Python extensions installed.

For the purpose of having clear commentary and documentation, all the scripts are Jupyter Notebooks.

## 3.2: Testing the waters with simple read and write queries

The first stage of the python transition involved:
1. Learning how to connect to a database
2. Attempting simple write queries
3. Attempting simple read queries
4. Turning the results of the read query into a pandas dataframe

The jupyter notebook produced during this stage can be found [here](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage3/neo4jPythonAPI_Simple.ipynb). Additional steps describing how to run the script are included within.

## 3.3: Creating a python script that loads the entire FSLR database

Once I completed the first stage, I then attempted to load the entire FSLR database using the existing [CSV Version 2](https://github.com/lteresah/AMLS-GraphDatabase/tree/main/Stage2/CSVs_Version2) files.

#### Lesson 3.1: The Neo4j Python Driver cannot run more than one query in a single transaction.

I ran into some difficulty because my CYPHER script was one giant block of code containing dozens or more queries, and the method by which I was running transactions did not allow for more than one at a time. There exists ways to run multiple, but I have not yet explored those avenues.

My solution was to separate each of the queries into independent scripts and store them in a strategically organized main folder. I then wrote a jupyter notebook script that enters each folder/subfolder in the main folder and executes every .cypher script it finds.

The notebook can be found [here](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage3/neo4jPythonAPI_FixedCSV_GH.ipynb). In order to run it, you need to download the [folder](https://github.com/lteresah/AMLS-GraphDatabase/blob/main/Stage3/FSLR_Model.zip) containing all the .cypher scripts.  Inside the notebook, change the variable named _directory_ to the appropriate folder path on your computer. Follow any other instructions outlined in the notebook.
