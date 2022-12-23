# Computational-Infrastructure-in-Bioinformatics-Tasks

# 2 - Working with remote servers

**git branch name:** jbrowser

## Theory \[2\] {#theory-2}

-   \[0.4\] What are [computer
    ports](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)
    on a high level? How many ports are there on a typical computer?

Computer ports are distinctive identifiers that are software-based and
assist an OS in dividing information streams. A port is like to a door,
and OS contains several of them. It utilizes many ports to deliver and
receive various types of information. A packet of information that is
delivered from one computer port contains information about the port via
which it should be received by the destination computer. Additionally,
the target computer is \"listening\" to a certain door to gather
information of this nature.

An integer in the range 0 to 65535 is designated as the port; however,
for TCP, zero port is reserved and cannot be used, while in UDP, zero
port denotes a portless connection. In a normal computer, not every port
is utilized. There are around 18 ports that receive heavy traffic and
are well-known:

**Port number -\> Protocol used**

    20 -> File Transfer Protocol (FTP) Data Transfer
    21 -> File Transfer Protocol (FTP) Command Control
    22 -> Secure Shell (SSH) Secure Login
    23 -> Telnet remote login service, unencrypted text messages
    25 -> Simple Mail Transfer Protocol (SMTP) email delivery
    53 -> Domain Name System (DNS) service
    67, 68 -> Dynamic Host Configuration Protocol (DHCP)
    80 -> Hypertext Transfer Protocol (HTTP) used in the World Wide Web
    110 -> Post Office Protocol (POP3)
    119 -> Network News Transfer Protocol (NNTP)
    123 -> Network Time Protocol (NTP)
    143 -> Internet Message Access Protocol (IMAP) Management of digital mail
    161 -> Simple Network Management Protocol (SNMP)
    194 -> Internet Relay Chat (IRC)
    443 -> HTTP Secure (HTTPS) HTTP over TLS/SSL
    546, 547 -> DHCPv6 IPv6 version of DHCP

-   \[0.4\] What is the difference between http, https, ssh, and other
    protocols? In what sense are they similar? Name default ports for
    several data transfer protocols.

The semantics of client-server interactions are governed by all
client-server protocols. They do application-level tasks. When data are
exchanged via a protocol, protocol-specific blocks of data are added to
the data so that a network member can recognize the data. The
protocol-specific blocks provide some meta-data regarding the protocol
version, date, time, status, and other details. What distinguishes one
protocol from another is the content of the block. Another distinction
is the use of encryption while transferring protocols. While HTTPS, SSH,
or SFTP have encryption, HTTP and FTP do not. They serve several
purposes. Generally speaking, HTTP/HTTPS protocols are used to transmit
HTML pages, whereas SSH, FTP, FTPS, and SFTP are used to provide remote
file system access.

-   \[0.4\] Explain briefly: (1) what is IP, (2) what IPs are called
    \'white\'/public, (3) and what happens when you enter \'google.com\'
    into the web browser.

A device\'s **IP** is a distinctive identification in a local or
international computer network. On the hardware-based network layer, IP
protocol is used to make it work. There are two variations: 192.0.2.235
is how IPv4 addresses seem; 2001:0db8:11a3:09d7:1f34:8a2e:07a0:765d is
how IPv6 addresses appear.

In contrast to **private IP** addresses, which are reserved for local
networks, **public IP** addresses are accessible to Internet users. A
handful of IP addresses are set aside for usage in local networks.

When a browser request is made, such as \"HSE,\" the URL is sent to the
closest DNS server that is accessible. There, the request is fulfilled
by the website\'s IP address for \"HSE,\" and my browser receives that
IP in response. My browser then sends a request to the Internet using
the IP it just acquired. My request is received by the HSE web server,
and it responds with an HTML page.

-   \[0.4\] What is Nginx? How does it work on the high level? List
    several alternative web servers.

One of the most popular web servers is **Nginx**. It handles http/https
requests, responds to them, or forwards them to other servers or ports
on the same server using the ports 80 for http and 443 for https. It is
a software server that has to be set up on a hardware server (or a
standard computer) in order to function properly.

One of the most well-known nginx alternatives is Apache, while others
include LiteSpeed Web Server, Microsoft-IIS, and others.

-   \[0.4\] What is SSH, and for what is it typically used? Explain two
    ways to authenticate in an SSH server in detail.

A network protocol called SSH enables remote operating system
administration. It employs encryption for information flow at the
application level.

