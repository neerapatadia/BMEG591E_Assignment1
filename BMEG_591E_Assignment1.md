BMEG\_591E\_Assignment1
================
Neera Patadia
21/01/2022

# Assignment Overview

For this assignment we will be using data from a Chromatin
Immunoprecipitation sequencing (ChIP-seq) produced by Choi, Won-Young,
et al., (2020). *<https://pubmed.ncbi.nlm.nih.gov/33293480/>*. This data
is part of a series of experiments that aimed to understand the
chromatin changes that happen when an induced Pluripotent Stem Cell
(iPSC) undergo differentiation towards a Neuronal Progenitor Cell (NPC).
The **fastqc** files from this experiment have been pre-downloaded and
added to the server, under the following path: **/projects/bmeg/A1/**.
The datasets that we will be using for the assignment are in a shared
*reading only* location that you can read but ARE NOT allowed to alter.

This assignment has 3 main goals:

1.  Get familiar with the server

2.  Manage a conda environment

3.  Perform an alignment against the human genome

4.  Cleaning your folder and uploading your assignment to Github

IMPORTANT: Every time you see *\#?\#* means, that’s a question you need
to answer, with a command or with an interpretation of the data. When an
instruction follows \#?\# you need to type the command you used to
complete the task. Please make sure to check for them and answer them
all, as this is how your assignment will be graded.

## 1\. Getting Familiar with the Server

For our course we will be using a server based on a Docker/Linux system.
This will be the place where you are gonna do the on-class practices and
the assignments. There are a couple of things that you need to be aware
while working on the server. The server has limited storage and computer
power. Therefore, you will be required to be mindful of the processes
you run as well as the files you keep.

To join the server you will need to be on an active connection of a UBC
Virtual Private Network (VPN). If you do not have one already, you can
check how to install it here:
*<https://it.ubc.ca/services/email-voice-internet/myvpn/setup-documents>*.
Once the VPN has been set, you will need to open a terminal.

  - **Windows system:**
    1.  Install a terminal emulator like Putty
        (*<https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html>*).
    2.  This will allow you to do a SSH connection using the server’s IP
        (orca1.bcgsc.ca) and your credentials.
  - **Linux/Unix system (Apple computer and Ubuntu):**
    1.  Open a terminal
    2.  Type the following command: ssh <username@orca1.bcgsc.ca>
    3.  When prompted, type your password to complete the login

Now that you have successfully joined the server, let’s get you used to
some basic commands

### a. Check a directory’s path

When using terminal, paths work as the addresses of the files and
directories you want to access and easily move between them. Once you
have logged into the server you will be in your *home directory*.

``` bash
#?# Check your current directory's path: 0.5 pt

pwd
```

### b. Creating a directory

``` bash
#?# On your home directory create a new directory (folder) - 1pt
## This folder will host the files created throughout this assignment 

mkdir assignment1_bmeg591e
```

*Note:* Generally speaking, is good to follow naming convention when
using the terminal. Remember:

  - Do not start a name with a number

  - Names are case sensitive (ASSIGNMENT1.txt and assignment1.txt are
    not the same)

  - Avoid to use spaces, as they are interpreted as special characters
    by the computer

  - Use \_ or - to replace spaces

  - File extensions are meaningful for us to know the file format but in
    terminal you can use it as part of the file name. Ex: you will be
    able to open a .sequences file that has tab delimited information

### c. Moving within directories

Access your newly created directory

``` bash
#?# Type the command you need to use to move to your newly created directory - 0.5 pt 
## Tip: look at the *cd* command documentation

cd assignment1_bmeg591e
```

How would you move back to your home directory
(*/home/your\_user\_name*)?

``` bash
#Check the tutorial: https://www.computerhope.com/unix/ucd.htm 
#?# Using the complete directory path of your home directory: - 0.25 pt
cd /home/npatadia_bmeg22

#?# Using the "go to parent directory" shortcut - 0.25
cd ../
```

### d. Explore the dataset

The sequencing data that we will be using is paired-end. This means that
each sequence has been sequenced twice, one on each end (5’ and 3’).
Choose **one** of the reads files (1 or 2) for the following exercises.

