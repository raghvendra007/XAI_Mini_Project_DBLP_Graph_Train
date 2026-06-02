# DBLP Paderborn University — Graph Dataset Preparation

## Project Overview
This repository contains the dataset preparation pipeline for an Explainable AI (XAI) 
mini project focused on Graph Neural Networks (GNN) and GNN explainability.

The goal is to extract a focused subgraph from the DBLP RDF dataset containing authors 
affiliated with Paderborn University, their publications, and publication venues — then 
convert it into a format suitable for GNN training and explanation.

---

## My Role
I was responsible for:
- Downloading and analyzing the raw DBLP RDF dataset (22GB TTL file)
- Extracting Paderborn University authors, their papers, and venues
- Fixing parsing bugs in the raw TTL format
- Building clean graph files ready for PyTorch Geometric (PyG)

---

## Raw Data Source
- Downloaded from: https://dblp.org/rdf/
- File: `dblp.ttl` (~22 GB, ~750 million lines)
- Format: RDF Turtle (.ttl)
- **Not committed** to this repo due to file size — must be downloaded separately

---

## Pipeline — What Each Step Does

### Step 1 — Extract Paderborn Authors
- Streamed through the full 22GB TTL file line by line
- Identified author blocks where `dblp:primaryAffiliation` contains "Paderborn"
- Extracted author URI, name, and all affiliations
- **Output:** `paderborn_authors_v2.csv`

### Step 2 — Extract Papers
- Made a second full pass through the TTL file
- For each publication block, checked if `dblp:authoredBy` matched a Paderborn author
- Fixed three bugs: wrong relation direction, block boundary detection, comma-separated authors on one line
- **Output:** `papers.csv`, `author_paper_edges.csv`

### Step 3 — Build Graph Files
- Extracted unique venues from papers
- Assigned integer node IDs to all authors, papers, and venues
- Built edge table with relation types
- **Output:** `nodes.csv`, `edges.csv`, `venues.csv`

---

## Dataset Statistics

| Item | Count |
|---|---|
| Author nodes | 198 |
| Paper nodes | 6,542 |
| Venue nodes | 1,445 |
| **Total nodes** | **8,185** |
| author_of edges | 7,795 |
| published_in edges | 6,385 |
| **Total edges** | **14,180** |

---

## Output Files

| File | Description |
|---|---|
| `paderborn_authors_v2.csv` | 198 Paderborn authors with name, URI, affiliations |
| `papers.csv` | 6,542 papers with title, year, venue, authors |
| `author_paper_edges.csv` | Raw author → paper edges with URIs |
| `venues.csv` | 1,445 unique conferences and journals |
| `nodes.csv` | All 8,185 nodes with integer ID, type, label, original URI |
| `edges.csv` | All 14,180 edges as integer pairs with relation type |
| `Mini_Project_XAI.ipynb` | Full pipeline notebook — Steps 1, 2, and 3 |

---

## Graph Structure