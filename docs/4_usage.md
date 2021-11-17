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

### User config file
The user config file is shown like this:
```text
# default config
default_config_file: /home/CLIPipe_user/clipipe/demo/general/config/default_config.yaml

# basicfigfile paths
root_dir: /home/CLIPipe_user/clipipe/clipipe_software

reference_dir: /home/CLIPipe_user/clipipe/clipipe_ref/Human
reference_dir2: /home/CLIPipe_user/clipipe/clipipe_ref2/Human_transcriptome_reference     ##special for parclip_suit software

data_dir: /home/CLIPipe_user/clipipe/demo/general/data
temp_dir: /home/CLIPipe_user/clipipe/demo/general/temp
output_dir: /home/CLIPipe_user/clipipe/demo/general/output
summary_dir: /home/CLIPipe_user/clipipe/demo/general/summary

# general parameters
threads_compress: 2
threads_mapping: 6

# pre process parameters
barcode_length: 1
paired_end: false

# mapping parameters
# choose from bowtie bwa and novoalign
aligner: bwa

adaptor1: AGATCGGAAGAGCACACGTCTGAACTCCAGTCAC
adaptor2: AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT

# peak calling parameters
# choose from Piranha, PureCLIP, CLIPper, CTK, MiClip
peak_caller: CTK

read_length: 100
```

### Pre-processing

CLIPipe provides pre-process step for raw CLIP-seq data. You needs to set up the `config/user_config.yaml` file correctly. The other parameters for pre-process step can be found in `config/default_config.yaml`.

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

|            | nonspecific                | Method-specific |
| ---------- | -------------------------- | --------------- |
| HITS-CLIP  | Piranha                    | CTK             |
| PAR-CLIP   | PARA suite                 |                 |
| iCLIP      | PureCLIP, iCLIPro,  iCount |                 |
| iCLAP      | PureCLIP, iCLIPro,  iCount |                 |
| eCLIP      | PureCLIP, iCLIPro,  iCount |                 |
| 4sU-iCLIP  | PureCLIP, iCLIPro,  iCount |                 |
| urea-iCLIP | PureCLIP, iCLIPro,  iCount |                 |
| BrdU-CLIP  | PureCLIP, iCLIPro,  iCount |                 |
| Fr-iCLIP   | PureCLIP, iCLIPro,  iCount |                 |
| FAST-iCLIP | PureCLIP, iCLIPro,  iCount |                 |
| irCLIP     | PureCLIP, iCLIPro,  iCount |                 |
| seCLIP     | PureCLIP                   |                 |
| uvCLAP     | JAMM, PEAKachu             |                 |
| FLASH      | PureCLIP                   |                 |
| dCLIP      | PeakRanger                 |                 |

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

for HOMER, the demo script like this:

```bash
# input: ${sample_id}.all_peak.bed

# 1. split training and test dataset

perl /home/CLIPipe_user/clipipe/clipipe_software/bin/homer/1.split.pl ${sample_id}.all_peak.bed

# 2. prepare training and test fasta

perl /home/CLIPipe_user/clipipe/clipipe_software/bin/homer/2.prepare_Homer.pl ${sample_id} training ${genome_fasta}
perl /home/CLIPipe_user/clipipe/clipipe_software/bin/homer/2.prepare_Homer.pl ${sample_id} test ${genome_fasta}

# 3. Run Homer on training dataset

findMotifs.pl ${sample_id}.training_peak.fa fasta Homer_training_output -len 4,5,6,7,8,9,10 -rna # the number of len could change

# 4. Run Homer on test dataset

mkdir Homer_test_output
findMotifs.pl ${sample_id}.test_peak.fa fasta Homer_test_output -rna -find Homer_training_output/homerMotifs.all.motifs > Homer_test_output/count.txt

```

for MEME, the demo script like this:

```bash
# input: ${sample_id}.all_peak.bed

# 1. split training and test dataset

perl /home/CLIPipe_user/clipipe/clipipe_software/bin/meme/1.split.pl ${sample_id}.all_peak.bed

# 2. prepare training and test fasta

perl /home/CLIPipe_user/clipipe/clipipe_software/bin/meme/2.prepare_MEME.pl ${sample_id} training ${genome_fasta}
perl /home/CLIPipe_user/clipipe/clipipe_software/bin/meme/2.prepare_MEME.pl ${sample_id} test ${genome_fasta}

# 3. run MEME on training dataset

mkdir MEME_output
meme ${sample_id}.training_peak.fa -o MEME_output -dna -minw 4 -maxw 10 -nmotifs 25 # the number of minw, maxw and nmotifs could change

# 4. run FIMO on test dataset

cat MEME_output/meme.txt | sed 's/10.0e+000/1.0e+001/g' | sed 's/10.0e+001/1.0e+002/g' | sed 's/10.0e+002/1.0e+003/g' | sed 's/10.0e+003/1.0e+004/g' | sed 's/10.0e+004/1.0e+005/g' | sed 's/10.0e+005/1.0e+006/g' | sed 's/10.0e+006/1.0e+007/g' | sed 's/10.0e+007/1.0e+008/g' | sed 's/10.0e+008/1.0e+009/g' | sed 's/10.0e+009/1.0e+010/g' | sed 's/10.0e+010/1.0e+011/g' > meme.txt
fimo --thresh 0.01 -o FIMO_output meme.txt ${sample_id}.test_peak.fa # thresh可改

```

other related tools are provided

```bash
# PhyloGibbs
$ phylogibbs-mp [-m motifwidth] input_seqfile [input_seqfile2 ...]

# STREME
$ streme [options]
```