``` bash
#?# Look at the first 3 rows of the dataset - 0.25
# Tip: look at the *head* command documentation 
head -n 3 input_iPSC_SRA66_subset_2.fastq

#?# Look at the last 7 rows of the dataset - 0.25
# Tip: look at the *tail* command documentation 
tail -n 7 input_iPSC_SRA66_subset_2.fastq

#?# Explore the file with the *less* command  - 0.25
less input_iPSC_SRA66_subset_2.fastq
```

### e. Piping

Because this is a very large dataset we will proceed to subset it in
order to make it more manageable for the assignment. Using the commands
that you learned above:

``` bash
#?# How many lines does the file was in total? - 0.25
#Tip: loop at the *wc* command documentation
wc -l input_iPSC_SRA66_subset_2.fastq 

#the file contains 10459640 lines based on the output from the above command


#?# Select only the id lines (e.g. @SRR12506919.667552 667552 length=151) of the dataset (the ones that start with @ and are followed by the read id) - 0.75 pt
## Tip: look at the *grep* command
grep @SRR input_iPSC_SRA66_subset_2.fastq 

#?# How many reads are in the file (i.e., how many id lines are in the file)? - 1 pt
## Tip: Try using * | head * after the command line you use for the previous question
grep @SRR input_iPSC_SRA66_subset_2.fastq | wc -l

#based on this command, there are 2614910 reads in the output

#?# Select only the reads id (e.g. @SRR12506919.667552) from the id lines - 1 pt
## Tip: Look into the *cut* command. Carefully read the default delimiter, is it the case for our file?
grep @SRR input_iPSC_SRA66_subset_2.fastq | cut -d "." -f 1,3

#default deliminator is not the case for out file. Need to be changed to "."
```

### f. Saving an output

``` bash
#?# Save a file that contains only the reads ids (the result of our previous exercise). - 0.5 pt
grep @SRR input_iPSC_SRA66_subset_2.fastq | cut -d "." -f 1,3 > /home/npatadia_bmeg22/assignment1_bmeg591e/read_ids.txt

#?# Now, list all the files in a directory: 
ls -al

#?# What do you see? Was the subset file created correctly? - 0.25 pt
npatadia_bmeg22@orca01:~/assignment1_bmeg591e$ ls -al
total 55776
drwxr-xr-x 2 npatadia_bmeg22 orca_users     4096 Jan 23 00:29 .
drwxr-xr-x 4 npatadia_bmeg22 orca_users     4096 Jan 21 10:46 ..
-rw-r--r-- 1 npatadia_bmeg22 orca_users 56874655 Jan 23 00:29 read_ids.txt

#using the cat command shows all of the read ids, each printed on a separate line. Therefore, I can conclude that the file was subsetted correctly.


## -
```

### g. Creating a backup

There will be times where you will want to save a copy of a dataset or
file before modifying in case something goes wrong. For those cases, you
can create a copy of any file or directory using the “copy” command

``` bash
#?# Create a copy of the reads ids file - 0.25 pt
## Tip: man cp 
cp read_ids.txt /home/npatadia_bmeg22/assignment1_bmeg591e/read_ids_copy.txt

#?# Change the name of the backup reads id file - 0.25 pt
## Tip: man mv
mv read_ids_copy.txt read_ids_backup.txt 

#?# Delete the copy of the reads id file - 0.25 pt
rm read_ids_backup.txt
```

## 2\. Managing Conda Environments

### a. Installing conda

