# Sat 11 Jul 2020 03:08:38 PM PDT
running masurca on the data. Really not sure about some of the parameters
 
```bash
# example configuration file 

# DATA is specified as type {PE,JUMP,OTHER,PACBIO} and 5 fields:
# 1)two_letter_prefix 2)mean 3)stdev 4)fastq(.gz)_fwd_reads
# 5)fastq(.gz)_rev_reads. The PE reads are always assumed to be
# innies, i.e. --->.<---, and JUMP are assumed to be outties
# <---.--->. If there are any jump libraries that are innies, such as
# longjump, specify them as JUMP and specify NEGATIVE mean. Reverse reads
# are optional for PE libraries and mandatory for JUMP libraries. Any
# OTHER sequence data (454, Sanger, Ion torrent, etc) must be first
# converted into Celera Assembler compatible .frg files (see
# http://wgs-assembler.sourceforge.com)

DATA
#Illumina paired end reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#if single-end, do not specify <reverse_reads>
#MUST HAVE Illumina paired end reads to use MaSuRCA
PE= pe 350 53  /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz
#Illumina mate pair reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#JUMP= sh 3600 200  /FULL_PATH/short_1.fastq  /FULL_PATH/short_2.fastq
#pacbio OR nanopore reads must be in a single fasta or fastq file with absolute path, can be gzipped
#if you have both types of reads supply them both as NANOPORE type
#PACBIO=/FULL_PATH/pacbio.fa
NANOPORE=/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.fastq
#Other reads (Sanger, 454, etc) one frg file, concatenate your frg files into one if you have many
#OTHER=/FULL_PATH/file.frg
#synteny-assisted assembly, concatenate all reference genomes into one reference.fa; works for Illumina-only data
#REFERENCE=/FULL_PATH/nanopore.fa
END

PARAMETERS
#PLEASE READ all comments to essential parameters below, and set the parameters according to your project
#set this to 1 if your Illumina jumping library reads are shorter than 100bp
#EXTEND_JUMP_READS=0
#this is k-mer size for deBruijn graph values between 25 and 127 are supported, auto will compute the optimal size based on the read data and GC content
GRAPH_KMER_SIZE = auto
#set this to 1 for all Illumina-only assemblies
#set this to 0 if you have more than 15x coverage by long reads (Pacbio or Nanopore) or any other long reads/mate pairs (Illumina MP, Sanger, 454, etc)
USE_LINKING_MATES = 0
#specifies whether to run the assembly on the grid
#USE_GRID=0
#specifies grid engine to use SGE or SLURM
#GRID_ENGINE=SGE
#specifies queue (for SGE) or partition (for SLURM) to use when running on the grid MANDATORY
#GRID_QUEUE=all.q
#batch size in the amount of long read sequence for each batch on the grid
#GRID_BATCH_SIZE=500000000
#use at most this much coverage by the longest Pacbio or Nanopore reads, discard the rest of the reads
#can increase this to 30 or 35 if your reads are short (N50<7000bp)
LHE_COVERAGE=25
#set to 0 (default) to do two passes of mega-reads for slower, but higher quality assembly, otherwise set to 1
MEGA_READS_ONE_PASS=0
#this parameter is useful if you have too many Illumina jumping library mates. Typically set it to 60 for bacteria and 300 for the other organisms 
#LIMIT_JUMP_COVERAGE = 300
#these are the additional parameters to Celera Assembler.  do not worry about performance, number or processors or batch sizes -- these are computed automatically. 
#CABOG ASSEMBLY ONLY: set cgwErrorRate=0.25 for bacteria and 0.1<=cgwErrorRate<=0.15 for other organisms.
CA_PARAMETERS =  cgwErrorRate=0.15
#CABOG ASSEMBLY ONLY: whether to attempt to close gaps in scaffolds with Illumina  or long read data
CLOSE_GAPS=1
#number of cpus to use, set this to the number of CPUs/threads per node you will be using
NUM_THREADS = 70
#this is mandatory jellyfish hash size -- a safe value is estimated_genome_size*20
JF_SIZE = 18000000000
#ILLUMINA ONLY. Set this to 1 to use SOAPdenovo contigging/scaffolding module.  
#Assembly will be worse but will run faster. Useful for very large (>=8Gbp) genomes from Illumina-only data
SOAP_ASSEMBLY=0
#If you are doing Hybrid Illumina paired end + Nanopore/PacBio assembly ONLY (no Illumina mate pairs or OTHER frg files).  
#Set this to 1 to use Flye assembler for final assembly of corrected mega-reads.  
#A lot faster than CABOG, AND QUALITY IS THE SAME OR BETTER. 
#Works well even when MEGA_READS_ONE_PASS is set to 1.  
#DO NOT use if you have less than 15x coverage by long reads.
FLYE_ASSEMBLY=0
END
```


running masurca script
```bash
sudo ./masurca /home/jon/Working_Files/MaSuRCA-3.4.1/config.txt

Verifying PATHS...
jellyfish OK
runCA OK
createSuperReadsForDirectory.perl OK
creating script file for the actions...done.
execute assemble.sh to run assembly
```

