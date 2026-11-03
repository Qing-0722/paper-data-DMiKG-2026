# Data for Knowledge Graph Construction

This repository contains the dataset used to build the knowledge graph (Neo4j) for our paper: **"[DMiKG: A Multidimensional Interaction Knowledge
Graph Platform for Depression]"**.

## 1. File Directory

Both files are located in the `data/` folder:

* **`entities.csv`**: The entity (node) table.
* **`relationships.csv`**: The relationship (edge) table.

## 2. Data Schema

### Entity Table (`entities_neo4j.csv`)
Each row represents a unique node. Key columns:
* `[pmid]`: The document numbers of the literature in the fields of life sciences and medicine that are included in the PubMed search engine.
* `[entity_text]`: The extracted entity names from the literature.
* `[your_type]`: Entity type.
* `[standard_id]`:Unique identifier for the entity.
* `[if]`: The impact factor of the literature.
* `[total_count]`: The total number of occurrences of this entity in the literature.

### Relationship Table (`relationships_neo4j.csv`)
Each row represents a directed edge. Key columns:
* `[pmid]`: The document numbers of the literature in the fields of life sciences and medicine that are included in the PubMed search engine.
* `[head_entity]`: The head entity name.
* `[head_type]`: The head entity type.
* `[relation]`: The relationship type (e.g., correlates_with, decreases).
* `[tail_entity]`: The tail entity name.
* `[tail_type]`: The tail entity type.
* `[head_standard_id]`: Unique identifier for the head entity.
* `[tail_standard_id]`: Unique identifier for the tail entity.
* `[if]`: The impact factor of the literature.
* `[total_count]`:The total number of occurrences of this triplet in the literature.
  
## 3. How to Import (Neo4j)
Example Cypher queries to load the data into Neo4j:

**Load Entities:**
```cypher
LOAD CSV WITH HEADERS FROM 'file:///entities_neo4j.csv' AS row
MERGE (n:Entity {id: row.[entity_text]})
SET n.name = row.[your_type]

**Load Relationships:**
```cypher
LOAD CSV WITH HEADERS FROM 'file:///relationships_neo4j.csv' AS row
MATCH (a:Entity {id: row.[head_standard_id]})
MATCH (b:Entity {id: row.[tail_standard_id]})
MERGE (a)-[:RELATIONSHIP]->(b)