Conda is a package and environment manager
(*<https://docs.conda.io/en/latest/>*). It helps to create different
virtual environments where you can have several packages installed to
meet different needs.

``` bash
## 1. Move to your home directory 
#?# 2. Use the wget command to get the following file: https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
## The file extension .sh pertains to a bash script, the script you just downloaded contains all the instructions needed to install Conda to your home directory
wget https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh

#?# 3. Run the Anaconda3-2021.11-Linux-x86_64.sh bash script. IMPORTANT: say yes to all the steps when prompted! 
chmod +x Anaconda3-2021.11-Linux-x86_64.sh
./Anaconda3-2021.11-Linux-x86_64.sh

## 4. To finalize the installation you need to log out your current session and log in back to the server
```

### b. Create a conda environment

In order to run the reads alignments against the human genome, there are
a few tools that we will need:

  - fastQC
    (*<https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>*):
    comprehensive quality control measures of sequencing data.

  - bowtie2 (*<http://bowtie-bio.sourceforge.net/bowtie2/index.shtml>*):
    alignments of sequencing data.

  - samtools (*<http://www.htslib.org/doc/samtools.html>*): set of tools
    to manage sam/bam alignment files

  - htop & screen: bash system tools to visualize the server capacity
    and to run commands in the background, respectively.

To install them, we will be making use of the conda environments. Conda
allows you to create and manage environments for any programming
language. Managing this environments mean that you can work with
specific versions of different programs at any given moment, simply by
loading the desired environment. You can find more information about
this resource here: *<https://docs.conda.io/en/latest/>* .

``` bash
#?# Create a new conda environment: - 0.5 pt
# Tip: Consult the previously provide links or consult the conda create help (conda create --help)

conda create -n assignment1_env
```

### b. Add programs to your conda environment

Now that the environment has been created, its time to add the packages
that we will need. Conda has an active community and a great
documentation
(*<https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>*)
which you can use throughout the course to help answer any questions you
may have.

``` bash
#?# Add fastQC bowtie2 to your conda environment: - 1 pt
conda install -c bioconda fastqc bowtie2

## Run the following command to install samtools onto your conda environment:
conda install -c bioconda samtools

#?# Add screen to your conda environment: - 1pt 
conda install -c conda-forge screen

#?# Add htop to your conda environment: - 1pt
conda install -c conda-forge htop
```

## 3\. Performing Alignments

### a. Data quality check

We will use the widely used fastQC software to do a quick inspection of
the data quality. Once it has ran, it will give you an html report of
the quality of the data, that you can open using a web browser. More
information on how to read the output can be found here:
*<https://dnacore.missouri.edu/PDF/FastQC_Manual.pdf>* and in the tool’s
website.

``` bash
#?# Run fastQC on the fastq files: - 1pt
## In order to open the html files on your web browser you will need to download the files to your computer
## Ubuntu/Linux and Mac users: look at the *scp* command
## Windows users: follow the following instructions: https://stackoverflow.com/questions/6217055/how-can-i-copy-a-file-from-a-remote-server-to-using-putty-in-windows

fastqc /projects/bmeg/A1/input_iPSC_SRA66_subset_2.fastq -o /home/npatadia_bmeg22/assignment1_bmeg591e/


#?# What can you say about the data quality? - 2 pt 
"""
When examining the per base sequence quality plot for SRA66 subset 2, the quality scores are quite high (all in the green zone of the quality graph). We can also see that the quality drops off at the end of the sequence, especially in the 150-151 basepair region. This is not unexpected as at all the clusters formed by illumina reads do not grow at the same rate, therefore the terminal ends of the read will be more error prone. 
"""

# ---
```

### b. Running a process on the background: screen

The processes that we are about to run, can take a long time to finish.
Thus, we will make use of the *screen* tool, which allows you to run a
process in the background while continue using your server session
without the risk of ending the process due to a bad internet connection,
accidentally closing the tab or other random circumstances.

``` bash
## To run a process in a background screen with screen you:
# 1. Run the following command: 
script /dev/null
# 2. Activate your conda environment 
# 3. Start a background process with a specific name 
screen -S background_screen_name 
# 4. Run the process and any commands you wish, for example:
wc -l /projects/bmeg/A1/input_iPSC_SRA66_subset_1.fastq
# 5. Get out of the background screen, you will need to type the following:
ctrl + A + D 
# 6. Return to the background screen to check the process
screen -r background_screen_name
# 7. Terminate the background screen once the process has ended
# Within the background screen type:
exit 
```

### c. Checking the server capacity and your current runs: htop

Another way to check on the progress of your runs and the server status
is with the *htop* command. This will open a screen showing all the
processes that are being currently being run in the server. Our server
only has 2 cpu’s/cores, the green bar next to each code, represents how
much of that node it is currently in use. Always make sure to check the
processes that are being run before sending yours.

``` bash
#?# Use the htop command to describe the status of the server - 1.5 pt
htop

"""
according to the htop command, I was the only user listed as running processes. There are 7 tasks list, 5 of which are considered to be running. 66.2G/126G of memory are being used. 0% of the cpu is being used and 0%MEM is being used (based on the below table). 
"""
```

### d. Genome indexing - bowtie2

Now, we will need to create an index of the human genome that bowtie2
will use to map our sequences in the genome. In order to do this, you
will need to use the previously downloaded files of the human genome
with the desired build (e.g. hg19,hg38), you can find those files within
the server here: */projects/bmeg/indexes/*

**BEFORE RUNNING ANYTHING**: go to the “Other resources” section at the
end of the assignment\!

``` bash
## Something useful to do when using a new software is to look at the documentation using the *help* option
## Try running: 
bowtie2 -h 
## IMPORTANT!!!!
## BEFORE RUNNING: go to the "Other resources" section at the end of the assignment!!
## __________________________________________________________________________________
#?# Use the hg38 build to create an index of the human genome with bowtie2
## Tip: look into the bowtie2-build help (bowtie2-build --help)  - 1.5 pt

bowtie2-build hg38.fa bowtie
```

### e. Alignment

We are working with paired-end data. Thus, you will need to make sure to
use both fastq files to align the sequences to the genome.
**IMPORTANT:** Run with *default parameters* DO NOT specify any
non-essential paramaters.

**Time flag**: This step will take up to 30 mins

``` bash
#?# Perform a paired-end alignment of the fastq sequences provided (located here: /projects/bmeg/A1/) to the human genome index build (located here: /projects/bmeg/indexes/hg38/ ) - 2pt
## Tip: look at the bowtie2 --help or the documentation on the tool website (http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml#getting-started-with-bowtie-2-lambda-phage-example)

bowtie2 -x hg38_bowtie2_index -1 /projects/bmeg/A1/input_iPSC_SRA66_subset_1.fastq -2 /projects/bmeg/A1/input_iPSC_SRA66_subset_2.fastq -S /home/npatadia_bmeg22/assignment1_bmeg591e/hg38_alignment_bowtie2.sam
```

### f. Viewing the alignments

Now, we will make use of **samtools** to review some basic features of
the alignment. For more information about this tool:
*<http://www.htslib.org/doc/>*

``` bash
## Use *samtools view* to get the:
## Tip: check out the different flag (-f and -F) options
## Tip: Read the samtools view --help, read carefully for an option that allows you to *count* the results of your search
#?# Number of mapped reads - 1 pt
samtools view -F 4 hg38_alignment_bowtie2.sam -o mapped_reads.sam
wc -l mapped_reads.sam
# 5195744 mapped reads

#?# Number of unmapped reads - 1 pt
samtools view -f 4 hg38_alignment_bowtie2.sam -o unmapped_reads.sam
wc -l unmapped_reads.sam
#34076 unmapped reads
```

## 4\. Cleaning and Uploading

### a. Cleaning your folders

Before signing up, we need to make sure that we won’t leave behind any
big files that can take up a lot of memory from our server. To do this,
make sure to:

1.  Delete any copies of the input assignment files you might have done
    on your personal folder

2.  Zip or delete all the files used for the assignment

### b. Uploading to Github

1.  Make sure to create a Github account of your own.

2.  Once you have set it up, create a new public repository for the
    course.

3.  Use the “upload file” option to upload your assignment Rmd to the
    website.

4.  Upload the link of your github repository where the assignment file
    is located onto Canvas

## Other Resources

### a. Bowtie2 index

Indexes take a lot of computational resources and time to run. The one
we need for the human genome will take around 3 hours to be done. ***DO
NOT RUN THE INDEX COMMAND***. Go on to the next step, using the
previously run index: *hg38\_bowtie2\_index* under the following path:
*/projects/bmeg/indexes/*

### b. Other

You can get up to 20 points, if you answer all the questions correctly.
Make sure to consult the resources given through the assignment, they
are meant to make the assignment easier.
