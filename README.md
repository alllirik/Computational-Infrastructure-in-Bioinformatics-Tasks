# Computational-Infrastructure-in-Bioinformatics-Tasks

# 1 - Dependencies management

***git branch name:*** dependencies

## Theory \[2\]

As usual, we will start with a few theoretical questions:

-   \[0.5\] What is Docker, and how it differs from dependencies
    management systems? From virtual machines?

Docker is a set of platform as a service products that use OS-level
virtualization to deliver software in packages called containers, that
can run on any Linux, Windows, or macOS computer. This makes it possible
for the application to run in many environments, including on-premises
and private or public clouds. A single server or virtual machine may
execute several Docker containers at once since they are lightweight.

Unlike other dependencies management systems, Docker is very fast,
flexible and secure. Docker\'s technology is unique because it focuses
on the requirements of developers and systems operators to separate
application dependencies from infrastructure, and it also has a high
security rate.

Docker differs a lot from virtual machines. The primary difference
between these two technologies is that whereas Docker uses virtualized
versions of the same operating system, VMs run as virtual environments
on the same hardware. In comparison, while VMs have poor performance and
a lengthy startup process, Docker has shorten boot-up timing. This is
due to the fact that VMs have to launch an entire OS for just a program
installation. Also, containers are much smaller than VMs.

------------------------------------------------------------------------

-   \[0.5\] What are the advantages and disadvantages of using
    containers over other approaches?

Containers have many advantages, such as:

-   They don\'t take much space since they run on the cloud
-   They are known to their stability
-   They can run on any Linux, Windows, or macOS computer
-   They allow planning a variety of tasks

However, despite those huge advantages, there are some downsides:

-   It\'s not really suited for quick deletion
-   Documentation updates slower than the Docker itself, which may led
    to some problems

------------------------------------------------------------------------

-   \[0.5\] Explain how Docker works: what are Dockerfiles, how are
    containers created, and how are they run and destroyed?

A Dockerfile is a text document that contains all the commands a user
could call on the command line to assemble an image.

In order to create a container, you need to execute following commands:

    touch Dockerfile
    nano Dockerfile #open the file in text editor

Then, add the following content:

    FROM ubuntu
    RUN apt-get update

Next, build an image

    docker build -t image_name

Image will be availabe in the repository.

Now, to create a container, execute:

    docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

To destroy a container, execute:

    docker kill [OPTIONS] CONTAINER [CONTAINER...]

------------------------------------------------------------------------

-   \[0.25\] Name and describe at least one Docker competitor (i.e., a
    tool based on the same containerization technology).

**CoreOS Rkt**

You may execute application workloads independently of the underlying
infrastructure using CoreOS rkt, a containerization software engine. It
is the biggest opponent of the Docker container engine.

CoreOS rkt is built on the Application Container Image (ACI) from the
App Container standard (appc). An ACI is a tarball that contains the
root file system and all the files necessary to execute an application,
as well as an image manifest. The bulk of the default options are
modifiable at runtime, and users can add their own execution parameters
to a specific image using the rkt run command.

As a binary file that interacts with Linux init systems and scripts and
operates in line with the parent-child Unix process paradigm, CoreOS rkt
containers are supported by the majority of the main Linux OS versions.
CoreOS rkt uses pods, which are groups of concurrently executing
processes, for container setup. A pod may have numerous settings,
ranging from the pod level all the way down to particular applications
inside the pod, for varied scopes. Pods run autonomously and without a
centralized daemon in outlying areas.

------------------------------------------------------------------------

-   \[0.25\] What is conda? How it differs from apt, yarn, and others?

Running on Windows, macOS, Linux, and z/OS, Conda is an open source
package management and environment management system. Conda installs,
executes, and updates packages as well as the dependencies on them fast.
On your own computer, Conda makes creating, saving, loading, and
switching between environments simple. It was designed to package and
distribute Python applications, but it can do the same for any
language\'s software.

More people use Conda for development requirements. In comparison with
yarn, it has a lot more of developer tools. As for difference with apt,
conda enables the user to operate locally in contexts other than the
root/bin directory, allowing them to utilize various Python versions in
each environment, for instance.


## Problem \[6.5\]

The problem itself is relatively simple.

Imagine that you developed an excellent RNA-seq analysis pipeline and
want to share it with the world. Based on your experience, you are
confident that the popularity of the pipeline will be proportional to
its ease of use. So, you decided to help your future users and to pack
all dependencies in a Conda environment and a Docker container.

Here is the list of tools and their versions that are used in your work:

-   [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/),
    v0.11.9
