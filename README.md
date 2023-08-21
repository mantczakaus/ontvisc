# ONTViSc (ONT-based Viral Screening for Biosecurity)

## Introduction
eresearchqut/ontvisc is a Nextflow-based bioinformatics pipeline designed to help diagnostics of viruses and viroid pathogens for biosecurity. It takes fastq files generated from either amplicon or whole-genome sequencing using Oxford Nanopore Technologies as input.

The pipeline can either: 1) perform a direct search on the sequenced reads, 2) assemble the reads to generate longer contigs or 3) directly map reads to a known reference. 

The reads can optionally be filtered from a plant host before performing downstream analysis.

## Pipeline summary
![diagram pipeline](docs/images/OVISP_pipeline.jpeg)

## Run the Pipeline

## Example of commands

1. After pre-processing WGS-derived ONT reads (adapter removal with PoreChop ABI and quality filtering with NanoFilt), perform direct read taxonomic classification at the protein level using the tool Kaiju
```
nextflow run eresearchqut/ontvisc -resume --skip_host_filtering \\
                                --skip_clustering \\
                                --skip_denovo_assembly \\
                                --kaiju \\
                                --kaiju_dbnodes /path_to_kaiju_databases/nodes.dmp \\
                                --kaiju_dbname /path_to_kaiju_databases/kaiju_db_rvdb.fmi
```

2. After pre-processing WGS-derived ONT reads (adapter removal with PoreChop ABI, quality filtering with NanoFilt and filtering for host reads), perform direct homology search on WGS-derived MinION reads using megablast and NR database, splitting the blast processes by chunks of 10000 reads
```
nextflow run eresearchqut/ontvisc -resume --plant_host_fasta /path_to_host_fasta_file_dir/host.fasta \\
                                --skip_clustering \\
                                --skip_denovo_assembly \\
                                --blastn_db /path_to_blastn_database/nt \\
                                --blast_threads 4 \\
                                --blast_split_factor 10000
```


3. Perform de novo assembly on raw reads using Canu
```
nextflow run eresearchqut/ontvisc -resume --skip_porechop \\
                                  --skip_nanofilt \\
                                  --skip_clustering \\
                                  --skip_host_filtering \\
                                  --canu  --canu_options 'useGrid=false' \\
                                  --blastn_db /path_to_blastn_database/nt \\
                                  --blast_threads 4 \\
                                  --blast_split_factor 5000
                                  

```
4. Perform de novo assembly on raw reads using Flye
```
nextflow run eresearchqut/ontvisc -resume --skip_porechop \\
                                  --skip_nanofilt \\
                                  --skip_clustering \\
                                  --skip_host_filtering \\
                                  --flye \\
                                  --blastn_db /path_to_blastn_database/nt \\
                                  --blast_threads 4 \\
                                  --blast_split_factor 5000
```