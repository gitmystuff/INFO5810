# Setting Up the Bob/Alice Graph in Neo4j

A small knowledge graph used to demonstrate Graph RAG -- grounding a language
model's answers in facts retrieved from Neo4j, instead of letting it guess.
This file covers only the graph itself, working directly in Neo4j Browser.
For running the full Python + Groq demo on top of this graph, see
`neo4j-demos-setup.md`.

## Why Bob and Alice?

The graph includes a few specific, made-up facts -- a company name, a job
title, a dollar figure -- that no language model could possibly know from its
training data. That's deliberate: if a grounded answer comes back correct,
it's proof the retrieval step actually worked, not a lucky guess from
something the model memorized.

## Starting fresh

If you want an empty database first, run one of these in Neo4j Browser:

```cypher
// Removes only this graph's nodes (safe to run alongside other data, e.g. the Movie Graph)
MATCH (e:Entity) DETACH DELETE e
```

```cypher
// Removes everything in the database, regardless of label
MATCH (n) DETACH DELETE n
```

Confirm it's empty with:

```cypher
MATCH (n) RETURN count(n)
```

## Step 1: Create the graph

Paste this into Neo4j Browser's query bar and run it:

```cypher
CREATE (_860_million:Entity {name: "$860 million"})
CREATE (n_2014:Entity {name: "2014"})
CREATE (Alice:Entity {name: "Alice"})
CREATE (Bob:Entity {name: "Bob"})
CREATE (Fenwick_Restoration_Studio:Entity {name: "Fenwick Restoration Studio"})
CREATE (Lead_Conservator:Entity {name: "Lead Conservator"})
CREATE (Leonardo_da_Vinci:Entity {name: "Leonardo da Vinci"})
CREATE (Louvre:Entity {name: "Louvre"})
CREATE (Meridian_Analytics:Entity {name: "Meridian Analytics"})
CREATE (Mona_Lisa:Entity {name: "Mona Lisa"})
CREATE (Paris:Entity {name: "Paris"})
CREATE (Senior_Data_Architect:Entity {name: "Senior Data Architect"})
CREATE (Vinci:Entity {name: "Vinci"})
CREATE (the_2019_Louvre_Digital_Archives_Conference:Entity {name: "the 2019 Louvre Digital Archives Conference"})

CREATE (Bob)-[:interestedIn]->(Mona_Lisa)
CREATE (Bob)-[:knows]->(Alice)
CREATE (Bob)-[:livesIn]->(Paris)
CREATE (Bob)-[:worksAt]->(Meridian_Analytics)
CREATE (Bob)-[:hasRole]->(Senior_Data_Architect)
CREATE (Bob)-[:metAliceAt]->(the_2019_Louvre_Digital_Archives_Conference)
CREATE (Mona_Lisa)-[:paintedBy]->(Leonardo_da_Vinci)
CREATE (Mona_Lisa)-[:locatedIn]->(Louvre)
CREATE (Mona_Lisa)-[:insuredFor]->(_860_million)
CREATE (Alice)-[:interestedIn]->(Mona_Lisa)
CREATE (Alice)-[:worksAt]->(Fenwick_Restoration_Studio)
CREATE (Alice)-[:hasRole]->(Lead_Conservator)
CREATE (Leonardo_da_Vinci)-[:bornIn]->(Vinci)
CREATE (Louvre)-[:locatedIn]->(Paris)
CREATE (Meridian_Analytics)-[:foundedIn]->(n_2014)
```

That's 14 entities and 15 relationships. No existing data is touched -- every
node uses the `:Entity` label, which doesn't collide with labels used
elsewhere (for example, the Movie Graph tutorial's `:Movie` and `:Person`
labels), so this is safe to run even if other data is already loaded.

## Step 2: Quick check

Confirm one node loaded correctly:

```cypher
MATCH (p:Entity {name: "Bob"}) RETURN p
```

This should return one node, labeled "Bob," viewable in Graph view.

## Step 3: Set the retrieval parameter

This tells the next query which entity to look up:

```cypher
:param entity_name => "Bob"
```

## Step 4: Run the retrieval query

This is the same query `graph_rag.py` runs under the hood -- given an entity
name, it finds every fact directly connected to it, in either direction:

```cypher
MATCH (e:Entity {name: $entity_name})-[r]-(other:Entity)
RETURN
  CASE WHEN startNode(r) = e THEN e.name ELSE other.name END AS subject,
  type(r) AS predicate,
  CASE WHEN startNode(r) = e THEN other.name ELSE e.name END AS object
```

**Expected result: 6 rows.**

| subject | predicate | object |
|---|---|---|
| Bob | hasRole | Senior Data Architect |
| Bob | interestedIn | Mona Lisa |
| Bob | knows | Alice |
| Bob | livesIn | Paris |
| Bob | metAliceAt | the 2019 Louvre Digital Archives Conference |
| Bob | worksAt | Meridian Analytics |

If you get those same 6 facts, the graph is loaded correctly and the
retrieval query is working as expected.

## Trying a different entity

Change the parameter and re-run the Step 4 query -- for example:

```cypher
:param entity_name => "Mona Lisa"
```

This should return 5 facts instead: `interestedIn` (from both Bob and Alice),
`insuredFor`, `locatedIn`, and `paintedBy`.