1.  Authentication by password. In this instance, we make a connection
    request to an SSH server. Our computer is now creating and storing a
    unique fingerprint.
    `12:f8:7e:78:61:b4:bf:e2:de:24:15:96:4e:d4:72:53` is how it appears.
    Our unique client-server connection may be recognized thanks to the
    fingerprint. Because such a device lacks the fingerprint, it aids in
    preventing connections to the SSH server from computers with the
    same IP address. The IPs and passwords of clients are stored in a
    database on the SSH server. A password must be entered while
    connecting to the SSH server. The connection opens when the password
    has been entered and verified by the server.

2.  Authentication by key couple. Public and private keys are produced.
    The client computer where the private key is kept should not have
    access to it. Everybody may see the public key, which is useless
    without the private one. In actuality, a public key resembles a
    lock. Everyone can see it, but only someone with a secret key can
    open it. An SSH server needs to receive the client\'s public key in
    order to establish a connection. The creation of the fingerprint
    follows the instructions in \"Authentication by password.\" If the
    server discovers a match with the public key, it transmits a message
    that has been encrypted using the public key. With no private key,
    it cannot decipher the data. It merely provides the encrypted data
    to the client, who uses its private key to decrypt it.


## Problem \[6.5\]

A real-life situation that occurred to me several times over the years.

Imagine wrapping up a large bioinformatics project and wanting to share
raw data with your colleagues in a friendly and straightforward format.
The best option would be to use an online genome browser and host your
data remotely, so it is easily accessible by anyone with a valid link.
This is exactly what we will be doing here.

*Please consider doing this HW using Linux since setting up the SSH
client on Windows is painful, and I won\'t be able to help you.*

Steps:

-   \[2\] Create a new virtual machine in the Yandex/Mail/etc cloud
    (order at least 10GB of free disk space). Generate SSH key pair and
    use it to connect to your server.
-   \[1\] Download the latest human genome assembly (GRCh38) from the
    Ensemble FTP server
    ([fasta](https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz),
    [GFF3](https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz)).
    Index the fasta using samtools (`samtools faidx`) and GFF3 using
    tabix.
-   \[1\] Select and download BED files for three ChIP-seq and one
    ATAC-seq experiment from the ENCODE (use one tissue/cell line).
    Sort, bgzip, and index them using tabix.