running the assemble.sh script
```bash
sudo ./assemble.sh
[Sat 11 Jul 2020 03:11:54 PM PDT] Processing pe library reads
[Sat 11 Jul 2020 03:50:27 PM PDT] Average PE read length 150
[Sat 11 Jul 2020 03:50:28 PM PDT] Using kmer size of 99 for the graph
[Sat 11 Jul 2020 03:50:28 PM PDT] MIN_Q_CHAR: 33
[Sat 11 Jul 2020 03:50:28 PM PDT] Creating mer database for Quorum
[Sat 11 Jul 2020 04:21:37 PM PDT] Error correct PE
[Sat 11 Jul 2020 05:00:18 PM PDT] Estimating genome size
[Sat 11 Jul 2020 05:21:19 PM PDT] Estimated genome size: 749318400
[Sat 11 Jul 2020 05:21:19 PM PDT] Creating k-unitigs with k=99
[Sat 11 Jul 2020 05:58:27 PM PDT] Computing super reads from PE 
[Sat 11 Jul 2020 07:35:27 PM PDT] Using CABOG from /home/jon/Working_Files/MaSuRCA-3.4.1/bin/../CA8/Linux-amd64/bin
[Sat 11 Jul 2020 07:35:27 PM PDT] Running mega-reads correction/assembly
[Sat 11 Jul 2020 07:35:27 PM PDT] Using mer size 15 for mapping, B=15, d=0.02
[Sat 11 Jul 2020 07:35:27 PM PDT] Estimated Genome Size 749318400
[Sat 11 Jul 2020 07:35:27 PM PDT] Estimated Ploidy 1
[Sat 11 Jul 2020 07:35:27 PM PDT] Using 70 threads
[Sat 11 Jul 2020 07:35:27 PM PDT] Output prefix mr.41.15.15.0.02
[Sat 11 Jul 2020 07:35:27 PM PDT] Using 25x of the longest ONT reads
[Sat 11 Jul 2020 07:35:28 PM PDT] Reducing super-read k-mer size
[Sat 11 Jul 2020 08:06:08 PM PDT] Mega-reads pass 1
[Sat 11 Jul 2020 08:06:08 PM PDT] Running locally in 1 batch
[Sat 11 Jul 2020 08:12:50 PM PDT] Mega-reads pass 2
[Sat 11 Jul 2020 08:12:50 PM PDT] Running locally in 1 batch
[Sat 11 Jul 2020 08:14:24 PM PDT] Refining alignments
[Sat 11 Jul 2020 08:14:39 PM PDT] Computing allowed merges
[Sat 11 Jul 2020 08:14:40 PM PDT] Joining
[Sat 11 Jul 2020 08:14:41 PM PDT] Gap consensus
[Sat 11 Jul 2020 08:14:42 PM PDT] Generating assembly input files
[Sat 11 Jul 2020 08:15:00 PM PDT] Coverage of the mega-reads less than 5 -- using the super reads as well
[Sat 11 Jul 2020 08:15:29 PM PDT] Coverage threshold for splitting unitigs is 15 minimum ovl 250
[Sat 11 Jul 2020 08:15:29 PM PDT] Running assembly
[Sat 11 Jul 2020 09:07:29 PM PDT] Recomputing A-stat
[Sat 11 Jul 2020 09:34:53 PM PDT] Mega-reads initial assembly complete
[Sat 11 Jul 2020 09:34:53 PM PDT] No gap closing possible
[Sat 11 Jul 2020 09:34:53 PM PDT] Removing redundant scaffolds
[Sat 11 Jul 2020 09:35:32 PM PDT] Assembly complete, final scaffold sequences are in CA.mr.41.15.15.0.02/final.genome.scf.fasta
[Sat 11 Jul 2020 09:35:32 PM PDT] All done
[Sat 11 Jul 2020 09:35:32 PM PDT] Final stats for CA.mr.41.15.15.0.02/final.genome.scf.fasta
N50 3034
Sequence 54074486
Average 2546
E-size 3780.96
Count 21239

```


it finished! lol, but wow, bad results. 
```bash
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3152	0.1847	0.1849	0.3152	0.0000	0.0000	0.0000	0.3696	0.0307

Main genome scaffold total:         	21239
Main genome contig total:           	21239
Main genome scaffold sequence total:	54.074 MB
Main genome contig sequence total:  	54.074 MB  	0.000% gap
Main genome scaffold N/L50:         	5711/3.034 KB
Main genome contig N/L50:           	5711/3.034 KB
Main genome scaffold N/L90:         	16093/1.403 KB
Main genome contig N/L90:           	16093/1.403 KB
Max scaffold length:                	41.434 KB
Max contig length:                  	41.434 KB
Number of scaffolds > 50 KB:        	0
% main genome in scaffolds > 50 KB: 	0.00%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	        21,239	        21,239	    54,074,486	    54,074,486	 100.00%
    250 	        21,239	        21,239	    54,074,486	    54,074,486	 100.00%
    500 	        21,045	        21,045	    53,991,788	    53,991,788	 100.00%
   1 KB 	        19,442	        19,442	    52,731,306	    52,731,306	 100.00%
 2.5 KB 	         8,040	         8,040	    33,455,941	    33,455,941	 100.00%
   5 KB 	         1,665	         1,665	    11,589,635	    11,589,635	 100.00%
  10 KB 	           119	           119	     1,583,445	     1,583,445	 100.00%
  25 KB 	             4	             4	       134,433	       134,433	 100.00%

```


# Sun 12 Jul 2020 02:11:31 PM PDT

