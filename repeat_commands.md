## repeatmodeler commands for platanus-allee assembly
```bash
./RepeatModeler-2.0/BuildDatabase -name P_cali ../assembly/out_consensusScaffold.fa
./RepeatModeler-2.0/RepeatModeler -database P_cali -pa 20 -LTRStruct 2> repeatmodelerstderr.log
```

## repeatmodeler commands for redundans assembly

```bash
./RepeatModeler-2.0/BuildDatabase -name P_cali_redundans_ajap ../../redundans-pcali-ajap_assembly.fa

./RepeatModeler-2.0/RepeatModeler -database P_cali_redundans_ajap -pa 20 -LTRStruct 2> redun_pcali_ajap_repeatmodelerstderr.log

```

the contig histogram 
```bash
Search Engine = rmblast
LTR Structural Analysis: Enabled
Random Number Seed: 1580350821
Database = P_cali_redundans_ajap ...............................
  - Sequences = 304189
  - Bases = 878550047
  - N50 = 232861
  - Contig Histogram:
  Size(bp)                                                        Count
  -----------------------------------------------------------------------
  2198101-2355094 |                                                   [  ]
  2041108-2198100 |                                                   [  ]
  1884115-2041107 |                                                   [ 2 ]
  1727122-1884114 |                                                   [  ]
  1570129-1727121 |                                                   [ 1 ]
  1413136-1570128 |                                                   [ 2 ]
  1256143-1413135 |                                                   [ 6 ]
  1099150-1256142 |                                                   [ 9 ]
  942157-1099149  |                                                   [ 20 ]
  785164-942156   |                                                   [ 53 ]
  628171-785163   |                                                   [ 88 ]
  471178-628170   |                                                   [ 195 ]
  314185-471177   |                                                   [ 294 ]
  157192-314184   |                                                   [ 606 ]
  200-157192      |************************************************** [ 302912 ]
  ```

#  Fri 31 Jan 2020 01:32:20 AM PST

hard drives filled up somehow and the run stopped. I deleted abunch of databases and am going to try and restart the run.

```bash
./RepeatModeler-2.0/RepeatModeler -recoverDir /home/jon/Working_Files/Patanus-Allee/repeats/RM_19724.WedJan291821212020 -database P_cali_redundans_ajap -pa 20 -LTRStruct 2> redun_pcali_ajap_repeatmodelerstderr.log
```

weird it is saying it didn't get passed round 1 even though it clearly did. ohwell
```bash
Oops...the RM_19724.WedJan291821212020 run did not get passed round-1.
It makes more sense to restart this run from the beginning.
Remove the -recoverDir option and rerun the program.
```

# Mon 03 Feb 2020 03:09:29 PM PST

ballz lost the repeatmasker commands and data when my workstation ssd crashed. 
also, ltrharvest didn't work because rmblast apparently wasn't installed. at somepoint I should go back and look at that. 

repeatmasker commands
```bash
./RepeatMasker-4.1.0/RepeatMasker/RepeatMasker  -e rmblast -pa 20 -s -lib /home/jon/Working_Files/Patanus-Allee/repeats/RM_48126.MonDec20802362019/consensi.fa.classified -dir repeatmasking -xsmall -gff -u -excln -poly /home/jon/Working_Files/pcali_ajap.redundans.platanus.fa
```

I just found an example where someone combined their repeatmodeler library with the repbase library. I think I will do this. Well, I was going to until I found out that repbase is now behind a paywall. those fuckers.....

also note that the -gff option may only output to gff2 and that there is a scrip c alled rmOutToGFF3.pl that can be used to create a gff3 file from the fasta.

there is a script called buildSummary.pl, it will need a chromosome/scaffold/contig size in tab delimited file. samtools faidx will do this. 

also this will convert the output to something more R friendly. 
```bash
tr -s ' ' my_input_genome_assembly.fasta.out.detalied < | sed 's/^ *//g' | tr ' ' '/t' > my_input_genome_assembly.fasta.out.detailed.tab
```

