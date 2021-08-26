## Usage

You can use the provided demo data to run CLIPipe:

```bash
cp /home/CLIPipe_user/clipipe/demo/general ${workspace}
```

The demo data folder has the following structure:

```text
./
├── config
|   ├── default_config.yaml
│   └── user_config.yaml
├── data
|   ├── fastq/
│   └── sample_ids.txt
└── output
    └── ...
```

```text
Note:
    `config/user_config.yaml`: configuration file with user defined parameters for each step.
    `config/default_config.yaml`: configuration file with additional detailed parameters for each step. The default file is not supposed to be changed unless you are very clear about what you are doing.
    `config/fastq/`: folder of raw CLIP-seq fastq file.
    `data/sample_ids.txt`: table of sample name information.
    `output/example/`: output folder.
```

### Pre-processing

CLIPipe provides pre process step for raw CLIP-seq data. You needs to set up the `config/user_config.yaml` file correctly. The other parameters for pre process step can be found in `config/default_config.yaml`.

```bash
clipipe -u ./config/user_config.yaml pre_process
```

```text
Note:
    The output folder `output/fastqc_raw/` contains quality control results of raw CLIP-seq data.
    The output folder `output/multiqc_raw/` contains summary of all raw sequencing data quality control results.
    The output folders `output/pre_process/` contain the pre process results of raw CLIP-seq data.
```

### Alignment

CLIPipe provides bowtie, bwa and novoalign for mapping CLIP-seq data. You need to set up the alignment tool in the `config/user_config.yaml` file correctly. It is **recommended** to specify the number of threads in `config/user_config.yaml` file by adding `threads_mapping: N`, or you can simply add `-j N` parameter in the CLIPipe command. The other detial parameters for alignment can be found in `config/default_config.yaml`.

```bash
clipipe -u ./config/user_config.yaml mapping
```

```text
Note:
    The output folder `output/mapping_bowtie/` contains alignment results using bowtie.
    The output folder `output/mapping_bwa/` contains alignment results using bwa.
    The output folders `output/mapping_novoalign/` contain alignment results using novoalign.
```

### Peak calling

CLIPipe provides multi peak calling methods for identifying recurring fragments of CLIP-seq data.

```bash
clipipe -u ./config/user_config.yaml peak_calling
```

```text
Note:
    The output folders `output/peak_calling_piranha/` contain alignment results using piranha.
    The output folder `output/peak_calling_CTK/` contains peak calling results using CTK.
    The output folders `output/peak_calling_pureclip/` contain alignment results using pureclip.
    The output folders `output/peak_calling_parclip_suite/` contain alignment results using parclip_suite.
```

Other peak calling tools can be used in the CLIPipe docker directily:

```bash
# iCLIPro
$ iCLIPro [options] in.bam

# iCount
$ iCount [-h] [-v] ...

# JAMM
$ ~/bin/miniconda3/envs/jamm/bin/JAMM.sh --help

# PEAKachu
$ peakachu [-h] [--version] {window,adaptive,coverage,consensus_peak} ...

# PeakRanger
$ peakranger <command> <arguments>

# clipcontext
$ clipcontext [-h] [-v] {g2t,t2g,lst,int,exb,eir} ...
```

### Motif discovery

The motif discovery function can be used directily in the CLIPipe docker:

```bash
# HOMER
$ homer [data] [parameters] -a [action]

# MEME
$ meme <dataset> [optional arguments]

# PhyloGibbs
$ phylogibbs-mp [-m motifwidth] input_seqfile [input_seqfile2 ...]

# STREME
$ streme [options]
```