-   [STAR](https://github.com/alexdobin/STAR), v2.7.10b
-   [reat](https://github.com/alnfedorov/reat), commit
    ee19bc928badd227975410a6b8b715c0e03bd4ab
-   [samtools](https://github.com/samtools/samtools), v1.16.1
-   [picard](https://github.com/broadinstitute/picard), v2.27.5
-   [salmon](https://github.com/COMBINE-lab/salmon), commit tag 1.9.0
-   [bedtools](https://github.com/arq5x/bedtools2), v2.30.0
-   [multiqc](https://github.com/ewels/MultiQC), v1.13


**Anaconda**:

-   \[1\] Install conda, create a new virtual environment, and install
    all necessary packages.
-   \[0.75\] You won\'t be able to install some tools - that\'s fine.
    List their names, and explain what should be done to make them
    conda-friendly
    ([conda-forge](https://conda-forge.org/docs/maintainer/adding_pkgs.html)
    channel,
    [bioconda](https://bioconda.github.io/contributor/workflow.html)
    channel).
-   \[0.25\]
    [Export](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#exporting-the-environment-yml-file)
    the environment
    ([example](https://github.com/nf-core/clipseq/blob/master/environment.yml))
    to the file and verify that it can be
    [rebuilt](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file)
    from the file without problems.

**Docker**:

-   \[3\] Create a Dockerfile for a container with **all** required
    dependencies. Don\'t forget about comments; test that all tools are
    accessible and work inside the container. Hints:
-   If needed, grant rights to execute downloaded/compiled binaries
    using chmod (`chmod a+x BINARY_NAME`)
-   Move all executables to \$PATH folders (e.g.`/usr/local/bin`) to
    make them accessible without specifying the full path.
-   Typical command to run a container interactively (`-it`) and delete
    on exit(`--rm`): `docker run --rm -it name:tag`
-   \[1\] Use [hadolint](https://hadolint.github.io/hadolint/) and
    remove as many reported warnings as possible.
-   \[0.5\] Add relevant
    [labels](https://docs.docker.com/engine/reference/builder/#label),
    e.g. maintainer, version, etc.
    ([hint](https://medium.com/@chamilad/lets-make-your-docker-image-better-than-90-of-existing-ones-8b1e5de950d))


### Anaconda

First, let\'s install conda and create new environment:

    bash ~/Anaconda3-2022.10-Linux-x86_64.sh
    conda create --name project

Execute to activate it:

    conda activate project

Next, add channels from which all necessary packages will be downloaded:

    conda config --add channels defaults
    conda config --add channels conda-forge
    conda config --add channels bioconda

Installing the packages using bioconda channel:

    conda install -c bioconda fastqc=0.11.9
    conda install -c bioconda bedtools=2.30.0
    conda install -c bioconda star=2.7.10b
    conda install -c bioconda salmon=1.9.0
    conda install -c bioconda multiqc=1.13
    conda install -c bioconda samtools=1.16.1 

It seems problematic to install specific version of Picard, so the
available version will be downloaded:

    conda install -c bioconda picard

Export the environment to the file:

    conda env export --from-history > environment.yml

Ensure that it can be rebuilt from the file without problems:

    conda deactivate
    conda remove --name myenv --all 
    conda env create --file environment.yml


The contens of **Enviroment.yml** are:

    name: project
    channels:
      - conda-forge
      - bioconda
      - defaults
    dependencies:
      - fastqc=0.11.9
      - bedtools=2.30.0
      - star=2.7.10b
      - salmon=1.9.0
      - multiqc=1.13
      - samtools=1.16.1 
      - picard

    prefix: /home/allirik/anaconda3/envs/p2



### Docker

Execute following to install docker:

    sudo apt install docker.io
    sudo snap install docker

Create a container:

    touch Dockerfile
    nano Dockerfile

Create dockerfile:

    FROM ubuntu:20.04
    MAINTAINER "allirik"

    # update links and install apt-utils apt-transport-https unzip and python3-pip
    RUN apt-get update && apt -y install apt-transport-https openjdk-11-jre-headless unzip python3-pip

    # create /.bashrc for aliases
    RUN touch /.bashrc

    # fastqc v0.11.9
    RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip && \
        unzip fastqc_v0.11.9.zip && \
        rm fastqc_v0.11.9.zip && \
        chmod a+x FastQC/fastqc && \
        echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

    # bedtools v.2.30.0
    RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary && \
        chmod a+x /bin/bedtools.static.binary && \
        echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

    # STAR v.2.7.10b
    RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip && \
        unzip STAR_2.7.10b.zip && \
        rm STAR_2.7.10b.zip && \
        chmod a+x STAR_2.7.10b/Linux_x86_64_static/STAR && \
        mv STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR && \
        rm -r STAR_2.7.10b

    # salmon v.1.9.0
    RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz && \
        tar -zxvf salmon-1.9.0_linux_x86_64.tar.gz && \
        rm salmon-1.9.0_linux_x86_64.tar.gz && \
        chmod a+x salmon-1.9.0_linux_x86_64/bin/salmon && \
        mv salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon && \
        rm -r salmon-1.9.0_linux_x86_64 

    # multic v1.13
    RUN pip install multiqc==1.13

    # samtools v1.16.1
    RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip && \
        unzip samtools-1.16.1.zip && \
        rm samtools-1.16.1.zip && \
        mv samtools-1.16.1/misc samtools && \
        rm -r samtools-1.16.1 && \
        echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

    # picard v2.27.5
    RUN wget https://github.com/broadinstitute/picard/releases/download/2.27.5/picard.jar -O /bin/picard.jar && \
        chmod a+x /bin/picard.jar && \
        echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc


To build docker image, execute:

    sudo docker build --no-cache -t ubuntu:20.04

Run the container and make sure that it will be deleted after exit.

    sudo docker run --rm -it ubuntu:20.04


To change dockerfile, Linter was used:

1.  MAINTAINER \--\> LABEL maintainer
2.  Delete the apt-get lists after installing something
3.  apt \--\> apt-get install package=\"package_vesrion\"


#### Linter Labeled Optimazed Dockerfile

    FROM ubuntu:20.04
    LABEL maintainer="allirik"
    ARG DEBIAN_FRONTEND=noninteractive

    LABEL org.label-schema.schema-version="1.0"
    LABEL org.label-schema.name="Homework2"
    LABEL org.label-schema.description="Homework2 description"

    # apt-transport-https + python-pip + unzip
    RUN apt-get update && apt-get -y --no-install-recommends install apt-utils=2.4.8 apt-transport-https=2.0.2ubuntu0.2 && \
      apt-get -y --no-install-recommends install openjdk-11-jre-headless=11.0.17+8-1ubuntu2~20.04   && \
      apt-get -y --no-install-recommends install python3-pip=20.0.2-5ubuntu1.5 unzip=6.0-25ubuntu1.1&& \ 
      apt-get clean && \ 
      rm -rf /var/lib/apt/lists/*

    # Create /.bashrc for aliases
    RUN touch /.bashrc

    # fastqc v0.11.9
    RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip && \
        unzip fastqc_v0.11.9.zip && \
        rm fastqc_v0.11.9.zip && \
        chmod a+x FastQC/fastqc && \
        echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

    # bedtools v.2.30.0
    RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary && \
        chmod a+x /bin/bedtools.static.binary && \
        echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

    # STAR v.2.7.10b
    RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip && \
        unzip STAR_2.7.10b.zip && \
        rm STAR_2.7.10b.zip && \
        chmod a+x STAR_2.7.10b/Linux_x86_64_static/STAR && \
        mv STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR && \
        rm -r STAR_2.7.10b

    # salmon v.1.9.0
    RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz && \
        tar -zxvf salmon-1.9.0_linux_x86_64.tar.gz && \
        rm salmon-1.9.0_linux_x86_64.tar.gz && \
        chmod a+x salmon-1.9.0_linux_x86_64/bin/salmon && \
        mv salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon && \
        rm -r salmon-1.9.0_linux_x86_64 

    # multic v1.13
    RUN pip install multiqc==1.13

    # samtools v1.16.1
    RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip && \
        unzip samtools-1.16.1.zip && \
        rm samtools-1.16.1.zip && \
        mv samtools-1.16.1/misc samtools && \
        rm -r samtools-1.16.1 && \
        echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

    # picard v2.27.5
    RUN wget https://github.com/broadinstitute/picard/releases/download/2.27.5/picard.jar -O /bin/picard.jar && \
        chmod a+x /bin/picard.jar && \
        echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc


## Extra points \[1.5\] 

You will be awarded extra points for the following:

-   \[0.5\] Using [multi-stage
    builds](https://docs.docker.com/build/building/multi-stage/) in
    Docker. E.g. to build reat and copy only the executable to the final
    image.

-   \[0.75\] Minimizing the size of the final Docker image. That is,
    removing all intermediates, unnecessary binaries/caches, etc. Don\'t
    forget to compare & report the final size before and after all the
    optimizations.

-   \[0.25\] Create an extra Dockerfile that starts from [a conda base
    image](https://hub.docker.com/r/continuumio/anaconda3) and builds
    everything from your conda environment file.

Hint: `conda env create --quiet -f environment.yml && conda clean -a`
([example](https://github.com/nf-core/clipseq/blob/master/Dockerfile))


Docker image size:

-   via raw Dockerfile \-- **1.89Gb**
-   via optimized Dockerfile \-- **1.17Gb**


#### Dockerfile for conda

    FROM continuumio/miniconda3
    LABEL maintainer="allirik"
    #Labels.   
    LABEL org.label-schema.schema-version="1.0" 
    LABEL org.label-schema.name="Homework2"
    LABEL org.label-schema.description="Homework2 description"

    #create the env    
    COPY environment.yml . 
    RUN apt-get update && apt-get -y install apt-utils=2.4.8 apt-transport-https=2.0.2ubuntu0.2 && \
      conda env create -f environment.yml && conda clean -a 