source: github.com/umd-byob/presentations/2015/0324-RepeatMasker-RepeatModeler


# Mon 25 May 2020 12:24:58 PM PDT

## repeatmodeler commands for platanus-allee assembly
```bash
./RepeatModeler-2.0/BuildDatabase -name P_cali /home/jon/Working_Files/Patanus-Allee/assembly/out_consensusScaffold.fa
```
success
```bash
Number of sequences (bp) added to database: 684753 ( 838807926 bp )
```

so it looks like I will need to reinstall repeatmodeler/repeatmasker. I am going to be lazy and use their docker container. hopefully.

```bash
git clone https://github.com/Dfam-consortium/TETools.git

cd TETools
./getsrc.sh

sudo docker build -v /home/jon/ -t org/name:tag .
curl -sSLO https://github.com/Dfam-consortium/TETools/raw/master/dfam-tetools.sh
chmod +x dfam-tetools.sh
./dfam-tetools.sh --trf_prgm=/home/jon/Working_Files/repeats/trf 
```

looks like the docker container is up and running
```bash
BuildDatabase -name P_cali /home/jon/Working_Files/Patanus-Allee/assembly/out_consensusScaffold.fa
RepeatModeler -LTRStruct -pa 20 -database P_cali 
```

standard output
```bash
RepeatModeler Version 2.0.1
===========================
Search Engine = rmblast 2.10.0+
Dependencies: TRF 4.09, RECON 1.08, RepeatScout 1.0.5, RepeatMasker 4.1.0
LTR Structural Analysis: Enabled ( GenomeTools 1.6.0, LTR_Retriever v2.8,
                                   Ninja 0.97-cluster_only, MAFFT 7.453,
                                   CD-HIT 4.8.1 )
Random Number Seed: 1590444345
Database = P_cali .....................................................................
  - Sequences = 684753
  - Bases = 838807926
  - N50 = 8569
  - Contig Histogram:
  Size(bp)                                                        Count
  -----------------------------------------------------------------------
  125796-134774 |                                                   [  ]
  116818-125795 |                                                   [ 3 ]
  107841-116818 |                                                   [ 3 ]
  98863-107840  |                                                   [ 9 ]
  89886-98863   |                                                   [ 14 ]
  80908-89885   |                                                   [ 14 ]
  71930-80907   |                                                   [ 54 ]
  62953-71930   |                                                   [ 94 ]
  53975-62952   |                                                   [ 187 ]
  44998-53975   |                                                   [ 373 ]
  36020-44997   |                                                   [ 808 ]
  27042-36019   |                                                   [ 1837 ]
  18065-27042   |                                                   [ 4499 ]
  9087-18064    |*                                                  [ 13821 ]
  110-9087      |************************************************** [ 663036 ]

  WARN: The N50 for this assembly is low ( <10,000 ).  The de novo methods
        employed by RepeatModeler are intended for use with long contiguous
        sequences and may not perform well with an over-abundance of short
        contigs in the database.

Using output directory = /home/jon/Working_Files/repeats/TETools/RM_47057.MonMay251506542020
Storage Throughput = fair ( 646.48 MB/s )

Ready to start the sampling process.
INFO: The runtime of RepeatModeler heavily depends on the quality of the assembly
      and the repetitive content of the sequences.  It is not imperative
      that RepeatModeler completes all rounds in order to obtain useful
      results.  At the completion of each round, the files ( consensi.fa, and
      families.stk ) found in:
      /home/jon/Working_Files/repeats/TETools/RM_47057.MonMay251506542020/ 
      will contain all results produced thus far. These files may be 
      manually copied and run through RepeatClassifier should the program
      be terminated early.

```

# Tue 26 May 2020 08:27:20 PM PDT

whelp, blew a braker. have to rerun it. 

starting up the singularity container
```bash
./dfam-tetools.sh --trf_prgm=/home/jon/Working_Files/repeats/trf 
```

