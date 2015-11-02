# How to develop CEP Queries within the HEADS IDE
This section describes how to get started with complex event processing (CEP) development and how to use the recommender to partition queries into smaller queries for deployment to distributed services. 
## How to use the Apama Studio
There are four components in Apama Studio for CEP development: The EPL Editor and debug environment, the Query Builder, the Event Modeler, and the Dashboard Builder. This How To describes the preferred components to use inside the HEADS IDE.

### Components of Apama Studio
 
- Developing EPL with Apama Studio: EPL Editor

To get started with EPL inside the Apama Studio use the tutorial on EPL monitor scripts. In Apama Studio open the Welcome page, go to the Tutorials section. There are three tutorials. The last one with the title 'Developing an Application with MonitorScript' is the most important one. It explains the Apama Event Programming Language (EPL), called "MonitorScript". A Cheat Sheet guides you through this tutorial. 

In addition there are several samples introducing topics of Apama. In Apama Studio open the Welcome page, go to the Samples section. These six samples can be imported into an Eclipse workspace as Apama projects. From the Apama Workbench perspective you can launch these samples, one at a time. The Readme files suggest small exercises to modify these samples. The source of the samples is the folder 'demos' in the Apama installation. The folder 'samples' contains more in-depth examples for components of Apama and the connection with other products like Universal Messaging or Terracotta BigMemory.

Steps to perform for a sample:

* Import project into workspace
* Explore files in the Apama Workbench perspective. Toggle on the 'Show All Folders' button in the toolbar of the 'Workbench Project View'.
* Launch the sample with the 'Launch Control Panel'. This starts a correlator (Apama CEP runtime process) listening on port localhost:15903 (default). 
In addition a dashboard viewer is started in a separate window and events are sent to the correlator. 

There are three perspectives in Apama Studio, Apama Developer, Apama Runtime, and Apama Workbench. Apama Developer is similar to the Java perspective in JDT. Apama Workbench is a simplified combination of Apama Developer and Apama Runtime. It is tailored to work on a single Apama project. This can be selected in the Workbench Project View.

- Using the Query Builder in Apama Studio

The Query Builder is a combined graphical and textual editor for Apama Queries. These queries are new with Apama 5.3 and they allow a purely descriptive way to define stream queries. These queries are capable of large time windows.
 
- Event Modeler

This component helps to model state machines in EPL. This is not the focus inside the HEADS IDE and therefore not described in detail. Refer to the Apama documentation which is included in the Eclipse Help.  

- Dashboard Builder

The dashboard builder is used for the development of dashboard to visualize stream data. This is not part of the HEADS IDE and not in the focus of HEADS.  

## How to use the CEP Recommender
Suppose a service developer implements an EPL query. This query can be partitioned into parts which define new streams of events which are filtered, joined, projected and otherwise transformed. The parts of the query can run on other nodes than one central CEP node which runs on a powerful machine. With the help of platform experts the service developer can run the CEP Recommender to get a recommendation for a set of EPL queries which run on several nodes according to their CEP capabilities. The platform experts have to describe the CEP capabilities of the nodes, the CPU capacities. In addition the service developer has to describe the CPU capacity consumption of the operations in the query and the event rates on the input streams and the selectivity of filter operations.

With this information the CEP Recommender calculates recommended partitions with model transformations through Triple Graph Grammars. The algorithmically steps are 
* Build the syntax model, a kind of an abstract syntax tree from the EPL syntax.
* Build the Query Operator Tree with Xtend from the syntax model.
* Transform the Query Operator Tree with eMoflon using Triple Graph Grammars into the Meta Model. The Meta Model adds information for the partitioning to the Query Operator Tree.
* Transform the Meta Model with eMoflon using Triple Graph Grammars into a partitioning of the query. This partitioning is one of a list of partitionings since the Tripple Graph Grammar transformation is not unique. 
The result of this algorithm is a list of recommendations, each partitioning the original query into a set of simpler queries. The user has to decide, which partitioning fits best.
Each partitioning is a model instance, containing a partitioned query, i.e. a set of simpler queries which have together the same functionality than the input query. Such a set of queries can be deployed to the nodes. 

This approach is also described in deliverable D4.2, section 4.2.

The steps for the different roles in the HEADS IDE are:
* The service developer develops query logic (EPL text, Apama query language is not implemented yet). This is the main input, which is put into a text file with the extension epl.
* The service developer starts with the context menu of this file, entry CEP Recommender, the algorithm. The result is the Meta Model, which is visible in the Meta Data Editor.
* In this editor, the service developer adds statistical data: event rates, event sizes, selectivity of filters, etc.
* In the same editor the platform expert provides nodes info: CPU capacity of the nodes and CEP capabilities of the nodes.
* When all the input is collected the service developer or the platform expert starts the generation of the recommendations with the same context menu on the epl file. The recommendations are in a folder besides the epl file.

The service developer or the platform expert has to decide which recommendation fits best to be deployed on the distributed nodes.  

- Future developments for the rest of the project

The CEP Recommender should also work on the Apama query language, which is a feature first included in Apama 5.3. We will work on the EPL generation of the EPL queries of a recommendation. In addition automatic deployment to the distributed nodes is a topic for the future development.