# Tue 11 Feb 2020 08:59:55 PM PST

## running tests

```bash
./interproscan.sh -i test_proteins.fasta -f tsv
./interproscan.sh -i test_proteins.fasta -f tsv -dp
```

success

## running interproscan on de novo gene predictions from braker2

```
./interproscan.sh -cpu 40 -d /home/jon/Working_Files/interproscan -f TSV,XML,JSON,GFF3 -goterms -i /home/jon/Working_Files/braker2/braker_denovo/augustus.ab_initio.aa -iprlookup -pa -d /home/jon/Working_Files/interproscan
```

# Sat 15 Feb 2020 02:50:02 PM PST

> KEGG-mapper should also be run in order to assign GO terms. apparently it assigns more GO terms and they don't always overlap with interproscan results so it would be good to compare. Also, "String" would be good for looking for protein interactions. Psort for cellular localization? KEGG for metabolic info, but doesn't interproscan already use it?

## A meta-method to predict orthology and paralogy from multiple phylogenetic evidence:

phylomeDB, eggNOG, orthoMCL, Treefam, HODGENOM, ETE

# Fri 21 Feb 2020 10:13:37 AM PST

running interproscan on braker run with rna-seq

```bash
./interproscan.sh -cpu 75 -d /home/jon/Working_Files/interproscan/ -f TSV,XML,JSON,GFF3 -goterms -i /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/rna-mode.augustus.hints.aa -iprlookup -pa 
```
....threw an error about '*' being in the file..... removing them now. used to sublime to find and remove them. Not sure if it will make a difference, but ohwell.* 