rerunning repeatmodeler, but attempting a restart. 
```bash
RepeatModeler -LTRStruct -pa 20 -recoverDir /home/jon/Working_Files/repeats/TETools/RM_47057.MonMay251506542020 -database P_cali 
```

# Wed 03 Jun 2020 12:22:37 PM PDT

well fuck, I fucked with the graphics card stuff and had to restore from backup, and apparently abunch of stuff didn't save or the backup happened just before stuff finished. I can't tell if the repeatmodeler run finished and I lost the repeatmasker output too. so I am going to rerun repeatmodeler and repeatmasker. 

```bash
./dfam-tetools.sh --trf_prgm=/home/jon/Working_Files/repeats/trf 

RepeatModeler -LTRStruct -pa 20 -database P_cali 
```

initial output
```bash
RepeatModeler Version 2.0.1
===========================
Search Engine = rmblast 2.10.0+
Dependencies: TRF 4.09, RECON 1.08, RepeatScout 1.0.5, RepeatMasker 4.1.0
LTR Structural Analysis: Enabled ( GenomeTools 1.6.0, LTR_Retriever v2.8,
                                   Ninja 0.97-cluster_only, MAFFT 7.453,
                                   CD-HIT 4.8.1 )
Random Number Seed: 1591212354
Database = P_cali .....................................................................
  - Sequences = 684753
  - Bases = 838807926
  - N50 = 8569
  - Contig Histogram:
  Size(bp)                                                        Count
  -----------------------------------------------------------------------
  125796-134774 |                                                   [  ]
  116818-125795 |                                                   [ 3 ]
  107841-116818 |                                                   [ 3 ]
  98863-107840  |                                                   [ 9 ]
  89886-98863   |                                                   [ 14 ]
  80908-89885   |                                                   [ 14 ]
  71930-80907   |                                                   [ 54 ]
  62953-71930   |                                                   [ 94 ]
  53975-62952   |                                                   [ 187 ]
  44998-53975   |                                                   [ 373 ]
  36020-44997   |                                                   [ 808 ]
  27042-36019   |                                                   [ 1837 ]
  18065-27042   |                                                   [ 4499 ]
  9087-18064    |*                                                  [ 13821 ]
  110-9087      |************************************************** [ 663036 ]

  WARN: The N50 for this assembly is low ( <10,000 ).  The de novo methods
        employed by RepeatModeler are intended for use with long contiguous
        sequences and may not perform well with an over-abundance of short
        contigs in the database.
Using output directory = /home/jon/Working_Files/repeats/TETools/RM_71511.WedJun31227072020
Storage Throughput = good ( 752.82 MB/s )

Ready to start the sampling process.
INFO: The runtime of RepeatModeler heavily depends on the quality of the assembly
      and the repetitive content of the sequences.  It is not imperative
      that RepeatModeler completes all rounds in order to obtain useful
      results.  At the completion of each round, the files ( consensi.fa, and
      families.stk ) found in:
      /home/jon/Working_Files/repeats/TETools/RM_71511.WedJun31227072020/ 
      will contain all results produced thus far. These files may be 
      manually copied and run through RepeatClassifier should the program
      be terminated early.
```

# Fri 05 Jun 2020 09:28:21 AM PDT

