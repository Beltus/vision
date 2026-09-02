---
layout: single
published: true
title: "Knowledge Graphs and GraphRAG: How Connected Facts Make AI Smarter"
collection: sc
author_profile: true
read_time: true
categories: [Blog]
excerpt: "
Traditional RAG is great at pinpointing specific chunks but poor at broad questions about an entire dataset. Here's how Knowledge Graphs and GraphRAG fix that — from RDF triples and ontologies to weaving graph search into your retrieval pipeline"
header:
    overlay_image: "https://beltus.github.io/vision/assets/images/knowledge_graphs_graphrag_blog_header.png"
    teaser: "https://cdn-images-1.medium.com/max/1200/1*cecsdvLJXw97A7f1w6T9pg.jpeg"
comments: true
toc:
toc_sticky:
---

# Knowledge Graphs and GraphRAG: How Connected Facts Make AI Smarter

If you build Retrieval-Augmented Generation (RAG) systems, you have probably run into a familiar wall. Your pipeline is excellent at pulling out a specific chunk of text when the question is precise, but it stumbles the moment someone asks something broad about the whole dataset. I have seen this repeatedly in the RAG pipelines I build in the health domain: the quality of a response depends heavily on how clear and specific the query is, and traditional systems struggle to reason across many documents at once.

That limitation led me to researching for potential solutions. That's when I noticed Microsoft researchers had observed a similar problem and proposed GraphRAG as a potential solution in their paper titled: [From Local to Global: A GraphRAG Approach to Query-Focused Summarization.](https://arxiv.org/abs/2404.16130).

 I was intrigued and decided to study GraphRAG with the goal of improving my own RAG systems. That led me to Knowledge Graphs, which play a critical role in GraphRAG. 
 
 This post is a walk-through of what I learned written the way I wish someone had explained it to me when I started.

## What Is a Knowledge Graph?

A knowledge graph (KG) is a structured representation of facts and relationships within a specific domain. The simplest way to picture it is as a web of connected facts: dots for the things, and lines connecting those dots to show how they relate.

Someone clever realized that knowledge could be stored as a graph, and that connecting individual pieces of knowledge to one another made the whole thing far more meaningful. Represent a specific domain this way and you get something genuinely useful.

Graphs are made of **nodes** (also called vertices). A node cannot exist alone in a graph; it has to connect to other nodes to form a network. That connection between one node and another is called an **edge**.

In a knowledge graph:

- A **node** represents an object or concept. In the public health domain, a node might be a disease like malaria, or a medication.
- An **edge** represents the relationship between nodes, such as *affects*, *causes*, or *located in*.

The basic building block of a KG is the **triple**: one node connected to another node through an edge. In a triple, the source node is the **subject**, the edge is the **predicate**, and the destination node is the **object**.

Nodes can also carry **attributes** (metadata that adds extra detail). Malaria might have attributes such as its causative agent, primary vector, and incubation period. A node representing a person might have attributes like birth date, nationality, and profession.

Every KG rests on two key components: an **ontology** and a **taxonomy**. The ontology acts as the structural blueprint or schema, defining the classes, properties, and logical rules of a domain. A taxonomy is the hierarchical classification layer of a knowledge graph; it organizes the domain's concepts (classes/entities) into structured parent-child relationships, typically through "is-a" or broader/narrower links, arranging them from general to specific.

## How Are Knowledge Graphs Built?

There are three common approaches:

- **Manual curation** — entering data into the graph by hand.
- **Automated extraction** — using algorithms and NLP to pull information out of unstructured sources.
- **Crowdsourcing** — collecting contributions from many people.

## Why Bother? Applications and Advantages

Knowledge graphs show up in a lot of places:

- **Search engines** use them to enhance search capabilities.
- **Recommendation systems** use them to deliver personalized suggestions.
- **Data integration** relies on them to combine data from many sources into a single framework.
- **AI and ML** systems use them to improve machine understanding.

The advantages follow naturally. KGs are excellent for integrating heterogeneous data sources, they improve data quality and consistency, they allow complex queries, and they support inference and reasoning. In short, they are a powerful tool for structuring and reasoning about complex data.