databases being used for this run are [CDD-3.17,Coils-2.2.1,Gene3D-4.2.0,Hamap-2019_01,MobiDBLite-2.0,Pfam-32.0,PIRSF-3.02,PRINTS-42.0,ProSitePatterns-2019_01,ProSiteProfiles-2019_01,SFLD-4,SMART-7.1,SUPERFAMILY-1.75,TIGRFAM-15.0

wow, that was fast. 

running it on the pcali rna-seq and ajap protein gene predictions
```bash
./interproscan.sh -cpu 75 -d /home/jon/Working_Files/interproscan/ -f TSV,XML,JSON,GFF3 -goterms -i /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa -iprlookup -pa 
```

annnnnnd complete. wow. pretty darn fast. 

# Sat 23 Jan 2021 10:56:51 PM PST

```bash
wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.48-83.0/interproscan-5.48-83.0-64-bit.tar.gz
wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.48-83.0/interproscan-5.48-83.0-64-bit.tar.gz.md5

# Recommended checksum to confirm the download was successful:
md5sum -c interproscan-5.48-83.0-64-bit.tar.gz.md5
# Must return *interproscan-5.48-83.0-64-bit.tar.gz: OK*
# If not - try downloading the file again as it may be a corrupted copy.

tar -pxvzf interproscan-5.48-83.0-*-bit.tar.gz

cd interproscan-5.48-83.0-.......
python3 initial_setup.py
```

k, it's downloaded. additionally, all the data is also downloaded. time to run it against the predicted proteins. 

version: interproscan-5.48-83.0

```bash
./interproscan.sh \
	-cpu 75 \
	-d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb \
	-f TSV,XML,JSON,GFF3 \
	-goterms \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa \
	-iprlookup \
	--pathways
```

just a friendly reminder to myself that interproscan does not like \* and they need to be stripped from the files prior to use.

# Wed 03 Feb 2021 10:22:51 PM PST

this is probably my issue https://github.com/ebi-pf-team/interproscan/issues/180

# Thu 04 Feb 2021 10:21:03 PM PST

I downloaded the updated version he suggested trying in the issues thing above. I'm going to run it with my braker2 data

```bash
./interproscan.sh \
	-cpu 75 \
	-d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb \
	-f TSV,XML,JSON,GFF3 \
	-goterms \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa \
	-iprlookup \
	--tempdir /home/jon/Working_Files/interproscan/temp_dir \
	--pathways
```
# Fri 05 Feb 2021 11:05:20 AM PST

got some feedback from the github guy

```bash
./interproscan.sh \
	-cpu 75 \
	--disable-precalc \
	-d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb \
	-f TSV,XML,JSON,GFF3 \
	-goterms \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa \
	-iprlookup \
	--tempdir /home/jon/Working_Files/interproscan/temp_dir \
	--pathways
```

# Mon 08 Feb 2021 09:07:20 AM PST

it finished
```bash
./interproscan.sh \
> -cpu 75 \
> --disable-precalc \
> -d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb \
> -f TSV,XML,JSON,GFF3 \
> -goterms \
> -i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa \
> -iprlookup \
> --tempdir /home/jon/Working_Files/interproscan/temp_dir \
> --pathways
05/02/2021 12:04:43:833 Welcome to InterProScan-5.50RC1-83.0
05/02/2021 12:04:43:838 Running InterProScan v5 in STANDALONE mode... on Linux
05/02/2021 12:04:52:830 RunID: jon-PowerEdge-R910_20210205_120452635_g0h6
05/02/2021 12:05:48:920 Loading file /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa
05/02/2021 12:05:48:923 Running the following analyses:
[CDD-3.18,Coils-2.2.1,Gene3D-4.2.0,Hamap-2020_05,MobiDBLite-2.0,PANTHER-15.0,Pfam-33.1,PIRSF-3.10,PRINTS-42.0,ProSitePatterns-2019_11,ProSiteProfiles-2019_11,SFLD-4,SMART-7.1,SUPERFAMILY-1.75,TIGRFAM-15.0]
Pre-calculated match lookup service DISABLED.  Please wait for match calculations to complete...
05/02/2021 12:06:16:523 Uploaded 52151 unique sequences for analysis
05/02/2021 14:22:27:015 25% completed
05/02/2021 15:22:31:480 46% completed
05/02/2021 15:59:55:647 75% completed
05/02/2021 16:05:40:690 90% completed
05/02/2021 17:03:22:041 Info: [52001_52151] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:03:22:042 Info: [52001_52151] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:05:42:234 98% completed
05/02/2021 17:14:11:030 Info: [51001_52000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:11:030 Info: [51001_52000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:17:582 Info: [7001_8000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:17:582 Info: [7001_8000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:24:985 Info: [46001_47000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:24:985 Info: [46001_47000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:24:986 Info: [48001_49000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:24:986 Info: [48001_49000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:25:236 Info: [20001_21000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:25:236 Info: [20001_21000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:28:094 Info: [31001_32000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:28:094 Info: [31001_32000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:28:341 Info: [11001_12000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:28:342 Info: [11001_12000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:34:704 Info: [44001_45000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:34:704 Info: [44001_45000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:35:790 Info: [2001_3000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:35:790 Info: [2001_3000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:41:454 Info: [13001_14000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:41:455 Info: [13001_14000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:406 Info: [1_1000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:406 Info: [1_1000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:408 Info: [42001_43000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:408 Info: [42001_43000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:578 Info: [27001_28000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:578 Info: [27001_28000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:591 Info: [15001_16000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:591 Info: [15001_16000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:754 Info: [33001_34000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:754 Info: [33001_34000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:765 Info: [43001_44000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:46:766 Info: [43001_44000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:47:574 Info: [45001_46000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:47:574 Info: [45001_46000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:48:013 Info: [50001_51000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:48:013 Info: [50001_51000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:48:014 Info: [39001_40000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:48:014 Info: [39001_40000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:417 Info: [22001_23000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:417 Info: [22001_23000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:419 Info: [37001_38000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:419 Info: [37001_38000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:432 Info: [8001_9000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:432 Info: [8001_9000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:846 Info: [26001_27000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:846 Info: [26001_27000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:859 Info: [28001_29000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:52:859 Info: [28001_29000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:129 Info: [6001_7000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:129 Info: [6001_7000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:423 Info: [9001_10000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:424 Info: [9001_10000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:577 Info: [35001_36000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:577 Info: [35001_36000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:592 Info: [49001_50000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:592 Info: [49001_50000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:710 Info: [32001_33000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:710 Info: [32001_33000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:711 Info: [34001_35000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:711 Info: [34001_35000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:853 Info: [24001_25000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:53:854 Info: [24001_25000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:689 Info: [38001_39000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:689 Info: [38001_39000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:776 Info: [18001_19000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:776 Info: [18001_19000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:885 Info: [25001_26000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:885 Info: [25001_26000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:934 Info: [41001_42000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:57:934 Info: [41001_42000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:000 Info: [36001_37000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:000 Info: [36001_37000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:107 Info: [12001_13000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:107 Info: [12001_13000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:325 Info: [17001_18000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:325 Info: [17001_18000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:346 Info: [19001_20000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:346 Info: [19001_20000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:452 Info: [40001_41000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:452 Info: [40001_41000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:485 Info: [47001_48000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:485 Info: [47001_48000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:880 Info: [16001_17000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:880 Info: [16001_17000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:907 Info: [5001_6000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:58:907 Info: [5001_6000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:055 Info: [1001_2000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:055 Info: [1001_2000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:155 Info: [3001_4000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:156 Info: [3001_4000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:261 Info: [29001_30000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:261 Info: [29001_30000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:262 Info: [10001_11000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:262 Info: [10001_11000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:274 Info: [21001_22000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:14:59:274 Info: [21001_22000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:04:846 Info: [14001_15000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:04:847 Info: [14001_15000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:04:948 Info: [30001_31000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:04:949 Info: [30001_31000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:05:674 Info: [4001_5000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:05:674 Info: [4001_5000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:06:080 Info: [23001_24000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:15:06:080 Info: [23001_24000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
05/02/2021 17:49:23:954 100% done:  InterProScan analyses completed 
```

it looks like the pathway stuff takes up a lot of space. I think I'll rerun it without the pathway analysis. 

```bash
./interproscan.sh \
	-cpu 75 \
	--disable-precalc \
	-d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb \
	-f TSV,XML,JSON,GFF3 \
	-goterms \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa \
	-iprlookup \
	--tempdir /home/jon/Working_Files/interproscan/temp_dir 
```


using sublime to find all "polypeptide" matches in the gff3 file shows 43406 matches. I am going to assume that is the number of entries for this. 

throws this error
```bash
./interproscan.sh \
> -cpu 75 \
> --disable-precalc \
> -d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb \
> -f TSV,XML,JSON,GFF3 \
> -goterms \
> -i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa \
> -iprlookup \
> --tempdir /home/jon/Working_Files/interproscan/temp_dir
08/02/2021 09:12:57:011 Welcome to InterProScan-5.50RC1-83.0
08/02/2021 09:12:57:016 Running InterProScan v5 in STANDALONE mode... on Linux
08/02/2021 09:13:05:711 RunID: jon-PowerEdge-R910_20210208_091305491_hmee
08/02/2021 09:14:02:398 Loading file /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisk.aa
08/02/2021 09:14:02:401 Running the following analyses:
[CDD-3.18,Coils-2.2.1,Gene3D-4.2.0,Hamap-2020_05,MobiDBLite-2.0,PANTHER-15.0,Pfam-33.1,PIRSF-3.10,PRINTS-42.0,ProSitePatterns-2019_11,ProSiteProfiles-2019_11,SFLD-4,SMART-7.1,SUPERFAMILY-1.75,TIGRFAM-15.0]
Pre-calculated match lookup service DISABLED.  Please wait for match calculations to complete...
08/02/2021 09:14:27:539 Uploaded 52151 unique sequences for analysis
08/02/2021 11:32:50:493 25% completed
08/02/2021 12:18:50:191 50% completed
08/02/2021 13:06:21:714 75% completed
08/02/2021 13:13:27:619 90% completed
08/02/2021 13:50:09:131 Info: [52001_52151] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 13:50:09:133 Info: [52001_52151] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:00:29:498 Info: [51001_52000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:00:29:500 Info: [51001_52000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:00:29:843 Info: [7001_8000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:00:29:843 Info: [7001_8000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:06:433 Info: [29001_30000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:06:433 Info: [29001_30000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:06:577 Info: [49001_50000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:06:577 Info: [49001_50000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:14:524 Info: [38001_39000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:14:524 Info: [38001_39000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:14:723 Info: [31001_32000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:14:723 Info: [31001_32000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:14:873 Info: [35001_36000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:14:873 Info: [35001_36000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:15:324 Info: [2001_3000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:15:324 Info: [2001_3000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:15:649 Info: [46001_47000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:15:649 Info: [46001_47000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:15:972 Info: [5001_6000] pcounts  tryCounts:1 maxTryCount:1 maxtotalWaitTime: 200
08/02/2021 14:01:15:972 Info: [5001_6000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
2021-02-08 14:01:16,109 [amqEmbeddedWorkerJmsContainer-20] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:213] ERROR - Execution thrown when attempting to executeInTransaction the StepExecution.  All database activity rolled back.
org.springframework.transaction.UnexpectedRollbackException: Transaction silently rolled back because it has been marked as rollback-only
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.processCommit(AbstractPlatformTransactionManager.java:755) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.commit(AbstractPlatformTransactionManager.java:714) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.interceptor.TransactionAspectSupport.commitTransactionAfterReturning(TransactionAspectSupport.java:533) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:304) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:98) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186) ~[spring-aop-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:212) ~[spring-aop-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at com.sun.proxy.$Proxy141.executeInTransaction(Unknown Source) ~[?:?]
	at uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener.onMessage(LocalJobQueueListener.java:197) [interproscan-5.jar:?]
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doInvokeListener(AbstractMessageListenerContainer.java:761) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractMessageListenerContainer.invokeListener(AbstractMessageListenerContainer.java:699) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doExecuteListener(AbstractMessageListenerContainer.java:674) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.doReceiveAndExecute(AbstractPollingMessageListenerContainer.java:318) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.receiveAndExecute(AbstractPollingMessageListenerContainer.java:257) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.invokeListener(DefaultMessageListenerContainer.java:1189) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.executeOngoingLoop(DefaultMessageListenerContainer.java:1179) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.run(DefaultMessageListenerContainer.java:1076) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at java.lang.Thread.run(Thread.java:834) [?:?]
2021-02-08 14:01:16,148 [amqEmbeddedWorkerJmsContainer-20] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:215] ERROR - The exception is :
org.springframework.transaction.UnexpectedRollbackException: Transaction silently rolled back because it has been marked as rollback-only
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.processCommit(AbstractPlatformTransactionManager.java:755)
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.commit(AbstractPlatformTransactionManager.java:714)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.commitTransactionAfterReturning(TransactionAspectSupport.java:533)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:304)
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:98)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:212)
	at com.sun.proxy.$Proxy141.executeInTransaction(Unknown Source)
	at uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener.onMessage(LocalJobQueueListener.java:197)
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doInvokeListener(AbstractMessageListenerContainer.java:761)
	at org.springframework.jms.listener.AbstractMessageListenerContainer.invokeListener(AbstractMessageListenerContainer.java:699)
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doExecuteListener(AbstractMessageListenerContainer.java:674)
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.doReceiveAndExecute(AbstractPollingMessageListenerContainer.java:318)
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.receiveAndExecute(AbstractPollingMessageListenerContainer.java:257)
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.invokeListener(DefaultMessageListenerContainer.java:1189)
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.executeOngoingLoop(DefaultMessageListenerContainer.java:1179)
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.run(DefaultMessageListenerContainer.java:1076)
	at java.base/java.lang.Thread.run(Thread.java:834)
2021-02-08 14:01:16,149 [amqEmbeddedWorkerJmsContainer-20] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:218] ERROR - 2. The exception is :org.springframework.transaction.UnexpectedRollbackException: Transaction silently rolled back because it has been marked as rollback-only
2021-02-08 14:01:16,150 [amqEmbeddedWorkerJmsContainer-20] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:219] ERROR - StepExecution with errors - stepName: stepPrepareForOutput
08/02/2021 14:01:20:377 Info: [34001_35000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:20:377 Info: [34001_35000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:20:550 Info: [13001_14000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:20:551 Info: [13001_14000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:21:130 Info: [48001_49000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:21:130 Info: [48001_49000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:21:292 Info: [16001_17000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:21:292 Info: [16001_17000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:061 Info: [20001_21000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:061 Info: [20001_21000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:389 Info: [40001_41000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:390 Info: [40001_41000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:569 Info: [15001_16000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:570 Info: [15001_16000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:571 Info: [18001_19000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:572 Info: [18001_19000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:27:895 Info: [42001_43000] pcounts  tryCounts:1 maxTryCount:1 maxtotalWaitTime: 200
08/02/2021 14:01:27:896 Info: [42001_43000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
2021-02-08 14:01:28,071 [amqEmbeddedWorkerJmsContainer-31] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:213] ERROR - Execution thrown when attempting to executeInTransaction the StepExecution.  All database activity rolled back.
org.springframework.transaction.UnexpectedRollbackException: Transaction silently rolled back because it has been marked as rollback-only
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.processCommit(AbstractPlatformTransactionManager.java:755) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.commit(AbstractPlatformTransactionManager.java:714) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.interceptor.TransactionAspectSupport.commitTransactionAfterReturning(TransactionAspectSupport.java:533) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:304) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:98) ~[spring-tx-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186) ~[spring-aop-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:212) ~[spring-aop-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at com.sun.proxy.$Proxy141.executeInTransaction(Unknown Source) ~[?:?]
	at uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener.onMessage(LocalJobQueueListener.java:197) [interproscan-5.jar:?]
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doInvokeListener(AbstractMessageListenerContainer.java:761) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractMessageListenerContainer.invokeListener(AbstractMessageListenerContainer.java:699) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doExecuteListener(AbstractMessageListenerContainer.java:674) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.doReceiveAndExecute(AbstractPollingMessageListenerContainer.java:318) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.receiveAndExecute(AbstractPollingMessageListenerContainer.java:257) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.invokeListener(DefaultMessageListenerContainer.java:1189) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.executeOngoingLoop(DefaultMessageListenerContainer.java:1179) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.run(DefaultMessageListenerContainer.java:1076) [spring-jms-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at java.lang.Thread.run(Thread.java:834) [?:?]
2021-02-08 14:01:28,074 [amqEmbeddedWorkerJmsContainer-31] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:215] ERROR - The exception is :
org.springframework.transaction.UnexpectedRollbackException: Transaction silently rolled back because it has been marked as rollback-only
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.processCommit(AbstractPlatformTransactionManager.java:755)
	at org.springframework.transaction.support.AbstractPlatformTransactionManager.commit(AbstractPlatformTransactionManager.java:714)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.commitTransactionAfterReturning(TransactionAspectSupport.java:533)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:304)
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:98)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:212)
	at com.sun.proxy.$Proxy141.executeInTransaction(Unknown Source)
	at uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener.onMessage(LocalJobQueueListener.java:197)
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doInvokeListener(AbstractMessageListenerContainer.java:761)
	at org.springframework.jms.listener.AbstractMessageListenerContainer.invokeListener(AbstractMessageListenerContainer.java:699)
	at org.springframework.jms.listener.AbstractMessageListenerContainer.doExecuteListener(AbstractMessageListenerContainer.java:674)
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.doReceiveAndExecute(AbstractPollingMessageListenerContainer.java:318)
	at org.springframework.jms.listener.AbstractPollingMessageListenerContainer.receiveAndExecute(AbstractPollingMessageListenerContainer.java:257)
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.invokeListener(DefaultMessageListenerContainer.java:1189)
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.executeOngoingLoop(DefaultMessageListenerContainer.java:1179)
	at org.springframework.jms.listener.DefaultMessageListenerContainer$AsyncMessageListenerInvoker.run(DefaultMessageListenerContainer.java:1076)
	at java.base/java.lang.Thread.run(Thread.java:834)
2021-02-08 14:01:28,075 [amqEmbeddedWorkerJmsContainer-31] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:218] ERROR - 2. The exception is :org.springframework.transaction.UnexpectedRollbackException: Transaction silently rolled back because it has been marked as rollback-only
2021-02-08 14:01:28,075 [amqEmbeddedWorkerJmsContainer-31] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:219] ERROR - StepExecution with errors - stepName: stepPrepareForOutput
08/02/2021 14:01:28:277 Info: [1_1000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:28:277 Info: [1_1000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:28:285 Info: [8001_9000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:28:286 Info: [8001_9000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:28:433 Info: [30001_31000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:28:434 Info: [30001_31000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:29:086 Info: [41001_42000] pcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
08/02/2021 14:01:29:087 Info: [41001_42000] mcounts  tryCounts:0 maxTryCount:0 maxtotalWaitTime: 0
2021-02-08 14:01:30,732 [main] [uk.ac.ebi.interpro.scan.jms.activemq.NonZeroExitOnUnrecoverableError:25] FATAL - Analysis step 43 : Prepare proteins for output of the completed analysis for proteins 5001 to 6000 has failed irretrievably.  Available StackTraces follow.
2021-02-08 14:01:30,732 [main] [uk.ac.ebi.interpro.scan.jms.activemq.NonZeroExitOnUnrecoverableError:42] FATAL - The JVM will now exit with a non-zero exit status.
2021-02-08 14:01:30,732 [main] [uk.ac.ebi.interpro.scan.jms.master.StandaloneBlackBoxMaster:341] ERROR - Exception thrown by StandaloneBlackBoxMaster: 
java.lang.IllegalStateException: InterProScan exiting with non-zero status, see logs for further information.
	at uk.ac.ebi.interpro.scan.jms.activemq.NonZeroExitOnUnrecoverableError.failed(NonZeroExitOnUnrecoverableError.java:43) ~[interproscan-5.jar:?]
	at uk.ac.ebi.interpro.scan.jms.master.StandaloneBlackBoxMaster.run(StandaloneBlackBoxMaster.java:166) [interproscan-5.jar:?]
	at uk.ac.ebi.interpro.scan.jms.main.Run.main(Run.java:475) [interproscan-5.jar:?]
java.lang.IllegalStateException: InterProScan exiting with non-zero status, see logs for further information.
	at uk.ac.ebi.interpro.scan.jms.activemq.NonZeroExitOnUnrecoverableError.failed(NonZeroExitOnUnrecoverableError.java:43)
	at uk.ac.ebi.interpro.scan.jms.master.StandaloneBlackBoxMaster.run(StandaloneBlackBoxMaster.java:166)
	at uk.ac.ebi.interpro.scan.jms.main.Run.main(Run.java:475)
2021-02-08 14:01:35,514 [main] [uk.ac.ebi.interpro.scan.jms.master.AbstractMaster:259] WARN - Master process unable to delete temporary directory /home/jon/Working_Files/interproscan/temp_dir/jon-PowerEdge-R910_20210208_091305491_hmee
InterProScan analysis failed. Exception thrown by StandaloneBlackBoxMaster. Check the log file for details

```

# Tue 09 Feb 2021 08:36:24 AM PST

ok, I reran it and it finished without problem. weird.

running interproscan on the next platanus allee gene model set
```bash
./interproscan.sh \
	-cpu 75 \
	--disable-precalc \
	-d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_ajaponicus_rnaseq_and_orthodb \
	-f TSV,XML,JSON,GFF3 \
	-goterms \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_ajaponicus_rnaseq_and_orthodb/braker/augustus.hints.without_asterisks.aa \
	-iprlookup \
	--tempdir /home/jon/Working_Files/interproscan/temp_dir 
```


annnd finished.
searching for polypeptide in teh gff3 shows 47924 matches. so a smidge more. It looks like transposases and some other odds and ends are identified so maybe those should be removed from the interproscan results when comparing the different methods? 


moving on to the next gene model set
```bash
./interproscan.sh \
	-cpu 75 \
	--disable-precalc \
	-d /home/jon/Working_Files/interproscan/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_orthodb_only \
	-f TSV,XML,JSON,GFF3 \
	-goterms \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_orthodb_only/braker/augustus.hints.without_asterisks.aa \
	-iprlookup \
	--tempdir /home/jon/Working_Files/interproscan/temp_dir 
```


# Sun 14 Feb 2021 04:50:14 PM PST

I'm working on trying to understand how I am going to compare the interproscan and eggnog-mapper results and found another example of something that is probably not a protein/gene

this was the only annotation for this gene in interproscan and it is missing from the eggnog-mapper results. So another good example of things to look for
```
MobiDBLite	mobidb-lite	consensus disorder prediction
```


so I think I will rerun interproscan without mobidb-lite and possibly with --dra which disables residue annotations. I think this will not identify domain structure/motifs in the protein? 