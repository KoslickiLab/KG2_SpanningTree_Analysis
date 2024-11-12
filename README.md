
# DrugMechDB vs. RTX-KG2 Spanning Tree Analysis

## Introduction
This repository is dedicted to the investigation and analysis of spanning trees between [DrugMechDB](https://github.com/SuLab/DrugMechDB) and [RTX-KG2](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-022-04932-3) (version 2.8.4c & 2.9.0c) knowledge graphs.

DrugMechDB is a drug mechanism database representing pathways through a manually curated knowledge graph. RTX-KG2 is a Biolink Model-compliant standardized knowledge graph.

## Objective
1. **Investigate Spanning Tree Variance**
    - Conduct and extract spanning tree search within 3-hop neighborhood in RTX-K2 using the _aligned and normalized nodes_ (only canonical IDs).
    - Analyze differences between KG2 and DrugMechDB trees. 
   
2. **Statistical Analysis**
    - Assess the heuristic approaches in RTX-KG2 and DrugMechDB and quantify the extent of heuristics in RTX-KG2.

## Repository Content
The script contains the data location and instructions necessary to perform the analysis. 

1. `k-hop_spanningtree_script.ipynb` is Jupyter Notebook script for spannin tree analysis.

2. The `data` folder contains the CSV file, `sorted_graph_DrugMechDB.csv`,  that was used as input(canonical IDs).

3. The `image` folder contains the spanning tree result in graph visuals format in PDF file. 

## Method
The concept of the approach is based on [Steiner Tree](https://en.wikipedia.org/wiki/Steiner_tree_problem). The goal is to determine a tree structure that minimizes edge count while maximizing node coverage, treating nodes as a collection (bag of nodes) without considering specific connections. 

There are 9695 graphs in DrugMechDB (DMDB); refer to their `indication_paths.yaml` file. To demonstrate our method we chose the graphs with longest paths, $\geq$ 10 nodes. We focused on `KG2.9.0c` (691 grpahs) because it yielded more than `KG2.8.0c`. 

### Spanning Tree Construction
For our purpose, we used Graph-tool's [`all_paths` function](https://graph-tool.skewed.de/static/doc/autosummary/graph_tool.topology.all_paths.html) to find paths.
When examning 1-hop neighbhood path in `KG2.9.0c`, the resulting trees were disjointed meaning there were disconnected components (isolated nodes, disjointed graphs). To compensate this challenge, we generated vertices (node) pair combinations. For example a graph with 20 nodes will result in 190 vertex pair combinations:

<p align="center">
  <img src="https://latex.codecogs.com/svg.latex?\large&space;{}_nC_r=\frac{20!}{2!(20-2)!}=190" title="Combinations formula"/>
</p>

Then, we expanded our search up to __3-hop neighborhood__ identifying the links (intermediary) that connect the disjointed components.
If the vertex pair has a path within 3-hop, it will return the pair; essentially you are drawing a direct edge between those nodes.

<p align="center">
 <img src="https://latex.codecogs.com/svg.latex?\large&space;A\rightarrow%20B\rightarrow%20C\;\text{%20will%20be...%20}\;A\rightarrow%20C" title="Path reduction"/>
</p>

## Data
To conduct or reproduce the spanning tree analysis you will need KG2 version 2.9.0c. Contact [RTX-KG2 team](https://github.com/RTXTeam/RTX) to request access to the knowledge graphs.
Please refer to [this](https://github.com/stephwon/KG2_DrugMechDB_EA_Analysis/tree/main) respository to obtain **node normalized and aligned**.
To run the Jupyter Notebook script, follow these steps:
1. Clone the repository to your local machine
```
git clone https://github.com/your-username/KG2_SpanningTree_Analysis.git
```
2. Navigate to the project directory.

3. Import the necessary input files. The location of the data for each analysis is provided in the script

4. Open and run the script using the command below:
```
jupyter notebook <name_of_the_script>.ipynb
```
The final output file that contains the spanning tree should in YAML format.

Note: The input data should be canonical IDs. Additionally, the nodes must be normaized and aligned.

## Requirements
We recommend you have the following specifications to successfuly run the script and analysis.
* Operatting system(s): Linux Ubuntu
* Programming Language: 
    * Bash
    * Python 3.8.12 or higher
* Other requirements:
    * [Graph-tool](https://graph-tool.skewed.de) (version 2.68)
    * [NetworkX](https://networkx.org/documentation/stable/index.html) (version 3.0)
    * Anaconda (version 24.1.2)
    * Multiple CPU cores

## References
1. Gonzalez-Cavazos, A.C., Tanska, A., Mayers, M. et al. DrugMechDB: A Curated Database of Drug Mechanisms. Sci Data 10, 632 (2023). [https://doi.org/10.1038/s41597-023-02534-z](https://www.nature.com/articles/s41597-023-02534-z)
2. DrugMechDB: [GitHub Repository](https://github.com/SuLab/DrugMechDB)
3. Wood, E.C., Glen, A.K., Kvarfordt, L.G. et al. RTX-KG2: a system for building a semantically standardized knowledge graph for translational biomedicine. BMC Bioinformatics 23, 400 (2022). [https://doi.org/10.1186/s12859-022-04932-3](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-022-04932-3)