oook, rerunning. This is the config file
```bash
# example configuration file 

# DATA is specified as type {PE,JUMP,OTHER,PACBIO} and 5 fields:
# 1)two_letter_prefix 2)mean 3)stdev 4)fastq(.gz)_fwd_reads
# 5)fastq(.gz)_rev_reads. The PE reads are always assumed to be
# innies, i.e. --->.<---, and JUMP are assumed to be outties
# <---.--->. If there are any jump libraries that are innies, such as
# longjump, specify them as JUMP and specify NEGATIVE mean. Reverse reads
# are optional for PE libraries and mandatory for JUMP libraries. Any
# OTHER sequence data (454, Sanger, Ion torrent, etc) must be first
# converted into Celera Assembler compatible .frg files (see
# http://wgs-assembler.sourceforge.com)

DATA
#Illumina paired end reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#if single-end, do not specify <reverse_reads>
#MUST HAVE Illumina paired end reads to use MaSuRCA
PE= pe 350 53  /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz
#Illumina mate pair reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#JUMP= sh 3600 200  /FULL_PATH/short_1.fastq  /FULL_PATH/short_2.fastq
#pacbio OR nanopore reads must be in a single fasta or fastq file with absolute path, can be gzipped
#if you have both types of reads supply them both as NANOPORE type
#PACBIO=/FULL_PATH/pacbio.fa
NANOPORE=/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.fastq
#Other reads (Sanger, 454, etc) one frg file, concatenate your frg files into one if you have many
#OTHER=/FULL_PATH/file.frg
#synteny-assisted assembly, concatenate all reference genomes into one reference.fa; works for Illumina-only data
#REFERENCE=/FULL_PATH/nanopore.fa
END

PARAMETERS
#PLEASE READ all comments to essential parameters below, and set the parameters according to your project
#set this to 1 if your Illumina jumping library reads are shorter than 100bp
#EXTEND_JUMP_READS=0
#this is k-mer size for deBruijn graph values between 25 and 127 are supported, auto will compute the optimal size based on the read data and GC content
GRAPH_KMER_SIZE = auto
#set this to 1 for all Illumina-only assemblies
#set this to 0 if you have more than 15x coverage by long reads (Pacbio or Nanopore) or any other long reads/mate pairs (Illumina MP, Sanger, 454, etc)
USE_LINKING_MATES = 1
#specifies whether to run the assembly on the grid
USE_GRID=0
#specifies grid engine to use SGE or SLURM
#GRID_ENGINE=SGE
#specifies queue (for SGE) or partition (for SLURM) to use when running on the grid MANDATORY
#GRID_QUEUE=all.q
#batch size in the amount of long read sequence for each batch on the grid
#GRID_BATCH_SIZE=500000000
#use at most this much coverage by the longest Pacbio or Nanopore reads, discard the rest of the reads
#can increase this to 30 or 35 if your reads are short (N50<7000bp)
LHE_COVERAGE=25
#set to 0 (default) to do two passes of mega-reads for slower, but higher quality assembly, otherwise set to 1
MEGA_READS_ONE_PASS=0
#this parameter is useful if you have too many Illumina jumping library mates. Typically set it to 60 for bacteria and 300 for the other organisms 
#LIMIT_JUMP_COVERAGE = 300
#these are the additional parameters to Celera Assembler.  do not worry about performance, number or processors or batch sizes -- these are computed automatically. 
#CABOG ASSEMBLY ONLY: set cgwErrorRate=0.25 for bacteria and 0.1<=cgwErrorRate<=0.15 for other organisms.
CA_PARAMETERS =  cgwErrorRate=0.15
#CABOG ASSEMBLY ONLY: whether to attempt to close gaps in scaffolds with Illumina  or long read data
CLOSE_GAPS=1
#number of cpus to use, set this to the number of CPUs/threads per node you will be using
NUM_THREADS = 70
#this is mandatory jellyfish hash size -- a safe value is estimated_genome_size*20
JF_SIZE = 9000000000
#ILLUMINA ONLY. Set this to 1 to use SOAPdenovo contigging/scaffolding module.  
#Assembly will be worse but will run faster. Useful for very large (>=8Gbp) genomes from Illumina-only data
SOAP_ASSEMBLY=0
#If you are doing Hybrid Illumina paired end + Nanopore/PacBio assembly ONLY (no Illumina mate pairs or OTHER frg files).  
#Set this to 1 to use Flye assembler for final assembly of corrected mega-reads.  
#A lot faster than CABOG, AND QUALITY IS THE SAME OR BETTER. 
#Works well even when MEGA_READS_ONE_PASS is set to 1.  
#DO NOT use if you have less than 15x coverage by long reads.
FLYE_ASSEMBLY=0
END
```

creating the assemble.sh script
```bash
sudo ./masurca /home/jon/Working_Files/MaSuRCA-3.4.1/config.txt

Verifying PATHS...
jellyfish OK

runCA OK
createSuperReadsForDirectory.perl OK
creating script file for the actions...done.
execute assemble.sh to run assembly
```


running the assemble.sh script
```bash
sudo ./assemble.sh

N50 3027
Sequence 54056268
Average 2543.47
E-size 3766.15
Count 21253
```

ooook, basically the same results. deleting it

# Mon 13 Jul 2020 10:31:24 AM PDT

ok, removed the LHE coverage and added the reference genome. 
```bash
# example configuration file 

# DATA is specified as type {PE,JUMP,OTHER,PACBIO} and 5 fields:
# 1)two_letter_prefix 2)mean 3)stdev 4)fastq(.gz)_fwd_reads
# 5)fastq(.gz)_rev_reads. The PE reads are always assumed to be
# innies, i.e. --->.<---, and JUMP are assumed to be outties
# <---.--->. If there are any jump libraries that are innies, such as
# longjump, specify them as JUMP and specify NEGATIVE mean. Reverse reads
# are optional for PE libraries and mandatory for JUMP libraries. Any
# OTHER sequence data (454, Sanger, Ion torrent, etc) must be first
# converted into Celera Assembler compatible .frg files (see
# http://wgs-assembler.sourceforge.com)

DATA
#Illumina paired end reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#if single-end, do not specify <reverse_reads>
#MUST HAVE Illumina paired end reads to use MaSuRCA
PE= pe 350 53  /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz
#Illumina mate pair reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#JUMP= sh 3600 200  /FULL_PATH/short_1.fastq  /FULL_PATH/short_2.fastq
#pacbio OR nanopore reads must be in a single fasta or fastq file with absolute path, can be gzipped
#if you have both types of reads supply them both as NANOPORE type
#PACBIO=/FULL_PATH/pacbio.fa
NANOPORE=/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.fastq
#Other reads (Sanger, 454, etc) one frg file, concatenate your frg files into one if you have many
#OTHER=/FULL_PATH/file.frg
#synteny-assisted assembly, concatenate all reference genomes into one reference.fa; works for Illumina-only data
REFERENCE=/home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta

END

PARAMETERS
#PLEASE READ all comments to essential parameters below, and set the parameters according to your project
#set this to 1 if your Illumina jumping library reads are shorter than 100bp
#EXTEND_JUMP_READS=0
#this is k-mer size for deBruijn graph values between 25 and 127 are supported, auto will compute the optimal size based on the read data and GC content
GRAPH_KMER_SIZE = auto
#set this to 1 for all Illumina-only assemblies
#set this to 0 if you have more than 15x coverage by long reads (Pacbio or Nanopore) or any other long reads/mate pairs (Illumina MP, Sanger, 454, etc)
USE_LINKING_MATES = 1
#specifies whether to run the assembly on the grid
#USE_GRID=0
#specifies grid engine to use SGE or SLURM
#GRID_ENGINE=SGE
#specifies queue (for SGE) or partition (for SLURM) to use when running on the grid MANDATORY
#GRID_QUEUE=all.q
#batch size in the amount of long read sequence for each batch on the grid
#GRID_BATCH_SIZE=500000000
#use at most this much coverage by the longest Pacbio or Nanopore reads, discard the rest of the reads
#can increase this to 30 or 35 if your reads are short (N50<7000bp)
#LHE_COVERAGE=25
#set to 0 (default) to do two passes of mega-reads for slower, but higher quality assembly, otherwise set to 1
MEGA_READS_ONE_PASS=0
#this parameter is useful if you have too many Illumina jumping library mates. Typically set it to 60 for bacteria and 300 for the other organisms 
#LIMIT_JUMP_COVERAGE = 300
#these are the additional parameters to Celera Assembler.  do not worry about performance, number or processors or batch sizes -- these are computed automatically. 
#CABOG ASSEMBLY ONLY: set cgwErrorRate=0.25 for bacteria and 0.1<=cgwErrorRate<=0.15 for other organisms.
CA_PARAMETERS =  cgwErrorRate=0.15
#CABOG ASSEMBLY ONLY: whether to attempt to close gaps in scaffolds with Illumina  or long read data
CLOSE_GAPS=1
#number of cpus to use, set this to the number of CPUs/threads per node you will be using
NUM_THREADS = 70
#this is mandatory jellyfish hash size -- a safe value is estimated_genome_size*20
JF_SIZE = 9000000000
#ILLUMINA ONLY. Set this to 1 to use SOAPdenovo contigging/scaffolding module.  
#Assembly will be worse but will run faster. Useful for very large (>=8Gbp) genomes from Illumina-only data
SOAP_ASSEMBLY=0
#If you are doing Hybrid Illumina paired end + Nanopore/PacBio assembly ONLY (no Illumina mate pairs or OTHER frg files).  
#Set this to 1 to use Flye assembler for final assembly of corrected mega-reads.  
#A lot faster than CABOG, AND QUALITY IS THE SAME OR BETTER. 
#Works well even when MEGA_READS_ONE_PASS is set to 1.  
#DO NOT use if you have less than 15x coverage by long reads.
FLYE_ASSEMBLY=0
END
```

