# schema-agnostic-process-trace-construction
This repository contains the implementation accompanying the paper 
“Schema-Agnostic Process Trace Construction: From Raw Tables to Execution Behavior” 
(submitted to CAiSE 2026).

The project introduces a schema-agnostic pipeline that reconstructs high-fidelity
process execution traces directly from heterogeneous, key-sparse relational databases.
Unlike traditional approaches that depend on predefined schemas, ER diagrams, or
clean primary/foreign keys, this method derives execution behavior solely from
statistical properties of the data.

The pipeline includes:

• Identifier and timestamp profiling based on distinctness, completeness, and stability  
• Type-aware statistical link discovery across tables using Jaccard and JS/KS similarity  
• Temporalization and sequencing of events across multiple timestamp fields  
• Learning cross-table ordering and flow relations using a Temporal Convolutional Network  

Evaluations on TPC-H/E benchmarks, synthetic datasets, and an industry dataset
demonstrate strong event recovery, accurate trace ordering, and robustness to schema drift.

The repository includes:
- Source code for Stages S1–S4 of the pipeline
- Scripts for dataset generation and preprocessing
- Training utilities for the TCN sequence model
- Experiment configurations and reproducible workflows
