# Cancer Deep Phenotype Extraction (DeepPhe), Software Release

## Prerequisites

You must have the following tools installed to build this project:

- [Oracle Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [Maven 3.x](https://maven.apache.org/download.cgi)
- [neo4j 3.2.1](https://neo4j.com/) to store graph database output

The following tools are recommended
- [IntelliJ](https://www.jetbrains.com/idea/) IDE is suggested to run the pipeline
- [Eclipse](https://eclipse.org/ide/) is another IDE that can be used to run the pipeline

## Feedback
- Please post questions and comments on the [Issues tab](https://github.com/DeepPhe/DeepPhe-Release/issues) at the top of the page.
- Issues can be searched using the "Filters" tool.


## Obtaining the code

There are several methods that can be used to obtain the DeepPhe code from GitHub.
The three most common are:
1. Download using a browser a Zip file from [GitHub](https://github.com/DeepPhe/DeepPhe-Release/)
    1. [Download the Zip File](https://github.com/DeepPhe/DeepPhe-Release/archive/master.zip)
    2. Unzip the file
2. Clone using [Git](https://git-scm.com/)
    ````
    git clone https://github.com/DeepPhe/DeepPhe-Release.git
    ````
3. Checkout using [Subversion](https://subversion.apache.org/)
    ````
    svn co https://github.com/DeepPhe/DeepPhe-Release.git
    ````

## DeepPhe file structure

The DeepPhe root directory contains four Maven modules:
1. deepphe-ctakes-cancer
    - Code and resources to extend [cTAKES](https://ctakes.apache.org)
2. deepphe-distribution
    - Code and resources to create a binary installation
3. deepphe-fhir
    - Code to create [FHIR](https://www.hl7.org/fhir/) output
4. deepphe-uima
    - Code and resources to perform Document and Phenotype summarization

There is also a ``data/`` directory with subdirectories containing:
1. ``data/ontology/`` 
    - two cancer ontologies:
        1. Breast Cancer
        2. Melanoma
2. ``data/sample/reports/patientX/`` 
    - three example notes for a fictional patientX
3. ``data/pipeline/`` 
    - configuration files to run the pipelines

## Running the DeepPhe software

There are two main pipelines in the DeepPhe workflow:
1. Document Summarization
2. Phenotype Summarization

### Document Summarization
Document Summarization is run using a [Maven Profile](http://maven.apache.org/guides/introduction/introduction-to-profiles.html).

There are two predefined Document Summarization profiles:
1. startBreastCancer
2. startMelanoma

Each Profile will start a [Piper File Submitter GUI](https://cwiki.apache.org/confluence/display/CTAKES/Piper+File+Submitter+GUI) 
with default settings to run notes for [patientX](#deepphe-file-structure) with the appropriate ontology.
These settings include:
- Input Directory (with Notes) *
- Output Directory (for Document Summary files) *
- Lookup Xml (with Ontology parameters)
- Ontology Uri (for non-default specification)
- Sections Owl (for Summarization)

\* Unless you are using a custom ontology, you only need to change the Input and Output directories.

The GUI will allow you to load different pipelines, load and save custom pipeline parameters, and run pipelines.
See [Piper File Submitter GUI](https://cwiki.apache.org/confluence/display/CTAKES/Piper+File+Submitter+GUI) for more information.

Profiles can be run from the command line:
````
mvn compile -PstartBreastCancer
````
or using an IDE such as [IntelliJ with Maven integration](https://www.jetbrains.com/help/idea/maven.html#use_profiles_maven).

`mvn compile` will start the [Piper File Submitter GUI](https://cwiki.apache.org/confluence/display/CTAKES/Piper+File+Submitter+GUI).  
General instructions are in the lower half of the GUI and the green button at the upper-right will start the example pipeline. 
When the run starts the GUI will become shaded and the instructions at the bottom will be replaced by a running log.
When the run is complete the shading will lift and the log at the bottom will indicate completion.


### Phenotype Summarization
Phenotype Summarization is run using a [Maven Profile](http://maven.apache.org/guides/introduction/introduction-to-profiles.html).

There are two predefined Phenotype Summarization profiles:
1. startBreastCancerPhe
2. startMelanomaPhe

Each Profile will start a [Piper File Submitter GUI](https://cwiki.apache.org/confluence/display/CTAKES/Piper+File+Submitter+GUI) 
with default settings to run notes for [patientX](#deepphe-file-structure) with the appropriate ontology.
These settings include:
- Input Directory (Document Summarization Output Directory) *
- Output Directory (for Phenotype Summary files) *
- Configuration Directory (with TranSMART files)
- TCGA ID Mapping File (for tcga mapping)
- Sections Owl (for Summarization)

\* Unless you are using a custom ontology, you only need to change the Input and Output directories.

Note that the Input Directory for Phenotype Summarization is the Output Directory from the [Document Summarization](#Document Summarization) pipeline.
Typically you use the same directory for Phenotype Summarization output.

The GUI will allow you to load different pipelines, load and save custom pipeline parameters, and run pipelines.
See [Piper File Submitter GUI](https://cwiki.apache.org/confluence/display/CTAKES/Piper+File+Submitter+GUI) for more information.

Profiles can be run from the command line:
````
mvn compile -PstartBreastCancerPhe
````
or using an IDE such as [IntelliJ with Maven integration](https://www.jetbrains.com/help/idea/maven.html#use_profiles_maven).

`mvn compile` will start the [Piper File Submitter GUI](https://cwiki.apache.org/confluence/display/CTAKES/Piper+File+Submitter+GUI).  
General instructions are in the lower half of the GUI and the green button at the upper-right will start the example pipeline. 
When the run starts the GUI will become shaded and the instructions at the bottom will be replaced by a running log.
When the run is complete the shading will lift and the log at the bottom will indicate completion.


## Building a binary installation

````
mvn clean package -Dmaven.test.skip=true
````
	
The package command will create compressed files in the ``deepphe-distribution/target/`` directory.  
There are two compressed files containing binaries, one for Windows and another for Linux.  
Unzipping the file appropriate for your operating system will create a complete binary installation of DeepPhe.

If you are using a binary installation of DeepPhe, you can run pipelines using a gui by executing scripts from the command line:
````
bin/DocumentSummarizer
bin/PhenotypeSummarizer
````
Each command will start a [Piper File Submitter GUI](https://cwiki.apache.org/confluence/display/CTAKES/Piper+File+Submitter+GUI) 
set up for a [Document Summary pipeline](#Document Summarization) or [Phenotype Summary pipeline](#Phenotype Summarization).  
Unlike using the maven profiles, the GUI is launched without example settings for [patientX](#deepphe-file-structure) or an ontology.
You can load example settings using configuration files in the ``data/pipeline/`` [directory](#deepphe-file-structure) and modify them to fit your preferences.
Configuration files have the extension `.piper_cli`.  You can save your custom configuration files for reuse.

	
## Load data to neo4j

Run the phenotype level pipeline at least once so you have a [neo4jdb](https://neo4j.com/) folder generated at `DeepPhe/data/sample/output`.

Neo4j 3.2.1 uses `NEO4J_HOME/data/databases` to store all the databases. 
Copy your neo4j output directory `/path_to/patientX/ne04jdb` to `/path_to/neoj4j-3.2.1/data/databases/`.

You will also need to edit the `conf/neo4j.conf` to point to this `deepphe.db`:
````
dbms.active_database=deepphe.db
````

Then start neo4J 3.2.1 Server and point it to the neo4jdb folder. 
````
cd neo4j_home_directory
./bin/neo4j start
````
The above command is for Linux systems.  Windows does not require the `./` command prefix.
Once the neo4j server is running, you should be able to access the Neo4j browser at `http://localhost:7474/`  

Run the query `match (n) return n;` in the query entry field.  
You will see the contents of the graph database generated by the Phenotype Summarizer.
You can explore the generated neo4j database, execute Cypher queries and see results in tabular or graph form.


## Errata
When building the DeepPhe code, it uses the latest up-to-date cTAKES libraries.
If errors are encountered that indicate a problem in cTAKES, you can use a version of cTAKES tagged at the time of the DeepPhe 0.1 release:
[cTAKES for DeepPhe-Release](https://svn.apache.org/repos/asf/ctakes/tags/DeepPhe.checkpoint.v1/)

To run multiple patients, use a single root directory with subdirectories, one per patient, named after each patient.  
Files in patient directories should be prefixed with the patient name.
Document types can be differentiated using a suffix of `_RAD _SP _DS _PGN` or `_NOTE`.
For example:
````
reports/
     patientX/
          patientX_doc1_RAD.txt
          patientX_doc2_SP.txt
          patientX_doc3_NOTE.txt
````

Patient names should not contain the underscore _ character.
Document file names may contain the underscore _ character.
Specification of document type is not necessary, the default type is NOTE.

- Please post questions and comments on the [Issues tab](https://github.com/DeepPhe/DeepPhe-Release/issues) at the top of the page.
- Issues can be searched using the "Filters" tool.

##  Important License Notice
This release of DeepPhe contains a calculation of overall stage using TNM stage values.  More information on this is available elsewhere.  For example, [This pdf](https://cancerstaging.org/references-tools/deskreferences/Documents/AJCC%20Breast%20Cancer%20Staging%20System.pdf).  Utilization of this functionality may require that you obtain a new license from the AJCC.