## How Are KGs Actually Built in Code?

Understanding the concept is one thing. The more interesting question is: what technologies actually turn a real-world fact into a graph?

### RDF: The LEGO Factory for Facts

The building block of a KG is a triple (subject, predicate, object). But to build a graph out of triples, we need a *standard* way of producing them. That is what **RDF (Resource Description Framework)** provides: a standardized framework for turning any fact into a triple that can link up with other triples.

Think of a triple as a LEGO block. Snap enough of them together and you can build complex structures. RDF is the machine that takes each fact as input and molds it into a block of the same shape and structure. Whether you are describing hospitals, music, recipes, spaceships, or diseases, every fact comes out as the same kind of block.

Because they share the same shape, these blocks do three useful things:

1. They **connect to each other**, since shared subjects and objects link up into a graph.
2. They can be **read by any system**, because they all follow one standard.
3. They can be **piled together from different sources** and still fit — which is precisely why KGs are so well suited to data integration.

There is one important rule in RDF: every resource must have a unique identifier called a **URI (Uniform Resource Identifier)**. The URI ensures that similar entities from different sources can be matched unambiguously and therefore connected. Imagine one dataset calls a mountain *Mt. Cameroon*, another calls it *Mt. Fako*, and a third calls it *Mt. Buea*. They can only be linked if they share the same URI.

Where do URIs come from? You can either mint your own as you build your graph, or reuse existing URIs that already identify common resources on the web. Large projects like Wikidata, DBpedia, and schema.org publish these. Instead of inventing your own "Paris," for example, you can reuse Wikidata's identifier for it.

A quick rule of thumb for what gets a URI inside a triple:

- **Subjects** always get URIs — they are the things being described.
- **Predicates** always get URIs — they are relationships being reused.
- **Objects** get a URI if they are another thing, or a **literal** if they are just a value.

### SPARQL: Asking the Graph Questions

Once your facts live in an RDF graph, you need a way to query them. That is **SPARQL (SPARQL Protocol and RDF Query Language)**, the language for querying, searching, and retrieving facts from a KG. Because predicates carry URIs, you can even query by relationship.

SPARQL's real power shows when facts are connected. Using a *federated query*, you can ask a single question that pulls answers from Wikidata and your own graph at the same time. And because the facts are linked, SPARQL can traverse the graph — for instance, "find friends of Alice's friends who live in Paris" — hopping across many triples in one query. A plain list of facts could never do that easily.

## Vocabulary and Ontology: Making Your World Shareable

When you build a KG, you are essentially creating your own little world where things relate to one another. In RDF, a subject like *Beltus* connects to an object like *Kopenicker* through a predicate like *livesIn*.

The problem is that, at first, only you understand what those terms mean. But KGs are meant to be shared, reused, and expanded by other people. For anyone to make sense of your graph, you need a **vocabulary** — a shared dictionary that provides agreed-upon definitions for the terms in your KG. A vocabulary is a published, agreed-upon set of terms (URIs) for classes of things and relationships. FOAF, schema.org, and Dublin Core are all vocabularies. Each one effectively says: "here is the official URI for *knows*, here is the one for *name*, here is the one for *Person* — please reuse these instead of inventing your own."

But a vocabulary only tells you what terms mean. It does not tell you the *rules* for how they interact. In our little world, we know a bird *hasLaid* eggs, but we cannot say a person *hasLaid* eggs — that is impossible. To describe rules like this, we need an **ontology**.

An ontology is simply a vocabulary plus the rules and structure that describe how terms relate. If a vocabulary is a dictionary (what words exist and what they mean), an ontology is the grammar and the laws of the world.

That rulebook gives your graph a superpower: it can derive facts nobody explicitly wrote. If your ontology says *hasParent* is the inverse of *hasChild*, and your data only contains "Alice hasChild Bob," a reasoner can automatically infer "Bob hasParent Alice" — even though no one ever typed that triple. Ontologies let a graph reason and reveal implicit knowledge, not just store explicit facts.