running the masurca script
```bash
sudo ./masurca /home/jon/Working_Files/MaSuRCA-3.4.1/config.txt

Verifying PATHS...
jellyfish OK
runCA OK
createSuperReadsForDirectory.perl OK
creating script file for the actions...done.
execute assemble.sh to run assembly

```


and running the assemble.sh script
```bash
./assemble.sh
```

# Tue 14 Jul 2020 01:02:18 PM PDT

weird results
```bash
[Mon 13 Jul 2020 10:45:45 AM PDT] Processing pe library reads
[Mon 13 Jul 2020 11:24:17 AM PDT] Average PE read length 150
[Mon 13 Jul 2020 11:24:18 AM PDT] Using kmer size of 99 for the graph
[Mon 13 Jul 2020 11:24:19 AM PDT] MIN_Q_CHAR: 33
[Mon 13 Jul 2020 11:24:19 AM PDT] Creating mer database for Quorum
[Mon 13 Jul 2020 11:52:59 AM PDT] Error correct PE
[Mon 13 Jul 2020 12:41:32 PM PDT] Estimating genome size
[Mon 13 Jul 2020 12:57:09 PM PDT] Estimated genome size: 749318400
[Mon 13 Jul 2020 12:57:09 PM PDT] Creating k-unitigs with k=99
[Mon 13 Jul 2020 01:35:25 PM PDT] Computing super reads from PE 
[Mon 13 Jul 2020 03:19:08 PM PDT] Using linking mates
[Mon 13 Jul 2020 03:19:08 PM PDT] Running synteny-guided assembly
[Mon 13 Jul 2020 03:19:08 PM PDT] Using mer size 17 for mapping, B=20, d=0.05
[Mon 13 Jul 2020 03:19:08 PM PDT] Using CA installation from /home/jon/Working_Files/MaSuRCA-3.4.1/bin/../CA8/Linux-amd64/bin
[Mon 13 Jul 2020 03:19:08 PM PDT] Using 70 threads
[Mon 13 Jul 2020 03:19:08 PM PDT] Output prefix mr.41.17.20.0.05
[Mon 13 Jul 2020 03:19:08 PM PDT] Reducing super-read k-mer size
[Mon 13 Jul 2020 03:48:30 PM PDT] Preparing reference
[Mon 13 Jul 2020 03:48:42 PM PDT] Running preliminary assembly
[Mon 13 Jul 2020 03:48:42 PM PDT] Mega-reads pass 1
compute_psa 4050524 2154769450
[Mon 13 Jul 2020 04:45:05 PM PDT] Recomputing A-stat
[Mon 13 Jul 2020 04:46:43 PM PDT] Joining
[Mon 13 Jul 2020 05:08:20 PM PDT] Polishing reference contigs
[Mon 13 Jul 2020 06:02:32 PM PDT] Final assembly
[Tue 14 Jul 2020 04:03:50 AM PDT] Final output sequences are in flye.mr.41.17.20.0.05/scaffolds.fasta
Error with file 'flye.mr.41.17.20.0.05/scaffolds.fasta'
```

there appears to be an assembly file though that is 700mb.

this was at the end of the flye log file
```bash

	Total length:	700290210
	Fragments:	170905
	Fragments N50:	8836
	Largest frg:	154539
	Scaffolds:	116
	Mean coverage:	2

[2020-07-14 04:03:50] root: INFO: Final assembly: /home/jon/Working_Files/MaSuRCA-3.4.1/bin/flye.mr.41.17.20.0.05/assembly.fasta
```

