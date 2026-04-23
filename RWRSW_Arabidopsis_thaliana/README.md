# An Enhanced Network-Based Method for Gene Functional Similarity Calculation (Arabidopsis thaliana Data)

## Project Description
This repository contains the implementation of the paper "An Enhanced Network-Based Method for Gene Functional Similarity Calculation" on Arabidopsis thaliana data. The method is based on the Random Walk with Restart algorithm on the Arabidopsis gene interaction network, integrating GO annotations and biochemical pathway information to calculate gene functional similarity.

---

## Dataset Information

### Raw Data Files
- `aracyc_pathways.20230103` – EC grouping data from the AraCyc database (Source: https://www.arabidopsis.org/download/list?dir=Pathways%2FArchived_Data_dumps%2FPMN9_September2014).  
  Note: Data access is difficult. Official application and special software are required. Only the 2014 version is available on TAIR, which is inaccessible in most regions of China. It is recommended to use the provided preprocessed files.
- `tair.gaf` – GO annotation file from TAIR (Source: https://current.geneontology.org/products/pages/downloads.html).
- `AraNet.txt` – Original gene interaction network (Source: https://www.functionalnet.org/aranet/download.html).
- `AraNet_GS.txt` – Gold-standard gene pair network (Source: https://www.functionalnet.org/aranet/download.html).
- `go-basic.obo` – Gene Ontology database file (Source: https://www.geneontology.org/docs/download-ontology/).

### Intermediate Data Files
- `lfcPair.txt` – Gene pairs for LFC scoring, preprocessed from `aracyc_pathways.20230103`.
- `geneList.txt` – Filtered gene list.
- `netMatrix.txt` – Network adjacency matrix.
- `pMatrix.txt` – Normalized probability transition matrix.
- `child.txt` – Collection of GO descendant terms.
- `son.txt` – Collection of direct GO child terms.

### Result Data Files
- `result0.9.mat` – Random walk result matrix with restart parameter 0.9.
- `similarityResult0.9.txt` – Gene pair similarity scores with restart parameter 0.9.
- `lfc0.9.txt` – Algorithm scoring results under each EC group.

### Important Warning
The acquisition of Arabidopsis EC grouping data (aracyc_pathways) is extremely difficult.  
Official full library: https://plantcyc.org/downloads/, which requires a license agreement and dedicated software, and is very unstable in China.  
TAIR archive: https://www.arabidopsis.org/download/list?dir=Pathways%2FArchived_Data_dumps%2FPMN9_September2014, only the 2014 old version is available.  
For experiment reproduction, it is recommended to directly use the preprocessed `lfcPair.txt` provided in this project to avoid re-downloading the original EC data.

---

## Code Information

| Class Name | Description |
|------------|-------------|
| `Annotation` | Data model for GO annotation information |
| `FunctionNet` | Data model for ontology term network |
| `LfcPair` | Tool for obtaining and preprocessing LFC gene pairs |
| `Term` | Data model for GO terms |
| `Reader` | Helper class for file reading |
| `Matrix` | Helper class for matrix operations |
| `RandWalk` | Implementation of Random Walk with Restart algorithm |
| `Calculator` | Core calculation class with three main interfaces |
| `Main` | Program entry, connecting the full pipeline |

---

## Usage Instructions

### 1. Import the Project
Open IntelliJ IDEA and select "Open Project".  
Select the root directory containing the source code and import with default settings.  
Ensure the project SDK is set to JDK 8 or higher.

### 2. Configure Data Paths
Note: Raw Arabidopsis EC data is difficult to obtain. Please directly use the `lfcPair.txt` provided in this project without re-downloading the original files.  

Place all raw data files in the `buf/` folder under the project root directory (or the path specified in the code).  
- `go-basic.obo` is placed directly under `buf/`.  
- All other data files are placed under `buf/Arabidopsis_thaliana/`.  

If the path is inconsistent, modify the file reading path in `Main.java` or related classes.

### 3. Select Ontology Domain
Note the line in `Main.main()`:
Calculator.data_initializer(gene_type, alpha, "molecular_function");

The parameter "molecular_function" indicates the selected GO domain. You can change it to "biological_process" to switch the domain.

### 4. Run the Program
Locate the Main class in the project view (usually under the src/ directory).
Right-click the Main class and select Run 'Main.main()'.
The program will execute sequentially: data loading, network construction, random walk, and result output.

### 5. Result Output
Result files are generated in the results/ folder by default.

## Runtime Environment Requirements
- IntelliJ IDEA (Community or Ultimate Edition)
- JDK 8 or higher
- Recommended memory: heap memory ≥ 4 GB

## Methodology Overview
The algorithm pipeline in the Calculator class is divided into four main stages:

1.Data Loading & Parsing: Read GO hierarchy (go-basic.obo), annotation file (tair.gaf), and interaction network (AraNet.txt).

2.Network Preprocessing: Construct the adjacency matrix and normalize it into a probability transition matrix (pMatrix.txt).

3.Random Walk with Restart: Perform random walk calculations for each gene pair using the RandWalk class.

4.Result Evaluation: Compare similarity scores with pathway-based EC groups (LFC scoring).

For detailed data preprocessing steps, please refer to the Materials and Methods section of the main paper.

## Citation
[Author List]. An Enhanced Network-Based Method for Gene Functional Similarity Calculation. PeerJ Computer Science, [Year].

## License & Contribution
This project is open-source under the MIT License.
