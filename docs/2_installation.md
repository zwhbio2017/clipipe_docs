## Installation

1.  All required software and packages are already installed in docker, so there are no more requirements. You can execute to get the docker `CLIPipe_v3` container:

         docker run --name=CLIPipe_v3 -t -d -h CLIPipe_docker --restart unless-stopped -v /lulab/lustre2/zhangshang/work/software/clipipe:/home/CLIPipe_user/clipipe zs/clipipe:v1 /bin/bash

2.  To show the docker `CLIPipe_v3` container, you can execute:

         docker container ls

3.  To execute the `CLIPipe_v3` container, you can execute:

         docker exec -it CLIPipe_v3 bash

4.  After entering the container, please change the user to `CLIPipe_user`

         su CLIPipe_user;
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

         positional arguments:
            {pre_process,mapping,peak_calling}

         optional arguments:
            -h, --help            show this help message and exit
            --user_config_file USER_CONFIG_FILE, -u USER_CONFIG_FILE
                                 the user config file

         =======================================================================================================
         For additional help or support, please visit https://github.com/ShangZhang/clipipe