results from stats.sh
```bash
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3137	0.1862	0.1863	0.3137	0.0000	0.0000	0.0000	0.3726	0.0400

Main genome scaffold total:         	170905
Main genome contig total:           	171021
Main genome scaffold sequence total:	700.290 MB
Main genome contig sequence total:  	700.279 MB  	0.002% gap
Main genome scaffold N/L50:         	18783/8.836 KB
Main genome contig N/L50:           	18846/8.826 KB
Main genome scaffold N/L90:         	90690/1.661 KB
Main genome contig N/L90:           	90788/1.661 KB
Max scaffold length:                	154.539 KB
Max contig length:                  	154.539 KB
Number of scaffolds > 50 KB:        	554
% main genome in scaffolds > 50 KB: 	5.14%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	       170,905	       171,021	   700,290,210	   700,278,610	 100.00%
    250 	       170,905	       171,021	   700,290,210	   700,278,610	 100.00%
    500 	       162,194	       162,310	   696,243,982	   696,232,382	 100.00%
   1 KB 	       116,877	       116,993	   664,445,469	   664,433,869	 100.00%
 2.5 KB 	        69,296	        69,412	   586,440,380	   586,428,780	 100.00%
   5 KB 	        37,257	        37,369	   472,472,507	   472,461,307	 100.00%
  10 KB 	        15,870	        15,961	   322,795,371	   322,786,271	 100.00%
  25 KB 	         3,554	         3,602	   135,940,459	   135,935,659	 100.00%
  50 KB 	           554	           571	    36,000,509	    35,998,809	 100.00%
 100 KB 	            24	            29	     2,668,938	     2,668,438	  99.98%
```


strange, but I'll take it. gonna run it without the reference and without nanopore data
```bash
# example configuration file 

# DATA is specified as type {PE,JUMP,OTHER,PACBIO} and 5 fields:
# 1)two_letter_prefix 2)mean 3)stdev 4)fastq(.gz)_fwd_reads
# 5)fastq(.gz)_rev_reads. The PE reads are always assumed to be
# innies, i.e. --->.<---, and JUMP are assumed to be outties
# <---.--->. If there are any jump libraries that are innies, such as
# longjump, specify them as JUMP and specify NEGATIVE mean. Reverse reads
# are optional for PE libraries and mandatory for JUMP libraries. Any
# OTHER sequence data (454, Sanger, Ion torrent, etc) must be first
# converted into Celera Assembler compatible .frg files (see
# http://wgs-assembler.sourceforge.com)

DATA
#Illumina paired end reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#if single-end, do not specify <reverse_reads>
#MUST HAVE Illumina paired end reads to use MaSuRCA
PE= pe 350 53  /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz
#Illumina mate pair reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#JUMP= sh 3600 200  /FULL_PATH/short_1.fastq  /FULL_PATH/short_2.fastq
#pacbio OR nanopore reads must be in a single fasta or fastq file with absolute path, can be gzipped
#if you have both types of reads supply them both as NANOPORE type
#PACBIO=/FULL_PATH/pacbio.fa
#NANOPORE=/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.fastq
#Other reads (Sanger, 454, etc) one frg file, concatenate your frg files into one if you have many
#OTHER=/FULL_PATH/file.frg
#synteny-assisted assembly, concatenate all reference genomes into one reference.fa; works for Illumina-only data
#REFERENCE=/home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta

END

PARAMETERS
#PLEASE READ all comments to essential parameters below, and set the parameters according to your project
#set this to 1 if your Illumina jumping library reads are shorter than 100bp
#EXTEND_JUMP_READS=0
#this is k-mer size for deBruijn graph values between 25 and 127 are supported, auto will compute the optimal size based on the read data and GC content
GRAPH_KMER_SIZE = auto
#set this to 1 for all Illumina-only assemblies
#set this to 0 if you have more than 15x coverage by long reads (Pacbio or Nanopore) or any other long reads/mate pairs (Illumina MP, Sanger, 454, etc)
USE_LINKING_MATES = 1
#specifies whether to run the assembly on the grid
#USE_GRID=0
#specifies grid engine to use SGE or SLURM
#GRID_ENGINE=SGE
#specifies queue (for SGE) or partition (for SLURM) to use when running on the grid MANDATORY
#GRID_QUEUE=all.q
#batch size in the amount of long read sequence for each batch on the grid
#GRID_BATCH_SIZE=500000000
#use at most this much coverage by the longest Pacbio or Nanopore reads, discard the rest of the reads
#can increase this to 30 or 35 if your reads are short (N50<7000bp)
#LHE_COVERAGE=25
#set to 0 (default) to do two passes of mega-reads for slower, but higher quality assembly, otherwise set to 1
MEGA_READS_ONE_PASS=0
#this parameter is useful if you have too many Illumina jumping library mates. Typically set it to 60 for bacteria and 300 for the other organisms 
#LIMIT_JUMP_COVERAGE = 300
#these are the additional parameters to Celera Assembler.  do not worry about performance, number or processors or batch sizes -- these are computed automatically. 
#CABOG ASSEMBLY ONLY: set cgwErrorRate=0.25 for bacteria and 0.1<=cgwErrorRate<=0.15 for other organisms.
CA_PARAMETERS =  cgwErrorRate=0.15
#CABOG ASSEMBLY ONLY: whether to attempt to close gaps in scaffolds with Illumina  or long read data
CLOSE_GAPS=1
#number of cpus to use, set this to the number of CPUs/threads per node you will be using
NUM_THREADS = 70
#this is mandatory jellyfish hash size -- a safe value is estimated_genome_size*20
JF_SIZE = 9000000000
#ILLUMINA ONLY. Set this to 1 to use SOAPdenovo contigging/scaffolding module.  
#Assembly will be worse but will run faster. Useful for very large (>=8Gbp) genomes from Illumina-only data
SOAP_ASSEMBLY=0
#If you are doing Hybrid Illumina paired end + Nanopore/PacBio assembly ONLY (no Illumina mate pairs or OTHER frg files).  
#Set this to 1 to use Flye assembler for final assembly of corrected mega-reads.  
#A lot faster than CABOG, AND QUALITY IS THE SAME OR BETTER. 
#Works well even when MEGA_READS_ONE_PASS is set to 1.  
#DO NOT use if you have less than 15x coverage by long reads.
FLYE_ASSEMBLY=0
END
```

```bash
./masurca /home/jon/Working_Files/MaSuRCA-3.4.1/config.txt
Verifying PATHS...
jellyfish OK
runCA OK
createSuperReadsForDirectory.perl OK
creating script file for the actions...done.
execute assemble.sh to run assembly
```

