+++
author = "Mahesh Singh"
title = "Chapter-02 Data model and query language"
date = "2021-02-06"
description = "Data model and query language"
tags = [
    "ddia", "notes",
]
+++

## Table of content
* [Chapter 01 - Reliable Scalable and Maintainable Applications](../reliable-scalable-and-maintainable-applications)
* Chapter 02 - Data Model and Query Language
* Chapter 03 - Storage and Retrieval (coming soon...)

## Relational model vs Document model

Relational model: Data is organized into relations (tables), where each relation is an unordered collection of tuples(rows).

NoSQL - Not only SQL

**Need of NoSQL**
- Greater scalability
- Free and open source
- Desire for a more dynamic and expressive data model
- Schema flexibility

*Polyglot persistence*

**Many to One and Many to Many relationship**:
In both the model related items are referenced by unique identifier, which is called foreign key relational model and document reference in document model. Relational model requires join and document model requires follow-up queries to fetch the records.

**Schema on Read and Schema on Write**: Schema on read applies on document model where where there is not need to fixed schema definition in database. Where Scheme on Write applies to relational database where fixed scheme is required in database.

**Data locality for query**: There are time when large amount of data require on same time like while fetching product also fetch all the features of the product. Document model is more appropriate in this case. But on the same time if you needs few field than its a wasteful. Updating the large document usually also require the recreate the document. 
Now there are few relational databases which allowing same kind of features where table's rows should be nested within a parent table. 
Column family databases have similar purpose.

## Declarative vs Imperative (Query language for data)

**Imperative:** Imperative language tell the computer a certain operation in certain order. Most of the programming languages are imperative.

**Declarative:** Declarative query language like SQL, you only define pattern, condition and transformation (sorting, grouping etc). Query optimizer will decide how this data will be fetched. It hides implementation details and this helps query optimizer to introduce performance improvement without requiring any changes to queries. They can also perform better in parallel execution because queries only define pattern not the algorithm.

CSS, XSL are also declarative query languages.

***MapReduce:*** MapReduce is neither a declarative or fully imperative but somewhere in between, logic of query expressed with code snipped which is called repeatedly by processing. It is based upon map and reduce function.

## Graph like data model
Simple many to many relationship can handle by relational databases but if many to many relationship become complex in your data , it become natural to use graph based data model for your data.

Graph contains two kind of objects 
- **vertices:** Known as node or entities 
- **edge:** known as relationship
Examples: 
- ***Social graph*** where vertices are people and edges are relationship between people.
- ***The web graph*** where veritces are web pages and edges are link to other pages.
- ***Road or rail network*** where vertices define the junctions and edges define the lines between these junctions.

Graph data model is not limited to homogeneous data like FaceBook uses single graph database to store people, locations, events, check-ins etc.

> ***Graph databases are good for evolvability***

There are mainly two type of models used in graph bases data model.
1. **Property Graphs:** 
    - vertices consist of  
        + unique identifier, 
        + set of outgoing edges, 
        + set of incoming edges and collection of properties (key, value pair). 
    - Edges consist of 
        + unique identifier, 
        + the vertex at which the edge start (tail vertex), 
        + the vertex at which edge ends (head vertex), 
        + label to describe relationship between two vertices 
        + a collection of properties (key, value pair)
2. **Triple store:**
    All information store in form of very simple three part statements ***subject, predicate and object***.
    _:lucy a        :Person
    _:lucy :name    :"Lucy"
    _:lucy :bornIn  _:idaho
    _:idaho a       :Location

## Document databases vs Graph databases
- Document database target use cases where individual documents is self contains and relationship between documents are rare.
- Graph database target use case is in opposite direction and target use cases where anything is potentially connected to everything.
- Both don't require schema enforcement hence good for adopt change in requirement. 