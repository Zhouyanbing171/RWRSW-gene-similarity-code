# Code and Experimental Data for RWRSW Method
This repository contains the source code, experimental scripts, and dataset for the paper "An enhanced network-based method for gene functional similarity calculation" (submitted to PeerJ Computer Science).

## Repository Structure
This repository includes three core folders in the root directory:

### 1. RWRSW_py/
Core Python implementation of the **RWRSW (Random Walk with Restart Smoothing)** algorithm.
- Main algorithm classes, data models, and program entry
- Includes modules for random walk, matrix calculation, GO term processing, annotation parsing, and result calculation
- Entry file: Main.py

### 2. RWRSW_re/
Experimental reproduction and result analysis scripts.
- Used to reproduce all experimental results in the paper
- Includes similarity calculation, LFC score computation, result feature statistics, and data validation
- Ensures full reproducibility of the experiments

### 3. RWRSW_Arabidopsis_thaliana/
Preprocessed dataset for *Arabidopsis thaliana* (used in the experiment).
- Raw data: gene network, GO terms, annotation files, pathway files
- Intermediate data: generated matrix files, gene lists, term relationships
- Result data: similarity scores, random walk output, and LFC evaluation results

## Usage
1. All files are compressed and split into 20MB volumes to meet GitHub limits.
2. Extract the .001 file to automatically restore the complete original package.
3. Run the main algorithm from RWRSW_py/Main.py.
4. Use scripts in RWRSW_re/ to reproduce experiments.

## Note on Large File Uploads
All large compressed files are split into 20MB volumes to comply with GitHub's file size limit.
To restore the original file:
1. Download all volume files (e.g., `.001`, `.002`, `.003`, `.004`) into the same folder.
2. Right-click the `.001` file and select "Extract here" (using 7-Zip or WinRAR).
All volumes will be automatically merged to restore the complete original archive.

## Citation
If you use this code or dataset in your research, please cite our paper.