```bash
./assemble.sh


[Tue 14 Jul 2020 01:53:40 PM PDT] Processing pe library reads
[Tue 14 Jul 2020 02:32:09 PM PDT] Average PE read length 150
[Tue 14 Jul 2020 02:32:10 PM PDT] Using kmer size of 99 for the graph
[Tue 14 Jul 2020 02:32:11 PM PDT] MIN_Q_CHAR: 33
[Tue 14 Jul 2020 02:32:11 PM PDT] Creating mer database for Quorum
[Tue 14 Jul 2020 03:01:56 PM PDT] Error correct PE
[Tue 14 Jul 2020 03:37:04 PM PDT] Estimating genome size
[Tue 14 Jul 2020 03:49:04 PM PDT] Estimated genome size: 749318400
[Tue 14 Jul 2020 03:49:04 PM PDT] Creating k-unitigs with k=99
[Tue 14 Jul 2020 04:25:32 PM PDT] Computing super reads from PE 
[Tue 14 Jul 2020 05:56:54 PM PDT] Using linking mates
[Tue 14 Jul 2020 05:58:28 PM PDT] Celera Assembler
[Tue 14 Jul 2020 07:20:23 PM PDT] Overlap/unitig success
[Tue 14 Jul 2020 07:20:23 PM PDT] Recomputing A-stat for super-reads
[Tue 14 Jul 2020 07:40:33 PM PDT] Filtering overlaps
[Tue 14 Jul 2020 08:38:02 PM PDT] Recomputing A-stat for super-reads
[Tue 14 Jul 2020 09:18:15 PM PDT] CA success
[Tue 14 Jul 2020 09:18:16 PM PDT] No gap closing possible
[Tue 14 Jul 2020 09:18:16 PM PDT] Removing redundant scaffolds
[Tue 14 Jul 2020 09:33:13 PM PDT] Assembly complete, final scaffold sequences are in CA/final.genome.scf.fasta
[Tue 14 Jul 2020 09:33:13 PM PDT] All done
[Tue 14 Jul 2020 09:33:13 PM PDT] Final stats for CA/final.genome.scf.fasta
N50 2421
Sequence 708122755
Average 1578.98
E-size 3529.35
Count 448469
```

stats
```bash
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3145	0.1880	0.1867	0.3108	0.0000	0.0000	0.0000	0.3747	0.0467

Main genome scaffold total:         	448469
Main genome contig total:           	448469
Main genome scaffold sequence total:	708.123 MB
Main genome contig sequence total:  	708.123 MB  	0.000% gap
Main genome scaffold N/L50:         	83270/2.421 KB
Main genome contig N/L50:           	83270/2.421 KB
Main genome scaffold N/L90:         	294538/686
Main genome contig N/L90:           	294538/686
Max scaffold length:                	81.973 KB
Max contig length:                  	81.973 KB
Number of scaffolds > 50 KB:        	9
% main genome in scaffolds > 50 KB: 	0.08%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	       448,469	       448,469	   708,122,755	   708,122,755	 100.00%
    250 	       448,469	       448,469	   708,122,755	   708,122,755	 100.00%
    500 	       349,587	       349,587	   669,742,033	   669,742,033	 100.00%
   1 KB 	       228,570	       228,570	   582,441,501	   582,441,501	 100.00%
 2.5 KB 	        79,077	        79,077	   343,781,648	   343,781,648	 100.00%
   5 KB 	        18,359	        18,359	   136,819,225	   136,819,225	 100.00%
  10 KB 	         1,994	         1,994	    30,249,963	    30,249,963	 100.00%
  25 KB 	           173	           173	     5,731,193	     5,731,193	 100.00%
  50 KB 	             9	             9	       562,535	       562,535	 100.00%
```
sudo chmod 777 -R /home/jon/Working_Files/MaSuRCA-3.4.1/bin


running it with nanopore data but with LHE coverage not set
```bash
./masurca /home/jon/Working_Files/MaSuRCA-3.4.1/config.txt
Verifying PATHS...
jellyfish OK
runCA OK
createSuperReadsForDirectory.perl OK
creating script file for the actions...done.
execute assemble.sh to run assembly
```

```bash
./assemble.sh
```


# Wed 15 Jul 2020 11:48:25 AM PDT

