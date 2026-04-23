# An Enhanced Network-Based Method for Gene Functional Similarity Calculation (Yeast Data)

## Project Description
This repository contains the implementation of the paper "An Enhanced Network-Based Method for Gene Functional Similarity Calculation" on yeast (Saccharomyces cerevisiae) data. The method is based on the random walk algorithm on the yeast gene interaction network, combined with Gene Ontology (GO) annotations and biochemical pathway information to calculate gene functional similarity.

---

## Dataset Information

### Raw Data Files
- `biochemical_pathways.tab` – Enzyme Commission (EC) grouping data for pathway analysis (Source: http://sgd-archive.yeastgenome.org/)
- `sgd.gaf` – Gene Ontology annotation file for yeast (Source: https://current.geneontology.org/products/pages/downloads.html)
- `YeastNet.v3.txt` – Original yeast gene interaction network (used to construct `netHashMap`, Source: https://www.functionalnet.org/yeastnet/)
- `YeastNet.v3.benchmark.txt` – Gold-standard benchmark gene pairs (used to generate `geneList.txt`, Source: https://www.functionalnet.org/yeastnet/)
- `go-basic.obo` – Gene Ontology term database file (Source: https://www.geneontology.org/docs/download-ontology/)

### Intermediate Data Files (Generated During Preprocessing)
- `lfcPair.txt` – Gene pairs for LFC scoring, preprocessed from `biochemical_pathways.tab`
- `geneList.txt` – Gene name list for analysis, filtered from `YeastNet.v3.benchmark.txt`
- `netMatrix.txt` – Matrix representation of the original gene interaction network
- `pMatrix.txt` – Normalized probability transition matrix calculated by the `RandWalk` class
- `child.txt` – Collection of descendant terms in GO
- `son.txt` – Collection of direct descendant terms in GO

### Result Data Files
- `result0.9.mat` – Random walk result matrix with restart parameter 0.9
- `similarityResult0.9.txt` – Gene pair similarity scores with restart parameter 0.9
- `lfc0.9.txt` – Algorithm scoring results under each EC group

---

## Code Information
This project is written in Java. The main classes and functions are as follows:

| Class Name | Description |
|------------|-------------|
| `Annotation` | Data model for GO annotation information |
| `FunctionNet` | Data model for ontology term networks |
| `LfcPair` | Utility class for obtaining LFC gene pairs |
| `Term` | Data model for GO terms |
| `Reader` | Helper class for reading special-format files |
| `Matrix` | Helper class for matrix operations |
| `RandWalk` | Implementation class for the Random Walk with Restart algorithm |
| `Calculator` | Core calculation class, divided into four parts with three main external interfaces |
| `Main` | Program entry class that connects the three interfaces to run the entire pipeline |
| `ResultCalculate` | Result calculation class for preliminary analysis of result features |

---

## Usage Instructions

### 1. Import the Project
Open IntelliJ IDEA and select "Open Project".  
Select the root directory containing the source code and import with default settings.  
Ensure the project SDK is set to JDK 8 or higher.

### 2. Configure Data Paths
Place all files listed in "Raw Data Files" into the `buf/` folder under the project root directory  
(or store them according to the path specified in the code).  

Among them:
- `go-basic.obo` is placed directly under `buf/`
- All other data files are placed under `buf/Yeast/`

If the path is inconsistent, modify the file reading path in `Main.java` or related classes.

### 3. Select Ontology Domain
Note the parameter in `Main.main()`:

Calculator.data_initializer(gene_type, alpha, "molecular_function");

The parameter "molecular_function" means the program uses the molecular_function domain.
You can change it to "biological_process" to switch the domain.

### 4. Run the Program
Locate the Main class in the project view (usually under the src/ directory).

Right-click the Main class and select Run 'Main.main()'.

The program will execute sequentially:

- Load GO structure and annotation files
- Build the gene interaction network
- Calculate the probability transition matrix
- Perform Random Walk with Restart (default restart parameter = 0.9)
- Output similarity result files and LFC evaluation files

### 5. Result Output
Result files will be generated in the results/ folder or the output directory specified in the code.

## Runtime Environment Requirements
- IntelliJ IDEA (Community or Ultimate Edition)
- Java Development Kit (JDK) 8 or higher
- Sufficient memory for matrix operations (heap memory > 4 GB recommended for full yeast network)

## Methodology Overview
The algorithm pipeline in the Calculator class is divided into four main stages:

1.Data Loading & Parsing: Read GO hierarchy (go-basic.obo), annotation file (sgd.gaf), and interaction network (YeastNet.v3.txt).

2.Network Preprocessing: Construct the adjacency matrix and normalize it into a probability transition matrix (pMatrix.txt).

3.Random Walk with Restart: Perform random walk calculations for each gene pair using the RandWalk class.

4.Result Evaluation: Compare similarity scores with pathway-based EC groups (LFC scoring).

For detailed data preprocessing steps, please refer to the "Materials and Methods" section of the main paper.

## Citation
[Author List]. An Enhanced Network-Based Method for Gene Functional Similarity Calculation. PeerJ Computer Science, [Year].

## License & Contribution
This project is open-source under the MIT License.