and success and it is backedup
```bash
RepeatScout/RECON discovery complete: 3316 families found


LTR Structural Analysis
=======================
Running LtrHarvest...     : 00:27:07 (hh:mm:ss) Elapsed Time
Running Ltr_retriever...  : 00:18:06 (hh:mm:ss) Elapsed Time
Aligning instances...     : 00:03:06 (hh:mm:ss) Elapsed Time
Clustering...             : 00:00:01 (hh:mm:ss) Elapsed Time
Refining families...      : 00:06:58 (hh:mm:ss) Elapsed Time
Program Time: 00:55:18 (hh:mm:ss) Elapsed Time
  -- Clustering results with previous rounds...
       - 3316 RepeatScout/RECON families
       - 111 LTRPipeline families
       - Removed 54 redundant LTR families.
       - Final family count = 3373
LTRPipeline Time: 00:56:53 (hh:mm:ss) Elapsed Time


RepeatClassifier Version 2.0.1
======================================
Search Engine = rmblast
  - Looking for Simple and Low Complexity sequences..
  - Looking for similarity to known repeat proteins..
  - Looking for similarity to known repeat consensi..
Classification Time: 01:54:46 (hh:mm:ss) Elapsed Time


Program Time: 37:49:13 (hh:mm:ss) Elapsed Time
Working directory:  /home/jon/Working_Files/repeats/TETools/RM_71511.WedJun31227072020
may be deleted unless there were problems with the run.

The results have been saved to:
  P_cali-families.fa  - Consensus sequences for each family identified.
  P_cali-families.stk - Seed alignments for each family identified.

The RepeatModeler stockholm file is formatted so that it can
easily be submitted to the Dfam database.  Please consider contributing
curated families to this open database and be a part of this growing
community resource.  For more information contact help@dfam.org.
```

now time to run repeatmasker
```bash
RepeatMasker -e rmblast -pa 20 -s -lib /home/jon/Working_Files/repeats/TETools/P_cali-families.fa -dir repeatmasking -xsmall -gff -u -excln -poly /home/jon/Working_Files/genome_assemblies/pcali_genome.platanus.fa
```


forgot about this detail.
```bash
Checking for E. coli insertion elements
FastaDB::_cleanIndexAndCompact(): Fasta file contains a sequence identifier which is too long ( max id length = 50 )
 at /opt/RepeatMasker/RepeatMasker line 1541.
WARNING: Retrying batch ( 19 ) [ 25,, 38301]...

Checking for E. coli insertion elements
FastaDB::_cleanIndexAndCompact(): Fasta file contains a sequence identifier which is too long ( max id length = 50 )
 at /opt/RepeatMasker/RepeatMasker line 1541.
WARNING: Retrying batch ( 20 ) [ 25,, 38302]...

Checking for E. coli insertion elements
FastaDB::_cleanIndexAndCompact(): Fasta file contains a sequence identifier which is too long ( max id length = 50 )
 at /opt/RepeatMasker/RepeatMasker line 1541.


FATAL ERROR: RepeatMasker giving up. One or more
batches failed!  Unfortunately this type of error
cannot be recovered from. Please submit the following
details to the feedback page at the repeatmasker
website:
```

using the rn_pcali_platanus.fa file which has only the scaffold id in the fasta headers
```bash
RepeatMasker -e rmblast -pa 20 -s -lib /home/jon/Working_Files/repeats/TETools/P_cali-families.fa -dir repeatmasking -xsmall -gff -u -excln -poly /home/jon/Working_Files/genome_assemblies/rn_pcali_platanus.fa
```

and success


# Thu 18 Jun 2020 08:25:15 PM PDT

just realized that if I messed up the platanus assembly repeat masking I probably did the same for the redundans assembly and should remask it and rerun braker2 on it. probably help with the gene predictions. 

```bash
./dfam-tetools.sh --trf_prgm=/home/jon/Working_Files/repeats/trf 

BuildDatabase -name P_cali_redun /home/jon/Working_Files/genome_assemblies/pcali_ajap.redundans.platanus.fasta

RepeatModeler -LTRStruct -pa 20 -database P_cali_redun
```

# Sat 20 Jun 2020 08:39:14 AM PDT

that took a bit. now to run repeatmasker

```bash
RepeatMasker -e rmblast -pa 20 -s -lib /home/jon/Working_Files/repeats/TETools/P_cali_redun-families.fa -dir repeatmasking -xsmall -gff -u -excln -poly /home/jon/Working_Files/genome_assemblies/pcali_ajap.redundans.platanus.fasta
```

