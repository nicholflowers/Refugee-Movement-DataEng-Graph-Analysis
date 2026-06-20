# 🌍 Refugee Movement Network Analysis

A graph analysis of global refugee movement. Countries are modeled as nodes and
refugee flows as directed, weighted relationships, and network algorithms are
used to find the countries at the center of forced displacement and the paths
that connect them.

## 📌 Overview

Refugee movement is naturally a network: people flow out of some countries and
into others, and the structure of those flows reveals which countries carry the
most weight in the global system. This project loads global refugee movement
data, models it as a graph in Neo4j, and applies centrality and path algorithms
to go beyond simple counts and measure each country's structural importance in
the network.

## 🗂️ Dataset

The data is the TidyTuesday **"Refugees"** dataset (2023-08-22), `population.csv`,
which comes from the **`{refugees}` R package** (PopulationStatistics) — a wrapper
around the **United Nations High Commissioner for Refugees (UNHCR)** Refugee Data
Finder. It records forced-displacement counts (refugees, asylum-seekers,
internally displaced persons, stateless persons, and others) by country of origin
and country of asylum.

- **Immediate source:** TidyTuesday (2023-08-22), via the `{refugees}` R package
- **Ultimate source:** United Nations High Commissioner for Refugees (UNHCR) Refugee Data Finder (which also draws on UNRWA and IDMC)
- **Coverage:** the table spans 2010–2022; this project compares three snapshot years — **2010, 2015, and 2020**
- **Structure:** each record is a country-of-origin → country-of-asylum pair with annual counts

## 🏗️ Architecture

The data moves through three NoSQL stores, each used for what it does best:

- **MongoDB** — document store for the ingested refugee records
- **Redis** — key-value store for fast lookups
- **Neo4j** — graph database where countries and flows are modeled and analyzed

## 🔬 Approach

1. **Graph model.** Countries are nodes. A directed, weighted relationship
   connects an origin country to an asylum country, weighted by the number of
   refugees moving along that path.
2. **Loading.** The data is loaded and the graph is built with Cypher.
3. **Analysis.** Neo4j Graph Data Science (GDS) algorithms run over the graph:
   - **Degree centrality** — out-degree captures sending pressure; in-degree captures receiving load
   - **PageRank** — each country's structural importance in the overall flow network
   - **Minimum spanning tree (MST)** — the backbone that connects the network at least total weight
   - **Dijkstra shortest path** — least-cost displacement routes between countries
4. **Visualization.** Incoming and outgoing flows are charted in Python so the
   leading countries read at a glance.

## 🏆 Findings

Comparing **2010, 2015, and 2020** shows how the centers of displacement shifted
over the decade. Degree centrality surfaces the countries that send and receive
the most refugees, PageRank highlights the countries most central to the overall
flow structure, the minimum spanning tree reveals the network's backbone, and
Dijkstra shortest paths trace the least-cost routes between countries. The
specific top sending and receiving countries for each year are summarized in the
project's accompanying slide deck.

## 🧠 Skills Demonstrated

- **Graph data modeling** — representing a real-world domain as nodes and weighted, directed relationships
- **Network analysis** — applying centrality and path algorithms to rank entities by structural importance rather than raw counts
- **Multi-database pipeline** — moving data across document (MongoDB), key-value (Redis), and graph (Neo4j) stores
- **Neo4j & Cypher** — building and querying a graph database and running GDS algorithms
- **Interpretation & visualization** — turning graph metrics into clear charts of where refugee movement concentrates

## 🧰 Tools

Neo4j (Cypher, Graph Data Science library), MongoDB, Redis, and Python for data
preparation and visualization.


## 📝 Notes

Final project for **DATA SCI W205 – Fundamentals of Data Engineering** (UC Berkeley
MIDS). Credentials and connection details have been removed from the published
code; the notebook reads them from environment variables.
