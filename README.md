<h1 align="center" style="margin-bottom:0px; padding-bottom:0px; border-bottom:none;">Introduction to Nextflow:</h1>
<p align="center" style="font-size:1.1em; color:gray;">Building Robust { Reproducible, Portable, Scalable } Bioinformatics Workflows</p>

## Instructor
- [Phelelani Mpangase](https://github.com/phelelani) |  [email](mailto:Phelelani.Mpangase@wits.ac.za)

## General Topics

- Use of workflow managent systems for automation/reproducibility
- Introduction to `nextflow`
- Basic syntax of `nextflow`
- Transform and execute a workflow in `nextflow`

## Learning Objectives

- Find and use `nextflow` tool definitions [online](https://www.nextflow.io/docs/latest/index.html)
- Understand the main concepts for "Reproducible Science", i.e., reproducibility, portability & scalability
- Understand the main ingredients for "Reproducible Science", i.e., workflow management systems, containerisation, scaling, documentation & sharing
- Understand how to write `nextflow` scripts and definitions for command line tools
- Understand the concepts of `nextflow` `Channels`, `Processes` and `Channel` operators
- Understand how to handle multiple inputs and outputs in `nextflow`
- Use Docker/Singularity with `nextflow` to provide software dependencies and ensure reproducibility
- Understand `nextflow`'s configuration file (`nextflow.config`), profiles and input parameters
- Run `nextflow` workflows on local, HPC and cloud systems

## Learning Material

- [Reproducible Science Slides](https://phelelani.github.io/nf-intro/slides/reproducibility/)
- [Introduction to Nextflow Slides](https://phelelani.github.io/nf-intro/slides/nextflow/)
- [Nextflow Documentation](https://www.nextflow.io/docs/latest/index.html)

## Schedule
- [Reproducible Science]() - [slide_section](https://phelelani.github.io/nf-intro/slides/reproducibility/)
    - [Big Data & Reproducible Science]() - [slide_section](https://phelelani.github.io/nf-intro/slides/reproducibility/#/big_data)
    - [Workflow Management Systems/Engines]() - [slide_section](https://phelelani.github.io/nf-intro/slides/reproducibility/#/engines)
    - [Containerisation]() - [slide_section](https://phelelani.github.io/nf-intro/slides/reproducibility/#/containers)
    - [Scaling, Documentat`ion & Sharing]() - [slide_section](https://phelelani.github.io/nf-intro/slides/reproducibility/#/scaling)
    - [How the Pieces Fit Together]() - [slide_section](https://phelelani.github.io/nf-intro/slides/reproducibility/#/overall)
- [Introduction to Nextflow]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/)
    - [Big Data & Bioinformatics]() - [slide_section](http://localhost:2026/slides/nextflow/#/bioinf_big_data)
    - [Introduction to Nextflow]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/#/nextflow)
    - [Nextflow {Script, Syntax}]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/#/nextflow_script)
    - [Workflow {Caching,Resuming}]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/#/partial_exec)
    - [Workflow {Visualisation, Tracing}]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/#/tracing)
    - [Generalising {Parameters, Channels}]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/#/extending)
    - [{Docker, Singularity} Containers]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/#/containers)
    - [Workflow Configuration]() - [slide_section](https://phelelani.github.io/nf-intro/slides/nextflow/#/config)


## 1. Nextflow {Script, Syntax}
This tutorial in an introduction to `nextflow`, primarily through examples. Since the tutorial is brief, it is designed to whet your appetite -- we're only going to dip in and out of some of its features in a superficial way.

**Exercises:** Throughout this tutorial there will be some practical examples. Not all will be covered in class for time reasons but you can come back and do them.

First, lets set up a directory where we will do all our `nextflow` exercises:
```bash
mkdir ~/nf-tutorial
```

Then we move into the directory we just created:
```
cd ~/nf-tutorial
```

Now, lets download data we will be using:
```bash
wget https://github.com/phelelani/nf-intro/raw/main/resources/downlads/data.zip
```

**Expected output:**
```bash
--2025-11-23 16:41:48--  https://github.com/phelelani/nf-intro/raw/main/resources/downlads/data.zip
Resolving github.com (github.com)... 20.26.156.215
Connecting to github.com (github.com)|20.26.156.215|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://raw.githubusercontent.com/phelelani/nf-intro/main/resources/downlads/data.zip [following]
--2025-11-23 16:41:49--  https://raw.githubusercontent.com/phelelani/nf-intro/main/resources/downlads/data.zip
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.110.133, 185.199.109.133, 185.199.111.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.110.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4822430 (4.6M) [application/zip]
Saving to: ‘data.zip’

data.zip    100%[=========================================================>]   4.60M  --.-KB/s    in 0.01s   

2025-11-23 16:41:50 (328 MB/s) - ‘data.zip’ saved [4822430/4822430]
```

Then we unzip the downloaded file:
```bash
unzip data.zip
```

**Expected output:**
```bash
Archive:  data.zip
   creating: data/
  inflating: data/2019-XTR-07.dat    
  inflating: data/2018-REG-09.dat    
  inflating: data/2018-SEN-11.dat    
  inflating: data/2016-SEN-09.dat    
  inflating: data/2016-REG-05.dat    
  inflating: data/2016-REG-11.dat    
  inflating: data/2018-SEN-04.dat    
...
```

<!-- https://raw.githubusercontent.com/phelelani/nf-intro/refs/heads/main/resources/downlads/data.zip  -->
<!-- https://github.com/phelelani/nf-intro/raw/main/resources/downlads/data.zip  -->

Type `ls -lR *` or `tree` and hit `<ENTER>` to view the contents of your `nf-tutorial`. The directory will now contain a `data` folder that we will use in this tutorial.
```bash
ls -lR *
```

**Expected output:**
```bash
├── data
│   ├── 11.bim
│   ├── 12.bim
│   ├── 13.bim
│   ├── 14.bim
│   ├── 2016-REG-01.dat
│   ├── 2016-REG-02.dat
...
│   ├── 2019-XTR-08.dat
│   ├── 2019-XTR-09.dat
│   ├── 2019-XTR-10.dat
│   ├── 2019-XTR-11.dat
│   ├── 2019-XTR-12.dat
│   └── pops
│       ├── BEB.bed
│       ├── BEB.bim
│       ├── BEB.fam
│       ├── CEU.bed
│       ├── CEU.bim
│       ├── CEU.fam
│       ├── JPT.bed
│       ├── JPT.bim
│       ├── JPT.fam
│       ├── YRI.bed
│       ├── YRI.bim
│       └── YRI.fam
└── data.zip
```

### **Exercise 1** 
You have an input file with 6 columns (see below), where column 2 is an "index" column. Identify rows that have **identical** indexes (column 2) and remove them from the file. Let's have a look at this file:
```bash
less -S data/11.bim
```

**Expected output:**
```
11	11:189256	0	189256	A	G
11	11:193788	0	193788	T	C
11	11:194062	0	194062	T	C
11	11:194228	0	194228	A	G
11	11:201482	0	201482	T	C
11	11:209089	0	209089	T	C
11	11:209089	0	209089	T	G
11	11:211584	0	211584	T	C
11	11:213272	0	213272	A	G
11	11:214832	0	214832	C	T
```

**Solution - using `bash`:**
```bash
cut -f 2 data/11.bim | sort | uniq -d > dups
grep -v -f dups data/11.bim > 11.clean
```

This is easy to do in `bash` - very simple example, not realistic for `nextflow`

**Solution - using `nextflow`:**
```nextflow
#!/usr/bin/env nextflow
nextflow.enable.dsl=2

input_data = Channel.fromPath("data/11.bim")

process getIDs {
    input:
    path(input_data)

    output:
    path("ids"), emit: ids_channel
    path("11.bim"), emit: original_channel
  
    """
    cut -f 2 ${input_data} | sort > ids
    """
}

process getDuplicates {
    input:
    path(ids_channel)
  
    output:
    path("dups"), emit: duplicates_channel
  
    """
    uniq -d ${ids_channel} > dups
    touch ignore
    """
}

process removeDuplicates {
    input:
    path(duplicates_channel)
    path(original_channel)
    
    output:
    path("clean.bim"), emit: cleaned_channel
    
    """
    grep -v -f ${duplicates_channel} ${original_channel} > clean.bim
    """
}

workflow {
    getIDs(input_data)
    getDuplicates(getIDs.out.ids_channel)
    removeDuplicates(getDuplicates.out.duplicates_channel, getIDs.out.original_channel).subscribe { print "Done!" }
}
```

Using a text editor like `emacs` or `vim`, open the `nextflow` script `clean_duplicates.nf` and have a look at it.

**NB:** The use of `nextflow` variables -- within a double quoted string, there is string interpolation marked with the `$`. If you want to access a system environment variable you need to also escape with a backslash. So in the `nextflow` program, you can normally just refer to `nextflow` variables unadorned with their names (e.g. `$input`) and environment variables with a  dollar (e.g. `$HOME`) but within a double/triple-quoted string it's `\$input` and `\$HOME`. File names can be relative (to the current working directory where the script is being run in, not to the location of the script), or absolute. Great care needs to be taken with using absolute path names since this reduces the portability of scripts, particualarly when you are using Docker. 

Now we can execute our script:
```
nextflow run clean_duplicates.nf
```

**Expected output:**
```bash
 N E X T F L O W   ~  version 25.04.2                                                                                   
                                                                                                                        
Launching `clean_duplicates.nf` [pedantic_allen] DSL2 - revision: 77b3253103                                            
                                                                                                                        
executor >  local (3)                                                                                                   
[1b/48ce3c] getIDs (1)           | 1 of 1 ✔                                                                             
[a3/d202db] getDuplicates (1)    | 1 of 1 ✔                                                                             
[1f/311b59] removeDuplicates (1) | 1 of 1 ✔                                                                             
Done!
```

**NB:** `nextflow` creates a `work` directory, and inside of that are the working directories of each process -- in the example above you can see that the `getIDs` process was launched in a directory with a prefix `48ce3c`, inside the directory `1b`. The directory structure is looks like:
```bash
tree work
```

**Expected output:**
```bash
work                                                                                                                    
├── 1b                                                                                                                  
│   └── 48ce3c27e0a347b06ceed8c32971eb                                                                                  
│       ├── 11.bim -> /home/phelelani/nf-tutorial/data/11.bim                                                           
│       └── ids                                                                                                         
├── 1f                                                                                                                  
│   └── 311b599371287de80732b39719b651                                                                                  
│       ├── 11.bim -> /home/phelelani/nf-tutorial/data/11.bim                                                           
│       ├── clean.bim                                                                                                   
│       └── dups -> /home/phelelani/nf-tutorial/work/a3/d202db0d339647a8a203df8fec6031/dups                             
└── a3                                                                                                                  
    └── d202db0d339647a8a203df8fec6031                                                                                  
        ├── dups                                                                                                        
        ├── ids -> /home/phelelani/nf-tutorial/work/1b/48ce3c27e0a347b06ceed8c32971eb/ids                               
        └── ignore                                                                                                      
                                                                                                                        
7 directories, 8 files 
```

The names of the working directory are randomly chosen so if you run it, you will get different names. Also, each time you run a process ever, it will get a unique working directory. There is  no danger of name clashes Instead of naming the file you get from a channel you can also:
- specify `stdin` if your process expects data to come from `stdin` rather than a named file. `nextflow` will pipe the file to standard input;
- specify `stdout` if your process produces data on `stdout` and you want that data to go into the `channel`

## 2. Workflow {Caching,Resuming}
If execution of workflow is only partial (e.g., because of error), only need to resume from process that failed:
```bash
nextflow run clean_duplicates.nf -resume
```

**Expected output:**
```bash
 N E X T F L O W   ~  version 25.04.2                                                                                   
                                                                                                                        
Launching `cleandups.nf` [grave_descartes] DSL2 - revision: 15cc3f0157                                                  
                                                                                                                        
[18/6d6f96] process > getIDs (1)     [100%] 1 of 1, cached: 1 ✔                                                         
[b1/6260d6] process > getDups (1)    [100%] 1 of 1, cached: 1 ✔                                                         
[a7/1f2cea] process > removeDups (1) [100%] 1 of 1, cached: 1 ✔                                                         
Done!
```

## 3. Workflow {Visualisation, Tracing}
`nextflow` supports several visualisation tools: `-with-dag`, `-with-timeline`, `-with-report`

### 3.1. `dag`
```
nextflow run cleandups.nf -with-dag <file-name>.dot
```
<div align="center">
  <img width="250" src="assets/images/nf_flowchart.svg">
</div>

### 3.2. `timeline`
```
nextflow run cleandups.nf -with-timeline <file-name>.html
```
<p align="center">
  <img width="1000" src="assets/images/nf_timeline.png">
</p>

### 3.3. `report`
```
nextflow run cleandups.nf -with-report <file-name>.html
```
<div align="center">
    <img width="1000"src="assets/images/nf_report.png">
</div>

### 3.4. `preview` (new feature)
```
nextflow run cleandups.nf -with-dag  -preview <file-name>.html
```
<div align="center">
    <img width="250"src="assets/images/nf_preview.png">
</div>

**NB:** For debugging, `-with-trace` option may be useful.

## 4. Generalising and Extending: Parameters & Channel Operators
We'll now extend this example, introducing more powerful features of `nextflow` as well as some of the complication of workflow design.

Extending the example:
- Parameterise the input.
- Want output to go to convenient place.
- Workflow takes in multiple input files -- processes are executed on each in turn.
- Complication: may need to carry the base name of the input to the final output.
- Can repeat some steps for different parameters.

### 4.1. Parameters

Parameters can be specified in a `nextflow` script file:
```nextflow
input_ch = Channel.fromPath(params.data)
```

They can also be passed to the `nextflow` command when executing a script:
```bash
nextflow run phylo1.nf --data data/polyseqs.fa
```

During debugging you may want to specify default values:
```nextflow
params.data = 'data'
```

If you run the `nextflow` program without parameters, it will use this as a default value; if you give it parameters, the default value is replaced. Of course, as a matter of good practice, default values for parameters are really designed for real parameters of the process (like gap penalties in a multiple sequence alignment) rather than data files. `nextflow` makes a distinction between parameters with a single dash (`-`) and with a double dash (`--`). The single dash ones are from a small, language defined subset modifying the behaviour of `nextflow` -- for example we've seen `--with-dag` already. The double-dash parameters are user-defined and completely extensible -- they are used to populate `params`. They modify the behaviour of **your** program.

### 4.2. [Channels](https://www.nextflow.io/docs/latest/channel.html)
`nextflow` channels support different data types:
- `path`
- `stdin`
- `env`
- `tuple`

**NB:** `val` is the most generic -- could be a file name. But sending a file provides power since you can access Groovy's file handling capacity **and**, more importantly does staging of files.

There are different methods for creating channels:
```nextflow
Channel.create()
Channel.empty
Channel.from("blast","plink")
Channel.fromPath("data/*.fa")
Channel.fromFilePairs("data/{YRI,CEU,BEB}.*)
Channel.watchPath("*fa")
```
There are others.

**NB:** The `fromPath` method takes a `Unix` `glob` and creates a new channel which has all the files that match the glob. These files are then emitted one  by one to processes that use these values. This default semantics can be changed using the channel operators that Nexflow provides, some of which are shown below. There are many, many operations you can do on channels and their contents.
```nextflow
bind          buffer        close
filter        map/reduce    group
join, merge   mix           copy
split         spread        fork
count         min/max/sum   print/view
```

### Exercise 2:
SOME EXPLAINATION HERE!

**Solution:**
```nextflow
#!/usr/bin/env nextflow
nextflow.enable.dsl=2

params.data = "data"
input_data = Channel.fromPath("${params.data}/*.bim")

process getIDs {
    input:
    path(input_data)
    
    output:
    path("${input_data.baseName}.ids"), emit: ids_channel
    path("${input_data}"), emit: original_channel
    
    """
    cut -f 2 ${input_data} | sort > ${input_data.baseName}.ids
    """
}

process getDuplicates {
    input:
    path(input_data)

    output:
    path("${input_data.baseName}.dups"), emit: duplicates_channel

    """
    uniq -d ${input_data} > "${input_data.baseName}.dups"
    touch ignore
    """
}

process removeDuplicates {
    publishDir "output", pattern: "${duplicates_channel.baseName}.bim", overwrite:true, mode:'copy'
    
    input:
    path(duplicates_channel)
    path(original_channel)

    output:
    path("${duplicates_channel.baseName}_clean.bim"), emit: cleaned_channel

    """
    grep -v -f ${duplicates_channel} ${original_channel} > ${duplicates_channel.baseName}_clean.bim
    """
}

workflow {
    getIDs(input_data)
    getDuplicates(getIDs.out.ids_channel)
    removeDuplicates(getDuplicates.out.duplicates_channel, getIDs.out.original_channel)
}
```
Here the `getIDs` process will execute once, for each file found in the initial glob. On a machine with multiple cores, these would probably execute in parallel, and as we'll see later if you are running on the head node of a cluster, each could run as a separate job.

**NB:** that in this version of `getIDs` we name the output file dependant on the input file. This is convenient to do because now we are taking many input files. There is no danger of there being any name clashes during execution because each parallel execution of `getIDs` runs in a separate local directory. However, at the end we want to be able to distinguish which output came from which input without having to do detective work -- so we name the files conveniently. Files that get created on the way but don't need at the end we can name boringly.
```bash
nextflow run clean_duplicates_v2.nf
```

**Expected output:**
```bash
 N E X T F L O W   ~  version 25.04.2                                                                                   
                                                                                                                        
Launching `clean_duplicates_v2.nf` [naughty_tesla] DSL2 - revision: 386f5bc813                                          
                                                                                                                        
executor >  local (12)                                                                                                  
[07/1384ed] getIDs (4)           | 4 of 4 ✔                                                                             
[e4/90decd] getDuplicates (4)    | 4 of 4 ✔                                                                             
[66/b3483e] removeDuplicates (4) | 4 of 4 ✔  
Done! 
```


## 4. `nextflow` + [Docker](https://www.nextflow.io/docs/latest/docker.html) & [Singularity](https://www.nextflow.io/docs/latest/singularity.html) Containers

Light-weight virtualisation abstraction layer:
- Currently run on `Unix` like systems (e.g., Linux, macOS).
- Windows support coming...

You create Docker/Singularity images locally, or get from repositories ():
Getting Docker images from repositories:
```bash
docker pull ubuntu
docker pull quay.io/banshee1221/h3agwas-plink
```

Getting Singularity images from repositories:
```bash
singularity pull docker://ubuntu
singularity pull docker://quay.io/banshee1221/h3agwas-plink
```

Running Docker
```bash
docker run <some-image-name>
```

Running Singularity
```bash
singularity exec <some-image-name>
```

Docker and Singularity often run images in background (e.g. webserver), but can also run interactively:
```bash
## Running Docker interactively
sudo docker run -t -i quay.io/banshee1221/h3agwas-plink

## Running Singularity interactively
singularity shell docker://quay.io/banshee1221/h3agwas-plink
```

`nextflow` supports Docker & Singularity
- Well designed script should be highly portable
- Each process gets run as a separate Docker call (e.g, under the hood, a `docker run` is called)
- Can use the same or different Docker images for each process, parameterisable

### Exercise:

Create `plink.nf`
```nextflow
#!/usr/bin/env nextflow
nextflow.enable.dsl=2
 
params.dir = "data/pops/"
dir = params.dir
params.pops = ["YRI","CEU","BEB"]
 
Channel
    .fromFilePairs("${params.dir}/{YRI,BEB,CEU}.{bed,bim,fam}",size:3) {
        file -> file.baseName 
    }
    .filter { key, files -> key in params.pops }
    .set { plink_data }
 
process checkData {
    input:
    tuple val(pop), path(pl_files)
    
    output:
    path("${pop}.frq"), emit: result
    
    """
    plink --bfile $pop --freq  --out $pop
    """
}

workflow {
    checkData(plink_data).view()
}
```

```bash
nextflow run plink.nf
```

I set you up for failure there! The above will not run because `plink` is not installed in the system. Now try running with a Docer container with `plink`:
```
nextflow run plink.nf -with-singularity docker://phelelani/misc:plink 
```

Now, even if you **don't** have `plink`, your script will work because my Docker/Singularity image has `plink` insalled!

##  Executors
<img width="50" src="assets/icons/construction.gif">

## Configuration
<img width="50" src="assets/icons/construction.gif"> 

## 5. Best Practices
<p align="center">
  <img width="1000" src="assets/images/best_practices.svg">
  Best practices for the development, maintenance & sharing of robust reproducible, portable & scalable workflows.
</p>