# Mon 14 Dec 2020 04:19:39 PM PST

building database for running repeatmodeler
```bash
BuildDatabase -name pcali_ragout assemblies/pcali_ragout.fa
BuildDatabase -name platanus-allee_illumina_step10 assemblies/platanus-allee_illumina_step10.fa
BuildDatabase -name redundans_platanus-allee_step10_ajap-reference assemblies/redundans_platanus-allee_step10_ajap-reference.fa
```

running repeatmodeler on all three
```bash
RepeatModeler -LTRStruct -pa 8 -database  pcali_ragout

RepeatModeler -LTRStruct -pa 8 -database  platanus-allee_illumina_step10

RepeatModeler -LTRStruct -pa 8 -database  redundans_platanus-allee_step10_ajap-reference
```


ehhh, I should filter short reads out of all 3 assemblies probably.

# Thu 17 Dec 2020 06:55:02 AM PST

They all finished!!! I didn't filter for short reads either. So not bad. 
```bash
RepeatScout/RECON discovery complete: 2230 families found


LTR Structural Analysis
=======================
Running LtrHarvest...     : 00:17:03 (hh:mm:ss) Elapsed Time
Running Ltr_retriever...  : 00:05:54 (hh:mm:ss) Elapsed Time
Aligning instances...     : 00:00:10 (hh:mm:ss) Elapsed Time
Clustering...             : 00:00:00 (hh:mm:ss) Elapsed Time
Refining families...      : 00:00:38 (hh:mm:ss) Elapsed Time
Program Time: 00:23:45 (hh:mm:ss) Elapsed Time
  -- Clustering results with previous rounds...
       - 2230 RepeatScout/RECON families
       - 19 LTRPipeline families
       - Removed 3 redundant LTR families.
       - Final family count = 2246
LTRPipeline Time: 00:24:55 (hh:mm:ss) Elapsed Time


RepeatClassifier Version 2.0.1
======================================
Search Engine = rmblast
  - Looking for Simple and Low Complexity sequences..
  - Looking for similarity to known repeat proteins..
  - Looking for similarity to known repeat consensi..
Classification Time: 00:53:52 (hh:mm:ss) Elapsed Time


Program Time: 54:22:19 (hh:mm:ss) Elapsed Time
Working directory:  /work/RM_8.TueDec150033082020
may be deleted unless there were problems with the run.

The results have been saved to:
  redundans_platanus-allee_step10_ajap-reference-families.fa  - Consensus sequences for each family identified.
  redundans_platanus-allee_step10_ajap-reference-families.stk - Seed alignments for each family identified.

The RepeatModeler stockholm file is formatted so that it can
easily be submitted to the Dfam database.  Please consider contributing
curated families to this open database and be a part of this growing
community resource.  For more information contact help@dfam.org.
```

```bash
RepeatScout/RECON discovery complete: 3288 families found


LTR Structural Analysis
=======================
Running LtrHarvest...     : 00:27:01 (hh:mm:ss) Elapsed Time
Running Ltr_retriever...  : 00:18:41 (hh:mm:ss) Elapsed Time
Aligning instances...     : 00:02:31 (hh:mm:ss) Elapsed Time
Clustering...             : 00:00:00 (hh:mm:ss) Elapsed Time
Refining families...      : 00:08:22 (hh:mm:ss) Elapsed Time
Program Time: 00:56:35 (hh:mm:ss) Elapsed Time
  -- Clustering results with previous rounds...
       - 3288 RepeatScout/RECON families
       - 128 LTRPipeline families
       - Removed 38 redundant LTR families.
       - Final family count = 3378
LTRPipeline Time: 00:59:38 (hh:mm:ss) Elapsed Time


RepeatClassifier Version 2.0.1
======================================
Search Engine = rmblast
  - Looking for Simple and Low Complexity sequences..
  - Looking for similarity to known repeat proteins..
  - Looking for similarity to known repeat consensi..
Classification Time: 01:58:46 (hh:mm:ss) Elapsed Time


Program Time: 59:36:50 (hh:mm:ss) Elapsed Time
Working directory:  /work/RM_9.TueDec150032442020
may be deleted unless there were problems with the run.

The results have been saved to:
  platanus-allee_illumina_step10-families.fa  - Consensus sequences for each family identified.
  platanus-allee_illumina_step10-families.stk - Seed alignments for each family identified.

The RepeatModeler stockholm file is formatted so that it can
easily be submitted to the Dfam database.  Please consider contributing
curated families to this open database and be a part of this growing
community resource.  For more information contact help@dfam.org.
```