finished
```bash
[Wed 15 Jul 2020 12:10:00 AM PDT] Processing pe library reads
[Wed 15 Jul 2020 12:49:51 AM PDT] Average PE read length 150
[Wed 15 Jul 2020 12:49:51 AM PDT] Using kmer size of 99 for the graph
[Wed 15 Jul 2020 12:49:52 AM PDT] MIN_Q_CHAR: 33
[Wed 15 Jul 2020 12:49:52 AM PDT] Creating mer database for Quorum
[Wed 15 Jul 2020 01:17:33 AM PDT] Error correct PE
[Wed 15 Jul 2020 01:53:37 AM PDT] Estimating genome size
[Wed 15 Jul 2020 02:03:09 AM PDT] Estimated genome size: 749318400
[Wed 15 Jul 2020 02:03:09 AM PDT] Creating k-unitigs with k=99
[Wed 15 Jul 2020 02:41:23 AM PDT] Computing super reads from PE 
[Wed 15 Jul 2020 04:22:49 AM PDT] Using linking mates
[Wed 15 Jul 2020 04:22:49 AM PDT] Using CABOG from /home/jon/Working_Files/MaSuRCA-3.4.1/bin/../CA8/Linux-amd64/bin
[Wed 15 Jul 2020 04:22:49 AM PDT] Running mega-reads correction/assembly
[Wed 15 Jul 2020 04:22:49 AM PDT] Using mer size 15 for mapping, B=15, d=0.02
[Wed 15 Jul 2020 04:22:49 AM PDT] Estimated Genome Size 749318400
[Wed 15 Jul 2020 04:22:49 AM PDT] Estimated Ploidy 1
[Wed 15 Jul 2020 04:22:49 AM PDT] Using 70 threads
[Wed 15 Jul 2020 04:22:49 AM PDT] Output prefix mr.41.15.15.0.02
[Wed 15 Jul 2020 04:22:49 AM PDT] Using 25x of the longest ONT reads
[Wed 15 Jul 2020 04:22:50 AM PDT] Reducing super-read k-mer size
[Wed 15 Jul 2020 04:53:00 AM PDT] Mega-reads pass 1
[Wed 15 Jul 2020 04:53:00 AM PDT] Running locally in 1 batch
[Wed 15 Jul 2020 04:59:33 AM PDT] Mega-reads pass 2
[Wed 15 Jul 2020 04:59:33 AM PDT] Running locally in 1 batch
[Wed 15 Jul 2020 05:01:42 AM PDT] Refining alignments
[Wed 15 Jul 2020 05:01:57 AM PDT] Computing allowed merges
[Wed 15 Jul 2020 05:01:58 AM PDT] Joining
[Wed 15 Jul 2020 05:01:59 AM PDT] Gap consensus
[Wed 15 Jul 2020 05:02:00 AM PDT] Generating assembly input files
[Wed 15 Jul 2020 05:02:16 AM PDT] Coverage of the mega-reads less than 5 -- using the super reads as well
[Wed 15 Jul 2020 05:02:51 AM PDT] Coverage threshold for splitting unitigs is 15 minimum ovl 250
[Wed 15 Jul 2020 05:02:51 AM PDT] Running assembly
[Wed 15 Jul 2020 05:53:29 AM PDT] Recomputing A-stat
[Wed 15 Jul 2020 06:24:04 AM PDT] Mega-reads initial assembly complete
[Wed 15 Jul 2020 06:24:04 AM PDT] No gap closing possible
[Wed 15 Jul 2020 06:24:04 AM PDT] Removing redundant scaffolds
[Wed 15 Jul 2020 06:24:42 AM PDT] Assembly complete, final scaffold sequences are in CA.mr.41.15.15.0.02/final.genome.scf.fasta
[Wed 15 Jul 2020 06:24:42 AM PDT] All done
[Wed 15 Jul 2020 06:24:42 AM PDT] Final stats for CA.mr.41.15.15.0.02/final.genome.scf.fasta
N50 3030
Sequence 53732764
Average 2542.72
E-size 3788.22
Count 21132
```


# Mon 05 Oct 2020 10:27:39 PM PDT

ok to summarize what I have done so far with masurca

Assembly 1: Paired end data 350, 53
			nanopore data
			LHE coverage=25
			JF_SIZE = 18000000000
			CLOSE_GAPS=1

			N50 3034
			Sequence 54074486
			Average 2546
			E-size 3780.96
			Count 21239

Assembly 2: Paired end data 350, 53
			nanopore data
			USE_LINKING_MATES = 1
			LHE_COVERAGE=25
			CLOSE_GAPS=1
			JF_SIZE = 9000000000

			N50 3027
			Sequence 54056268
			Average 2543.47
			E-size 3766.15
			Count 21253

			deleted

Assembly 3: Paired end data 350, 53
			nanopore data
			reference genome
			USE_LINKING_MATES = 1
			CLOSE_GAPS=1
			JF_SIZE = 9000000000

			Total length:	700290210
			Fragments:	170905
			Fragments N50:	8836
			Largest frg:	154539
			Scaffolds:	116
			Mean coverage:	2

			May not have finished correctly

Assembly 4: Paired end data 350, 53
			USE_LINKING_MATES = 1
			JF_SIZE = 9000000000

			N50 2421
			Sequence 708122755
			Average 1578.98
			E-size 3529.35
			Count 448469

to run 
done - paired end data with reference genome
done - rerun assembly 3
does already - maybe try the flye assembler?
can't - maybe try the chromosome assembler
done - run masurca with just illumina and no  reference or nanopore

# Tue 06 Oct 2020 09:56:58 PM PDT

running masurca with illumina paired end data and with the A. japonicus reference genome

```bash
./masurca /home/jon/Working_Files/MaSuRCA-3.4.1/illumina_reference_config.txt
```

results
```bash
[Tue 06 Oct 2020 10:00:26 PM PDT] Processing pe library reads
[Tue 06 Oct 2020 10:40:09 PM PDT] Average PE read length 150
[Tue 06 Oct 2020 10:40:10 PM PDT] Using kmer size of 99 for the graph
[Tue 06 Oct 2020 10:40:11 PM PDT] MIN_Q_CHAR: 33
[Tue 06 Oct 2020 10:40:11 PM PDT] Creating mer database for Quorum
[Tue 06 Oct 2020 11:09:02 PM PDT] Error correct PE
[Tue 06 Oct 2020 11:45:47 PM PDT] Estimating genome size
[Wed 07 Oct 2020 12:01:14 AM PDT] Estimated genome size: 749318400
[Wed 07 Oct 2020 12:01:14 AM PDT] Creating k-unitigs with k=99
[Wed 07 Oct 2020 12:38:58 AM PDT] Computing super reads from PE 
[Wed 07 Oct 2020 02:21:27 AM PDT] Using linking mates
[Wed 07 Oct 2020 02:21:27 AM PDT] Running synteny-guided assembly
[Wed 07 Oct 2020 02:21:27 AM PDT] Using mer size 17 for mapping, B=20, d=0.05
[Wed 07 Oct 2020 02:21:27 AM PDT] Using CA installation from /home/jon/Working_Files/MaSuRCA-3.4.1/bin/../CA8/Linux-amd64/bin
[Wed 07 Oct 2020 02:21:27 AM PDT] Using 70 threads
[Wed 07 Oct 2020 02:21:27 AM PDT] Output prefix mr.41.17.20.0.05
[Wed 07 Oct 2020 02:21:27 AM PDT] Reducing super-read k-mer size
[Wed 07 Oct 2020 02:52:49 AM PDT] Preparing reference
[Wed 07 Oct 2020 02:53:00 AM PDT] Running preliminary assembly
[Wed 07 Oct 2020 02:53:00 AM PDT] Mega-reads pass 1
compute_psa 4050643 2154752230
[Wed 07 Oct 2020 03:49:55 AM PDT] Recomputing A-stat
[Wed 07 Oct 2020 03:54:42 AM PDT] Joining
[Wed 07 Oct 2020 04:13:03 AM PDT] Polishing reference contigs
[Wed 07 Oct 2020 05:06:30 AM PDT] Final assembly
[Wed 07 Oct 2020 03:19:43 PM PDT] Final output sequences are in flye.mr.41.17.20.0.05/scaffolds.fasta
Error with file 'flye.mr.41.17.20.0.05/scaffolds.fasta'
```