Here is the whole chain in one line:

> URIs (unique names) → vocabularies (agree on the names and meanings) → ontologies (add rules, structure, and logic) → RDF (write it all as triples) → SPARQL (ask questions and get back both stated and inferred facts).

Or, to hold the difference in your head: **vocabulary = the words; ontology = the words plus the rules of the world they describe.** Every ontology includes a vocabulary, but not every vocabulary is a full ontology.

### OWL: Writing Ontologies Machines Can Reason Over

**OWL (Web Ontology Language)** is a formal language for writing ontologies in a form machines can read and reason over. Elegantly, OWL is itself written as RDF triples, so the rules of your ontology are stored in the very same format as your data. Everything stays uniform — facts and the logic about those facts live in the same graph, expressed in the same building blocks.

OWL lets you state logical facts about your classes and relationships, such as:

- *hasParent* is the inverse of *hasChild*.
- A *Person* and an *Organization* are disjoint — nothing can be both.

Feed an OWL ontology plus your data into a **reasoner** (an inference engine) and it will:

- **Derive new triples** by applying inverse, symmetric, and subclass rules to reveal facts nobody typed.
- **Check consistency** by catching contradictions — for example, flagging an individual declared to be both a Person and an Organization when those are disjoint.
- **Classify** individuals automatically — figuring out, say, that a person with a deceased spouse is a Widow, straight from the definition.

## RDF vs. Property Graphs: Where Neo4j Fits

Not every graph technology is built on RDF. **Neo4j** is a graph database — software that stores your graph and lets you query it quickly — but it is based on the **Labeled Property Graph (LPG)** model rather than RDF.

So there are really two worlds:

- If your world is **RDF**, you store triples in a triplestore (such as GraphDB, Blazegraph, or Amazon Neptune) and query with SPARQL.
- If your world is **property graphs**, you store your data in Neo4j (the most popular option) and query with **Cypher**.

Cypher is the query language used to interact with a property-graph KG.

## GraphRAG: Bringing Knowledge Graphs into RAG

This is where everything comes together for anyone building RAG systems.

A knowledge graph can be used inside RAG to improve retrieval. A KG is simply an alternative form of data storage. In a standard RAG setup, you have a vector store. By adding a KG database as a *second* data source during the retrieval step, you get **GraphRAG**.

The payoff is that when a user sends a query, you can run more than one kind of search to find relevant information: **sparse**, **dense**, and **graph** search. GraphRAG lets you perform graph search *in addition to* vector search.

Why does this matter? Traditional RAG systems are good at pinpointing specific document chunks in a knowledge base, but they fail at the holistic, global context. Because they search for isolated text segments, they are poor at answering broad questions about an entire dataset. This is exactly the problem Microsoft researchers observed, and it led them to propose GraphRAG as a solution in their paper *From Local to Global: A GraphRAG Approach to Query-Focused Summarization*.

Traditional RAG works by using text embeddings to retrieve specific information. It is powerful, but it has limits: response quality depends heavily on how clear and specific the query is, and — the bigger challenge — it struggles to reason effectively across multiple documents. Layering a knowledge graph on top gives the system a way to follow relationships and reason across the whole dataset, not just fetch the nearest chunk.

## Closing Thoughts

As a researcher and data scientist, I believe there is always room to improve any system — and that improving a system starts with continuously improving yourself. Studying Knowledge Graphs and GraphRAG gave me a clearer mental model for building retrieval systems that reason rather than merely fetch.

If you build RAG pipelines, especially in a domain as connected as health, I think it is worth the time to understand how connected facts can make your AI genuinely smarter. The path runs from URIs to vocabularies to ontologies, expressed as RDF triples, queried with SPARQL or Cypher, and finally woven into your retrieval pipeline as GraphRAG.

*These notes grew out of the "[AI Enhancement with Knowledge Graphs — Mastering RAG Systems](https://www.coursera.org/learn/packt-ai-enhancement-with-knowledge-graphs-mastering-rag-systems-lnmqm)" course offered by Packt via Coursera, combined with my own reading and experiments.*
