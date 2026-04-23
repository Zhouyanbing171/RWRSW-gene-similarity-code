An Enhanced Network-Based Method for Gene Functional Similarity Calculation (Python Implementation)
======================================================================

Project Description
--------
This repository contains the Python implementation of the paper "An Enhanced Network-Based Method for Gene Functional Similarity Calculation".
The code includes two core modules: 
1) Random Walk with Restart Similarity Score Matrix Calculation
2) Result Visualization & Plotting

For human gene data experiments, only the random walk similarity score matrix calculation is completed.

This study covers three model organisms:
    - Saccharomyces cerevisiae (Yeast)
    - Arabidopsis thaliana
    - Homo sapiens (Human)

========================================================================
Dataset Information
========================================================================

1. Yeast Data
--------------------
Raw Data Files:
    `biochemical_pathways.tab` – EC group data for pathway analysis (Source: http://sgd-archive.yeastgenome.org/)
    `sgd.gaf` – Gene Ontology annotation file for yeast (Source: https://current.geneontology.org/products/pages/downloads.html)
    `YeastNet.v3.txt` – Original yeast gene interaction network (used to build netHashMap, Source: https://www.functionalnet.org/yeastnet/)
    `YeastNet.v3.benchmark.txt` – Gold-standard benchmark gene pairs (Source: https://www.functionalnet.org/yeastnet/)
    `go-basic.obo` – Gene Ontology structure file (Source: https://www.geneontology.org/docs/download-ontology/)

Intermediate Files:
    lfcPair.txt                      - LFC gene pairs generated from biochemical_pathways.tab
    geneList.txt                     - Filtered gene list from YeastNet.v3.benchmark.txt
    netMatrix.txt                    - Adjacency matrix of gene network
    pMatrix.txt                      - Normalized probability transition matrix
    child.txt                        - GO term descendants
    son.txt                          - Direct GO term children

Result Files:
    result0.9.mat                    - Random walk result (restart probability = 0.9)
    similarityResult0.9.txt          - Gene pair similarity scores
    lfc0.9.txt                       - LFC evaluation scores per EC group
    yeast_biological_process.zip     - Results for biological_process domain
    yeast_molecular_function.zip     - Results for molecular_function domain

Additional plotting files:
    0.9.txt
    18.txt
    bo.txt
    lfcresult18.txt


2. Arabidopsis thaliana Data
--------------------------------------
Raw Data Files:
    aracyc_pathways.20230103        - EC grouping data from AraCyc (Source: https://www.arabidopsis.org/download/list?dir=Pathways%2FArchived_Data_dumps%2FPMN9_September2014)
    tair.gaf                         - TAIR Gene Ontology annotation (Source: https://current.geneontology.org/products/pages/downloads.html)
    AraNet.txt                       - Original gene interaction network (Source: https://www.functionalnet.org/aranet/download.html)
    AraNet_GS.txt                    - Gold-standard gene network (Source: https://www.functionalnet.org/aranet/download.html)
    go-basic.obo                     - Gene Ontology structure (Source: https://www.geneontology.org/docs/download-ontology/)

Important Note: 
Official Arabidopsis EC pathway data requires application, special software, and is unstable in mainland China. 
We strongly recommend using the provided preprocessed `lfcPair.txt` directly.

Intermediate Files:
    lfcPair.txt                      - Preprocessed LFC gene pairs
    geneList.txt                     - Filtered gene list
    netMatrix.txt                    - Network adjacency matrix
    pMatrix.txt                      - Transition matrix
    child.txt                        - GO term descendants
    son.txt                          - Direct GO children

Result Files:
    result0.9.mat                    - Random walk output
    similarityResult0.9.txt          - Similarity scores
    lfc0.9.txt                       - LFC evaluation
    Arabidopsis_thaliana_biological_process.zip
    Arabidopsis_thaliana_molecular_function.zip


3. Human Data
--------------------
Raw Data Files:
    PubChem_pathway_text_human.csv  - EC grouping data from PubChem
    goa_human.gaf                  - Human GO annotation (Source: https://current.geneontology.org/products/pages/downloads.html)
    HumanNet-XC.tsv             - Gene interaction network (Source: https://www.functionalnet.org/humannet/download.html)
    HumanNet-GSP.tsv               - Gold-standard gene pairs (Source: https://www.functionalnet.org/humannet/download.html)
    go-basic.obo                    - Gene Ontology structure

Intermediate Files:
    geneList.txt
    netMatrix.txt
    pMatrix.txt

Result Files:
    result0.9.mat                    - Random walk score matrix only


========================================================================
Code Structure
========================================================================

Annotation          – Data model for gene annotations
FunctionNet         – GO term network model
Term                – GO term data structure
Reader              – Helper for file parsing
PrintResult         – Result plotting helper
PrintMap            – Visualization module
RandWalk            – Random walk algorithm
Calculator          – Core calculation class (four modules)
Main                – Program entry point


========================================================================
Usage Instructions (Python Environment)
========================================================================

1. Environment Setup
   - PyCharm (Community or Professional)
   - Python 3.12 or higher (Anaconda recommended)
   - CUDA Toolkit installed (for GPU acceleration)

2. Install Dependencies
       numpy
       scipy
       pandas
       matplotlib
       seaborn
       networkx
       cupy
       scikit-learn

   Install via:
       pip install numpy scipy pandas matplotlib seaborn networkx cupy scikit-learn

3. Import Project
   - Open PyCharm → Open → Select root folder

4. Data Path Configuration
   - Place data in:
         buf/yeast/
         buf/arabidopsis/
         buf/human/

5. Select GO Domain
   In Main.main():
       Calculator.data_initializer(gene_type, alpha, "molecular_function");
   Change to "biological_process" if needed.

6. Run the Program
   - Right-click Main.py → Run 'Main'

7. Plotting
   - Run PrintResult.py or PrintMap.py after similarity calculation

8. Important Notes
   - Human data: Only random walk matrix is completed.
   - For large networks (e.g., HumanNet), increase memory limit in PyCharm.

========================================================================
Methodology Overview
========================================================================

1. Data Loading & Parsing
2. Network Preprocessing
3. Random Walk with Restart
4. Evaluation & Plotting (Yeast & Arabidopsis)

========================================================================
Data Availability & Citation
========================================================================

Data Sources:
   - YeastNet:  https://www.functionalnet.org/yeastnet/
   - AraNet:    https://www.functionalnet.org/aranet/
   - HumanNet:  https://www.functionalnet.org/humannet/
   - GO:        https://geneontology.org/
   - TAIR:      https://www.arabidopsis.org/
   - SGD:       https://yeastgenome.org/
   - GOA:       https://ebi.ac.uk/GOA

Please cite our paper when using this code:
[Authors]. An Enhanced Network-Based Method for Gene Functional Similarity Calculation. PeerJ Computer Science, [Year].

========================================================================
License
========================================================================

This project is open-sourced under the MIT License. See LICENSE for details.
