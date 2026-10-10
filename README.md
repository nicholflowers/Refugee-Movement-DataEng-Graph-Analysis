# 🌍 Refugee Movement Network Analysis

## 📌 Overview

When millions of people are forced from their homes, aid organizations and
policymakers need to know where to focus their support. That starts with knowing
where refugees go and which countries send and absorb the largest flows. This
project explores both by modeling refugee flows as graphs in Neo4j. Since
people flow out of some countries and into others, this approach goes beyond
simple counts to measure each country's structural importance and trace the
routes that connect them.

## 🗂️ Dataset

Refugee data from the United Nations High Commissioner for Refugees (UNHCR),
published through TidyTuesday (`population.csv`, 2023-08-22). Each record is a
country-of-origin → country-of-asylum pair with annual counts of refugees and
other displaced people, covering 2010–2022.

## 🚀 Implementation Details

1. **Graph model.** Countries are nodes. A directed, weighted relationship
   connects an origin country to an asylum country, weighted by the number of
   refugees moving along that path. A second graph reverses the direction to
   rank countries as both refugee producers and recipients.
2. **Loading.** Only flows of more than 5,000 refugees are kept, to focus the
   analysis on the largest movements. A separate graph is built with Cypher for
   each year.
3. **Analysis.** Neo4j Graph Data Science (GDS) algorithms run over the graphs,
   moving from which countries matter, to how they're connected, to the routes
   between them:
   - **PageRank**: each country's structural importance in the overall flow network
   - **Degree centrality**: out-degree counts how many countries each country sends refugees to; in-degree counts how many countries it receives refugees from
   - **Minimum spanning tree (MST)**: the strongest connections from each top origin country to asylum countries 
   - **Dijkstra shortest path**: least-cost displacement routes from the top origin countries to the United States
4. **Visualization.** Results are charted and mapped for 2010, 2015 and 2020 so
   the leading countries and routes read at a glance.

## 🏆 Results

**PageRank (2010–2022)**
The United States ranked as the top destination every year from 2013 to 2022,
drawing refugees from many of the countries producing the most refugees. Canada
led from 2010 to 2012. On the origin side, stateless populations ranked at or
near the top every year and Syria rose sharply by 2015 at the height of the
Syrian refugee crisis. The strength of the U.S. ranking shaped the rest of the
analysis, which follows routes to the United States.

**Degree Centrality (2010, 2015, 2020)**
Germany, the United States and the United Kingdom received large flows from the
widest range of countries in all three years, making them global hubs. Uganda
and South Africa stood out as major regional hosts. Somalia, Iraq, Afghanistan
and the Democratic Republic of the Congo consistently sent refugees to the most
countries, while Syria and Venezuela rose in later years as new or intensifying
conflicts drove displacement in many directions.

**Minimum Spanning Tree (2010, 2015, 2020)**
The analysis examined connections between refugee-origin and asylum countries 
across three years. Due to an oversight, the edge weights were set to refugee 
counts instead of their inverse, which led the algorithm to select the network 
with the smallest combined refugee counts and favor smaller flows over the 
strongest migration connections. Inverse counts would have assigned lower weights 
to connections carrying more refugees, allowing the algorithm to prioritize larger 
flows.

**Shortest Path (2010, 2015, 2020)**
Syria, Iraq and Venezuela had direct paths to the United States, suggesting
strong resettlement ties. Refugees from Somalia, Afghanistan and Pakistan often
passed through other countries first, pointing to longer and riskier journeys.
Stateless populations had no path to the U.S. at all, highlighting how
vulnerable they remain.

See the slide deck for charts and maps from each analysis. The year-by-year animations play only in the PowerPoint version.

## 🧠 Skills Demonstrated

- **Graph data modeling**: representing a real-world domain as nodes and weighted, directed relationships
- **Network analysis**: applying centrality and path algorithms to rank entities by structural importance rather than raw counts
- **Neo4j & Cypher**: building and querying a graph database and running GDS algorithms
- **Interpretation & visualization**: turning graph metrics into clear charts of where refugee movement concentrates

## 🧰 Tools

Neo4j (Cypher, Graph Data Science library) and Python for data preparation and
visualization.
