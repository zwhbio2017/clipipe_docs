# CLIPipe

CLIPipe (CLIP-seq Pipeline) is an integrated pipeline for analyzing CLIP sequencing data, which provides all the commands needed to process protein-RNA CLIP-seq interaction data and to identify and quantify sites of protein-RNA interactions on RNA.

CLIPipe's main input are FASTQ files with CLIP-seq data, its main output are BED files with identified and quantified cross-linked sites.

A number of analyses are included in CLIPipe that provide insights into the properties of protein-RNA interaction.

![Pipeline of Tutorial](img/CLIPipe_pipeline.png)

The CLIPipe workflow consists of:

-   Pre-processing function:
    -   Quality control, remove adapter, filter low quality reads, collpase duplicates, remove barcode of the raw CLIP-seq data.
-   Alignment function:
    -   mapping sequencing data to reference genome using bowtie, bwa and novoalign
-   Peak calling function:
    -   Peak calling of the CLIP-seq data using Piranha, CTK, PureCLIP, iCLIPro, iCount, JAMM, PEAKachu, PeakRanger and clipcontext
-   Motif discovery function:
    -   Motif discovery of the CLIP-seq data using HOMER, PhyloGibbs, MEME, GraphProt, DREME and STREME

## Table of Contents:

-   [Requirements](1_requirement.md)
-   [Installation](2_installation.md)
-   [Basic Usage](3_basic_usage.md)
-   [Usage](4_usage.md)
    -   [Pre-processing](4_usage.md#rre-processing)
    -   [Alignment](4_usage.md#alignment)
    -   [Peak calling](4_usage.md#peak-calling)
    -   [Motif discovery](4_usage.md#motif-discovery)
-   [Frequently asked questions](5_faq.md)
-   [Citation](#citation)
-   [Copyright and License](6_copyright_and_license.md)

This documentation is for CLIPipe version: 0.0.1.dev