```bash
RepeatScout/RECON discovery complete: 3302 families found


LTR Structural Analysis
=======================
Running LtrHarvest...     : 00:28:50 (hh:mm:ss) Elapsed Time
Running Ltr_retriever...  : 00:18:36 (hh:mm:ss) Elapsed Time
Aligning instances...     : 00:02:33 (hh:mm:ss) Elapsed Time
Clustering...             : 00:00:00 (hh:mm:ss) Elapsed Time
Refining families...      : 00:06:55 (hh:mm:ss) Elapsed Time
Program Time: 00:56:54 (hh:mm:ss) Elapsed Time
  -- Clustering results with previous rounds...
       - 3302 RepeatScout/RECON families
       - 109 LTRPipeline families
       - Removed 43 redundant LTR families.
       - Final family count = 3368
LTRPipeline Time: 01:00:04 (hh:mm:ss) Elapsed Time


RepeatClassifier Version 2.0.1
======================================
Search Engine = rmblast
  - Looking for Simple and Low Complexity sequences..
  - Looking for similarity to known repeat proteins..
  - Looking for similarity to known repeat consensi..
Classification Time: 01:58:49 (hh:mm:ss) Elapsed Time


Program Time: 61:00:46 (hh:mm:ss) Elapsed Time
Working directory:  /work/RM_38.TueDec150032222020
may be deleted unless there were problems with the run.

The results have been saved to:
  pcali_ragout-families.fa  - Consensus sequences for each family identified.
  pcali_ragout-families.stk - Seed alignments for each family identified.

The RepeatModeler stockholm file is formatted so that it can
easily be submitted to the Dfam database.  Please consider contributing
curated families to this open database and be a part of this growing
community resource.  For more information contact help@dfam.org.
```

Fragmented assemblies show more families discovered. Not too surprising. 

downloading the complete dfam database for repeatmasking
```bash
wget https://www.dfam.org/releases/Dfam_3.3/families/Dfam.h5.gz
```
yeesh, 80gb when unzipped

running repeatmasker
```bash
RepeatMasker \
  -e rmblast \
  -pa 6 \
  -lib redundans_platanus-allee_step10_ajap-reference-families.fa \
  -dir redundans_platanus-allee_step10_ajap-reference-families/Dfam.h5 \
  -xsmall \
  -gff \
  -u \
  -excln \
  -poly \
  -html \
  assemblies/redundans_platanus-allee_step10_ajap-reference.fa
```

cool. So I am using the smaller dfam library rather than the complete one for this. Hopefully it works just as well. Too lazy to reconfigure RepeatMasker inside of the tetools container. Moving on to running the other two assemblies. 

running repeatmasker
```bash
RepeatMasker \
  -e rmblast \
  -pa 6 \
  -lib pcali_ragout-families.fa \
  -dir pcali_ragout \
  -xsmall \
  -gff \
  -u \
  -excln \
  -poly \
  -html \
  assemblies/pcali_ragout.fa
```

running repeatmasker
```bash
RepeatMasker \
  -e rmblast \
  -pa 6 \
  -lib platanus-allee_illumina_step10-families.fa \
  -dir platanus-allee_illumina_step10 \
  -xsmall \
  -gff \
  -u \
  -excln \
  -poly \
  -html \
  assemblies/platanus-allee_illumina_step10.fa
```