weird, that error with the file is strange - the scaffolds.fasta is missing, but there is an assembly.fasta in the folder. The flye.log file also contains this
```bash
	Total length:	698906982
	Fragments:	169818
	Fragments N50:	8911
	Largest frg:	188381
	Scaffolds:	130
	Mean coverage:	2

[2020-10-07 15:19:43] root: INFO: Final assembly: /home/jon/Working_Files/MaSuRCA-3.4.1/bin/flye.mr.41.17.20.0.05/assembly.fasta
```
soo I don't know waht to this about and should probably look into it more


# Thu 08 Oct 2020 09:11:41 AM PDT

ran masurca with illumina and no reference. see results below
```bash
[Wed 07 Oct 2020 04:23:17 PM PDT] Processing pe library reads
[Wed 07 Oct 2020 05:01:41 PM PDT] Average PE read length 150
[Wed 07 Oct 2020 05:01:42 PM PDT] Using kmer size of 99 for the graph
[Wed 07 Oct 2020 05:01:42 PM PDT] MIN_Q_CHAR: 33
[Wed 07 Oct 2020 05:01:42 PM PDT] Creating mer database for Quorum
[Wed 07 Oct 2020 05:32:21 PM PDT] Error correct PE
[Wed 07 Oct 2020 06:08:24 PM PDT] Estimating genome size
[Wed 07 Oct 2020 06:23:20 PM PDT] Estimated genome size: 749318400
[Wed 07 Oct 2020 06:23:20 PM PDT] Creating k-unitigs with k=99
[Wed 07 Oct 2020 07:05:56 PM PDT] Computing super reads from PE 
[Wed 07 Oct 2020 08:37:07 PM PDT] Using linking mates
[Wed 07 Oct 2020 08:38:40 PM PDT] Celera Assembler
[Wed 07 Oct 2020 10:03:02 PM PDT] Overlap/unitig success
[Wed 07 Oct 2020 10:03:02 PM PDT] Recomputing A-stat for super-reads
[Wed 07 Oct 2020 10:22:14 PM PDT] Filtering overlaps
[Wed 07 Oct 2020 11:23:04 PM PDT] Recomputing A-stat for super-reads
[Wed 07 Oct 2020 11:50:52 PM PDT] CA success
[Wed 07 Oct 2020 11:50:53 PM PDT] No gap closing possible
[Wed 07 Oct 2020 11:50:53 PM PDT] Removing redundant scaffolds
[Thu 08 Oct 2020 12:05:31 AM PDT] Assembly complete, final scaffold sequences are in CA/final.genome.scf.fasta
[Thu 08 Oct 2020 12:05:31 AM PDT] All done
[Thu 08 Oct 2020 12:05:31 AM PDT] Final stats for CA/final.genome.scf.fasta
N50 2421
Sequence 707870530
Average 1579.07
E-size 3531.46
Count 448282
```

this was another run with illumina, reference, and a large jellyfish size thing
```bash
[Thu 08 Oct 2020 09:27:11 AM PDT] Processing pe library reads
[Thu 08 Oct 2020 10:06:19 AM PDT] Average PE read length 150
[Thu 08 Oct 2020 10:06:19 AM PDT] Using kmer size of 99 for the graph
[Thu 08 Oct 2020 10:06:20 AM PDT] MIN_Q_CHAR: 33
[Thu 08 Oct 2020 10:06:20 AM PDT] Creating mer database for Quorum
[Thu 08 Oct 2020 10:38:40 AM PDT] Error correct PE
[Thu 08 Oct 2020 11:22:11 AM PDT] Estimating genome size
[Thu 08 Oct 2020 11:37:14 AM PDT] Estimated genome size: 749318400
[Thu 08 Oct 2020 11:37:14 AM PDT] Creating k-unitigs with k=99
[Thu 08 Oct 2020 12:14:30 PM PDT] Computing super reads from PE 
[Thu 08 Oct 2020 01:57:39 PM PDT] Using linking mates
[Thu 08 Oct 2020 01:57:39 PM PDT] Running synteny-guided assembly
[Thu 08 Oct 2020 01:57:39 PM PDT] Using mer size 17 for mapping, B=20, d=0.05
[Thu 08 Oct 2020 01:57:39 PM PDT] Using CA installation from /home/jon/Working_Files/MaSuRCA-3.4.1/bin/../CA8/Linux-amd64/bin
[Thu 08 Oct 2020 01:57:39 PM PDT] Using 70 threads
[Thu 08 Oct 2020 01:57:39 PM PDT] Output prefix mr.41.17.20.0.05
[Thu 08 Oct 2020 01:57:39 PM PDT] Reducing super-read k-mer size
[Thu 08 Oct 2020 02:27:25 PM PDT] Preparing reference
[Thu 08 Oct 2020 02:27:39 PM PDT] Running preliminary assembly
[Thu 08 Oct 2020 02:27:39 PM PDT] Mega-reads pass 1
compute_psa 4050737 2154723995
[Thu 08 Oct 2020 03:26:21 PM PDT] Recomputing A-stat
[Thu 08 Oct 2020 03:29:48 PM PDT] Joining
[Thu 08 Oct 2020 03:49:56 PM PDT] Polishing reference contigs
[Thu 08 Oct 2020 04:43:16 PM PDT] Final assembly
[Fri 09 Oct 2020 03:04:07 AM PDT] Final output sequences are in flye.mr.41.17.20.0.05/scaffolds.fasta
Error with file 'flye.mr.41.17.20.0.05/scaffolds.fasta'
```

there's that error again. sigh
```bash
	Total length:	700881828
	Fragments:	171316
	Fragments N50:	8827
	Largest frg:	132787
	Scaffolds:	111
	Mean coverage:	2
```