## Installation

### Use CLIPipe Docker

#### Prepare reference demo and docker

1. Download the reference and demo data to directory, and unzip

         mkdir clipipe_test;
         cd clipipe_test;
         wget ;
         wget ;
         wget ;
         tar -xvzf clipipe_ref.tar.gz;
         tar -xvzf clipipe_demo.tar.gz;

#### Run CLIPipe Docker

1.  [Docker](https://www.docker.com/) provides an easy way to run CLIPipe in a working environment that is completely separated from your host machine. All required software and packages are already installed in a ready-to-use image of CLIPipe docker, so there are no more requirements. you can use the docker image we provide: [CLIPipe Docker Image](https://hub.docker.com/). You can execute to get the docker `CLIPipe_1.0.X` container:
         
         cd clipipe_test;
         docker import CLIPipe_v1.0.2_.tar.gz zs/clipipe:1.0.2_test     ##import the docker

         docker run --name=CLIPipe_1.0.2_test -t -d -h CLIPipe_docker --restart unless-stopped -v <the-absolute-path-of-current-directory>:/home/CLIPipe_user/clipipe zs/clipipe:1.0.2_test /bin/bash

    -   Make sure to create a local folder and provide the path to it. The example above uses a path that may not be applicable to your computer. Both, path to the folder on the host machine and path within the container (`/home/CLIPipe_user/clipipe`), must be absolute.

2.  To show the docker `CLIPipe_1.0.2_test` container, you can execute:

         docker container ls

3.  To execute the `CLIPipe_1.0.2_test` container, you can execute:

         docker exec -it CLIPipe_1.0.2_test bash

4.  After entering the container, please change the user to `CLIPipe_user`

         su CLIPipe_user;su 
         cd ~

5.  To test the installation and get information about the command-line interface of CLIPipe, you can execute:

         clipipe --help

    A helper message is shown like this:

         usage: clipipe [-h] --user_config_file USER_CONFIG_FILE
                        {pre_process,mapping,peak_calling}

         CLIPipe: A comprehensive quality control and analysis pipeline for CLIP highthroughput sequencing data
         =======================================================================================================
         CLIPipe is a Python module and associated command-line interface (CLI), which provides all the
         commands needed to process protein-RNA CLIP interaction data and to identify and quantify
         sites of protein-RNA interactions on RNA.

         CLIPipe's main input are FASTQ files with iCLIP sequencing data, its main output are BED files
         with identified and quantified cross-linked sites.

         A number of analyses are included in CLIPipe that provide insights into the properties of
         protein-RNA interaction.

         optional arguments:
            -h, --help                show this help message and exit
            -u, --user_config_file    the user config file

         positional arguments:
            {pre_process,mapping,peak_calling}

         =======================================================================================================
         For additional help or support, please visit https://github.com/ShangZhang/clipipe

### Use from source

We have hide the details of each step which write by [Snakemake](https://snakemake.readthedocs.io/en/stable/) and you only need to run one single command. However you can use some of the codes if you are familiar with snakemake. The source code is [here](https://github.com/ShangZhang/clipipe).