huh, interesting errors
```bash

FATAL ERROR: RepeatMasker giving up. One or more
batches failed!  Unfortunately this type of error
cannot be recovered from. Please submit the following
details to the feedback page at the repeatmasker
website:

       http://www.repeatmasker.org

RepeatMasker Version: 4.1.1
Library Version: 
Search Engine: ncbi [ 2.10.0+ ]
Command Line: /opt/RepeatMasker/RepeatMasker-e rmblast -pa 6 -lib platanus-allee_illumina_step10-families.fa -dir platanus-allee_illumina_step10 -xsmall -gff -u -excln -poly -html assemblies/platanus-allee_illumina_step10.fa
Batch Number: 2
Disk Space:
Filesystem      1K-blocks       Used  Available Use% Mounted on
/dev/sda2      6969019544 4590049580 2027680724  70% /work

System Memory:
MemTotal:       379716012 kB
MemFree:        245566984 kB
MemAvailable:   356711740 kB
Cached:         107690040 kB
SwapCached:            0 kB
SwapTotal:       2097148 kB
SwapFree:        2097148 kB
Further details about this problem may be found in
the directory: /work/RM_1110414.ThuDec171707222020
``` 
and 
```bash
nalyzing file assemblies/pcali_ragout.fa
FastaDB::_cleanIndexAndCompact(): Fasta file contains a sequence identifier which is too long ( max id length = 50 )
 at /opt/RepeatMasker/RepeatMasker line 746.
```

so it looks like I should probably clean up the fasta headers a bit. Also, not sure how to interpret the batch failure errors. It might be because I already have one run of repeatmasker going? 

so repeatmasker finished on the redundans assembly. The stats show mostly repeatmodeler families were masked. So I am going to run it with just dfam and see what kind of results I get. If those looks a little off then I will try to figure out how to include the complete dfam database


```bash
RepeatMasker \
  -e rmblast \
  -species Deuterostomia \
  -pa 20 \
  -dir redundans_platanus-allee_step10_ajap-reference-families/ \
  -xsmall \
  -gff \
  -u \
  -excln \
  -poly \
  -html \
  assemblies/redundans_platanus-allee_step10_ajap-reference.fa
```

k, this seems to be working. Although it is taking a lot more time. However, hopefully not having homo sapien as default for masking will improve the masking. Also successfully moved the complete dfam database to the tetools library. So it is now using dfam 33. 

# Sat 19 Dec 2020 10:36:19 AM PST

whelp, that took awhile to finish. running it with the repeatmodeler families now
```bash
RepeatMasker \
  -e rmblast \
  -pa 20 \
  -lib platanus-allee_illumina_step10-families.fa \
  -dir redundans_platanus-allee_step10_ajap-reference-families/ \
  -xsmall \
  -gff \
  -u \
  -excln \
  -poly \
  -html \
  assemblies/redundans_platanus-allee_step10_ajap-reference.fa
```


testing sed command
```bash
echo '>scaffold1_len95936_cov126.743_read142_maxK110' |  sed 's/_[^_]*_[^_]*_[^_]*_[^_]*$//'
```

actual sed command
```bash
sed 's/_[^_]*_[^_]*_[^_]*_[^_]*$//' /home/jon/Working_Files/repeats/TETools/assemblies/platanus-allee_illumina_step10.fa > platanus-allee_illumina_step10.renamed.fa
```

cool, running on the next asssembly. 
```bash
RepeatMasker \
  -e rmblast \
  -pa 20 \
  -lib platanus-allee_illumina_step10-families.fa \
  -dir platanus-allee_illumina_step10.renamed \
  -xsmall \
  -gff \
  -u \
  -excln \
  -poly \
  -html \
  assemblies/platanus-allee_illumina_step10.renamed.fa
```

and done. Cool. Moving on to mapping rna-seq reads to the masked genomes. Not that STAR cares if they are masked or not. 