-   \[1\] Download and install [JBrowse 2](https://jbrowse.org/jb2/).
    Create a new jbrowse
    [repository](https://jbrowse.org/jb2/docs/cli/#jbrowse-create-localpath)
    in `/mnt/JBrowse/` (or some other folder).
-   \[0.25\] Install nginx and amend its config(/etc/nginx/nginx.conf)
    to contain the following section:

```{=html}
<!-- -->
```
    http {
      # Don't touch other options!
      # ........
      # ........

      # Add this:
      server {
    		listen 80;
    		index index.html;

    		location /jbrowse/ {
    			alias /mnt/JBrowse/;	
    		}
    	}

      # ........
    }

-   \[0.25\] Restart the nginx (reload its config) and make sure that
    you can access the browser using a link like this:
    `http://64.129.58.13/jbrowse/`. Here `64.129.58.13` is your public
    IP address.
-   \[1\] Add your files to the genome browser and verify that
    everything works as intended. Don\'t forget to
    [index](https://jbrowse.org/jb2/docs/cli/#jbrowse-text-index) the
    genome annotation, so you could later search by gene names.

**Remember to put a persistent link to a session with all your BED files
and the genome annotation in the report. I must be able to access it
without problems.**


Generating key for Yandex Virtual Machine:

    ssh-keygen -t ed25519

Generated key:

    ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHgxUYyYIsq8GH1VrYcgn1qqnEydVZSx72LjtugVTDuc llirik@HP-HELLOWORLD

Connect:

    ssh -i homework all-lirik@158.160.45.214


    sudo apt-get update
    sudo apt-get upgrade

Install some important stuff:

    sudo apt-get install bedtools
    sudo apt-get install samtools
    sudo apt-get install tabix

### GRCh38

Download latest human genome assembly:

    wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
    wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz

Unzip:

    gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
    gunzip Homo_sapiens.GRCh38.108.gff3.gz

Index fasta file:

    samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa

Sort GFF3:

    (grep "^#" Homo_sapiens.GRCh38.108.gff3; grep -v "^#" Homo_sapiens.GRCh38.108.gff3 | sort -t"printf '\t'" -k1,1 -k4,4n) | bgzip > sorted.Homo_sapiens.GRCh38.108.gff3.gz;

Index GFF3:

    tabix -p gff sorted.Homo_sapiens.GRCh38.108.gff3.gz;


### Processing BED and ATAC-seq files

→ Cell line: GM12878

→ ATAC-seq \| <https://www.encodeproject.org/experiments/ENCSR637XSC/>
\| ENCFF654BVB (IDR)

→ ChIP-seq 1 \| SRF       \|
<https://www.encodeproject.org/experiments/ENCSR041XML/> \| ENCFF489QRS
(IDR)

This gene encodes a ubiquitous nuclear protein that stimulates both cell
proliferation and differentiation. It is a member of the MADS (MCM1,
Agamous, Deficiens, and SRF) box superfamily of transcription factors.
This protein binds to the serum response element (SRE) in the promoter
region of target genes. This protein regulates the activity of many
immediate-early genes, for example c-fos, and thereby participates in
cell cycle regulation, apoptosis, cell growth, and cell differentiation.
This gene is the downstream target of many pathways; for example, the
mitogen-activated protein kinase pathway (MAPK) that acts through the
ternary complex factors (TCFs). Two transcript variants encoding
different isoforms have been found for this gene.

→ ChIP-seq 2 \| HSF1    \|
<https://www.encodeproject.org/experiments/ENCSR009MBP/> \| ENCFF405WKK
(IDR)

Heat shock factor 1 (HSF1) is a protein that in humans is encoded by the
HSF1 gene. HSF1 is highly conserved in eukaryotes and is the primary
mediator of transcriptional responses to proteotoxic stress with
important roles in non-stress regulation such as development and
metabolism.

→ ChIP-seq 3 \| CEBPB \|
<https://www.encodeproject.org/experiments/ENCSR681NOM/> \| ENCFF370CBN
(IDR)

This intronless gene encodes a transcription factor that contains a
basic leucine zipper (bZIP) domain. The encoded protein functions as a
homodimer but can also form heterodimers with CCAAT/enhancer-binding
proteins alpha, delta, and gamma. Activity of this protein is important
in the regulation of genes involved in immune and inflammatory
responses, among other processes. The use of alternative in-frame AUG
start codons results in multiple protein isoforms, each with distinct
biological functions.

Download data:

    wget -nchttps://www.encodeproject.org/files/ENCFF370CBN/@@download/ENCFF370CBN.bed.gz  -O chip1.bed.gz

    wget -nc https://www.encodeproject.org/files/ENCFF405WKK/@@download/ENCFF405WKK.bed.gz -O chip2.bed.gz

    wget -nc https://www.encodeproject.org/files/ENCFF489QRS/@@download/ENCFF489QRS.bed.gz -O chip3.bed.gz

    wget -nc https://www.encodeproject.org/files/ENCFF654BVB/@@download/ENCFF654BVB.bed.gz -O atac.bed.gz


Sort, bgzip, and index them using tabix:

    for a in atac chip1 chip2 chip3
    do
        gunzip ${a}.bed.gz
        bedtools sort -i $a.bed > sort_${a}.bed
        bgzip sort_${a}.bed
        tabix sort_${a}.bed.gz
    done

### JBrowse 2

Installation using npm:

    sudo apt install npm
    sudo npm install -g @jbrowse/cli

    # And for path from your source
    jbrowse create JBrowse

### Nginx

    sudo apt-get install -y nginx
    cd /etc/nginx
    sudo nano nginx.conf



Modify config by adding server:

    #include /etc/nginx/sites-enabled/*;

    server {
      listen 80 default_server;
      index index.html;
      server_name _;

      # Don't put JBrowse inside the home directory!
      # You will have problems with permissions
      location /jbrowse/ {
        alias /home/all-lirik/JBrowse/;
      }
    }



After reloading nginx, we can now open jbrowse in browser:

    sudo nginx -s reload
    http://158.160.45.214/jbrowse/



### Adding files to JBrowse



    sudo jbrowse add-assembly Homo_sapiens.GRCh38.dna.primary_assembly.fa --load copy --out /home/all-lirik/JBrowse

    for i in chip1 chip2 chip3 atac
    do
      gunzip sort_${i}.bed.gz
      awk '{gsub(/^chr/,""); print}' sort_${i}.bed > $(echo sort_${i}.bed| cut -d '.' -f 1)'_renamed.bed'
      bgzip sort_${i}_renamed.bed
      tabix -f sort_${i}_renamed.bed.gz
    done

    sudo jbrowse add-track sorted.Homo_sapiens.GRCh38.108.gff3.gz  --load copy --out /home/all-lirik/JBrowse/

    for i in chip1 chip2 chip3 atac
    do 
      sudo jbrowse add-track sort_${i}_renamed.bed.gz --load copy --out /home/all-lirik/JBrowse/
    done

## Extra points \[1.5\]

-   \[1\] Create a Docker container for running JBrowse 2. It should be
    a self-contained application, listening on the default HTTP port.
    Users must be able to mount directories with custom configs and
    access them later without any problems.

Hint: to specify the config, use the config=PATH query parameter. E.g.
`http://64.129.58.13/jbrowse/?config=my_folder%2Fconfig.json` where
`my_folder%2Fconfig.json` is the
[escaped](https://en.wikipedia.org/wiki/Percent-encoding) path to the
config file.

-   \[0.5\] Give an in-depth explanation of the OSI model and how the
    TCP/IP stack works. Don\'t copy-paste descriptions from the
    internet; paraphrase and shorten as much as possible (imagine
    writing a cheat sheet for yourself).

A logical and conceptual paradigm called Open Systems Interconnections
(OSI) describes the networking that open systems use to interact and
communicate with one another.

Layers should only be created when particular degrees of abstraction are
necessary, their roles should be chosen in line with established
international standards, and there should be enough layers present for
the various functions to not overlap. Each layer in the OSI model should
be able to provide services to the layer above it since each layer in
the OSI model depends on the layer below it to carry out essential
functions. At the same time, each layer should be modest enough to avoid
the architecture from becoming unduly complicated. It shouldn\'t be
necessary to update one layer before changing another.

-   Physical layer. The main responsibility of this layer is to convert
    the bitstream into radio signals, electrical/optical pulses, or
    both, which may subsequently be converted into a physical medium. In
    addition to the physical connection to the underlying media, this
    layer also offers the hardware necessary to activate, maintain, and
    deactivate the physical connections between the data link entities.
    This includes bit stream sequencing, channel identification on the
    underlying media, and multiplexing. This should not be confused with
    the medium itself.

-   Data Link Layer. The data connection layer controls and directs the
    physical layer\'s operations. The data link layer delivers data to
    the physical layer at one end, and it receives data from the
    physical layer at the other. The definition of the flow control
    mechanism between the two nodes also ensures that any flaws that may
    have occurred during transmission or reception on the physical
    carrier are identified and corrected. This prevents buffer overflow
    on either side of the data link connection.

-   Network Layer. This layer is in charge of determining the necessary
    intermediary nodes that may be required when transmitting data to a
    destination node that is not on the same network as the source node.
    This layer also breaks down the datagram into smaller bits when the
    underlying data link layer is unable to handle a datagram that the
    network layer demands be delivered over the network.

-   Transport layer. The transport layer allows functional and
    procedural movement of variable length data sequences from source to
    destination over one or more networks. This layer provides
    end-to-end services with or without connection establishment for the
    session layer. This layer is responsible for the creation,
    management, and release of connections.

-   Session layer. The main responsibility of this layer is to manage
    data flow between the presentation layers at the two ends as well as
    to synchronize and coordinate their communication. This layer
    creates, maintains, and terminates connections between applications.
    The session layer initiates, controls, and terminates exchanges and
    dialogs between programs at each endpoint.

-   Presentation level. A consistent representation of data communicated
    between application objects is provided by the presentation layer,
    which provides independence from changes in data format and grammar.
    This layer is also known as the syntactic layer at times. The
    presentation layer must first alter the data before the application
    layer may use it. This layer also handles the encryption and
    decryption of application data.

-   Application Layer. The application layer is the top layer in the OSI
    model since there are no upper layer protocols present there. The
    OSI application layer is directly used by applications of software
    that need to communicate with other systems. This layer should not
    be confused with application software, which is a type of program
    that employs software to carry out its operations; as an example,
    HTTP is an application layer protocol and Google Chrome is software
    application.

TCP/IP is a transmission control protocol and Internet protocol. This
model helps determine the best way to connect a certain machine to the
Internet and how data may be exchanged between them.

Features of the OSI model include:

Support flexible architecture; Make it simple to add more systems to the
network; Ensure that data arriving out of order must be sequenced; Be a
connection-oriented protocol; Provide reliability; Ensure that data
arriving out of order must be sequenced; Enable flow control so that the
sender never overwhelms the receiver with data.

Four levels.

-   Application level. This layer is used by the vast majority of
    network applications.
-   Transport level. The appropriate sequence of data arrival may be
    ensured by transport layer protocols, and they can also deal with
    the problem of unreliable message delivery (\"did the message reach
    the recipient?\"). The TCP/IP stack\'s transport protocols specify
    the application that the data is intended for.
-   Network level. One of the network layer\'s original goals was to
    transport data between networks. In order to forward packets to the
    correct network, this layer uses routers to determine the network
    address using the network mask. An identification number for the
    next-layer protocol that should be used to access an IP network
    protocol packet\'s payload may be included in the packet itself.
    This is the unique IP number for the protocol.
-   Channel level. The data link layer describes a procedure for
    encoding data for transmission as a data packet at the physical
    layer. For instance, the header elements of an Ethernet packet
    provide information on the machine or machines that the packet is
    intended for.
