# Fri 07 Feb 2020 01:49:25 PM PST

## generating indices for STAR mapping

```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star \
--genomeFastaFiles /home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta 
```

uh, wow, is sucked down 154gb of ram before it crashed. I didn't realize generating genome indicies could eat up so much ram. 

```bash
Feb 07 13:52:51 ..... started STAR run
Feb 07 13:52:51 ... starting to generate Genome files

EXITING because of FATAL PARAMETER ERROR: limitGenomeGenerateRAM=31000000000is too small for your genome
SOLUTION: please specify --limitGenomeGenerateRAM not less than 213523654698 and make that much RAM available 

Feb 07 13:55:02 ...... FATAL ERROR, exiting
```

so new command

```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star \
--genomeFastaFiles /home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta \
--limitGenomeGenerateRAM 223523654698
```

ballz, I forgot about the softmasked version. Will have to run the indexing again, but on the softmasked version. It takes a surprising amount of time to generate an index. 

```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star \
--genomeFastaFiles /home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked \
--limitGenomeGenerateRAM 223523654698
```


## mapping rna-seq data to pcali-plat-redundans.masked genome

```bash
STAR --runThreadN 75 \
--genomeDir /home/jon/Working_Files/star/ \
--readFilesIn /home/jon/Working_Files/parastichopus_californicus_rna-seq/SRR1139198_1.trim.fq,SRR1695477_1.trim.maq15.fq /home/jon/Working_Files/parastichopus_californicus_rna-seq/SRR1139198_2.trim.fq,SRR1695477_2.trim.maq15.fq \
--outSAMtype BAM SortedByCoordinate 
```

note to self - quant mode requires a gtf file of gene predictions

another note to self - ulimit size has to be modified in the systemd files and the limits.conf too I think

mapping rate was about 10million/hr which is way way way too slow. I stopped the run. I think it is all the short reads so I am going to remove the short reads and reindex/map the reads

```bash
bbduk.sh in=/home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked out=pcali_ajap.redundans.platanus.masked.filter-1k.fa minlen=1000
```

results
```bash
0.057 seconds.
Initial:
Memory: max=78953m, total=78953m, free=78869m, used=84m

Warning: Did not find expected fasta file extension for filename /home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked
Input is being processed as unpaired
Started output streams:	0.036 seconds.
Processing time:   		12.599 seconds.

Input:                  	304189 reads 		878550047 bases.
Total Removed:          	226785 reads (74.55%) 	88909575 bases (10.12%)
Result:                 	77404 reads (25.45%) 	789640472 bases (89.88%)

Time:                         	12.637 seconds.
Reads Processed:        304k 	24.07k reads/sec
Bases Processed:        878m 	69.52m bases/sec
```

rerunning the index
```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star \
--genomeFastaFiles /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
--limitGenomeGenerateRAM 223523654698
```

rerunning star with new index
```bash
STAR --runThreadN 75 \
--genomeDir /home/jon/Working_Files/star/ \
--readFilesIn /home/jon/Working_Files/parastichopus_californicus_rna-seq/SRR1139198_1.trim.fq,/home/jon/Working_Files/parastichopus_californicus_rna-seq/SRR1695477_1.trim.maq15.fq /home/jon/Working_Files/parastichopus_californicus_rna-seq/SRR1139198_2.trim.fq,/home/jon/Working_Files/parastichopus_californicus_rna-seq/SRR1695477_2.trim.maq15.fq \
--outSAMtype BAM SortedByCoordinate 
```


annnnnd results!!
```bash
                                 Started job on |	Feb 07 20:25:36
                             Started mapping on |	Feb 07 20:26:02
                                    Finished on |	Feb 07 20:33:22
       Mapping speed, Million of reads per hour |	348.11

                          Number of input reads |	42546497
                      Average input read length |	199
                                    UNIQUE READS:
                   Uniquely mapped reads number |	35299295
                        Uniquely mapped reads % |	82.97%
                          Average mapped length |	196.00
                       Number of splices: Total |	7974712
            Number of splices: Annotated (sjdb) |	0
                       Number of splices: GT/AG |	7837703
                       Number of splices: GC/AG |	23861
                       Number of splices: AT/AC |	4074
               Number of splices: Non-canonical |	109074
                      Mismatch rate per base, % |	1.05%
                         Deletion rate per base |	0.07%
                        Deletion average length |	2.37
                        Insertion rate per base |	0.07%
                       Insertion average length |	1.48
                             MULTI-MAPPING READS:
        Number of reads mapped to multiple loci |	3612519
             % of reads mapped to multiple loci |	8.49%
        Number of reads mapped to too many loci |	31594
             % of reads mapped to too many loci |	0.07%
                                  UNMAPPED READS:
  Number of reads unmapped: too many mismatches |	0
       % of reads unmapped: too many mismatches |	0.00%
            Number of reads unmapped: too short |	3485525
                 % of reads unmapped: too short |	8.19%
                Number of reads unmapped: other |	117564
                     % of reads unmapped: other |	0.28%
                                  CHIMERIC READS:
                       Number of chimeric reads |	0
                            % of chimeric reads |	0.00%
```


# Sat 19 Dec 2020 10:50:03 PM PST

so things I have learned since mapping reads to genome assemblies. First is that star has a large intron default size which needs to be reduced or there will be a load of false hits.

Also, it appears there is a bit of debate regarding read-trimming. I also noticed in one of the rna-seq datasets there are some adapters and also just low quality reads. But I am going to map and see what happens. Something about mapping tools using soft-clipping to deal with adapters. 


rerunning the index
```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star/redundans_platanus-allee_step10_ajap-reference.masked_dfam/ \
--genomeFastaFiles /home/jon/Working_Files/star/assemblies/redundans_platanus-allee_step10_ajap-reference.masked_dfam.fa \
--limitGenomeGenerateRAM 243354217397 \
--genomeSAindexNbases 13
```

running star with index
```bash
STAR --runThreadN 75 \
  --alignIntronMax 5000 \
  --genomeDir /home/jon/Working_Files/star/redundans_platanus-allee_step10_ajap-reference.masked_dfam \
  --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_1.fq,/home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_2.fq  \
  --outSAMtype BAM SortedByCoordinate 
```

ok, so it appears that the ulimit open files is still an issue in ubuntu 20.04. However, they have improved how to edit it. It seems that I can change the ulimit per a bash session. But only once per a bash session. Trying with ulimit -n 10000.

It appears to be working

finished - mapped rate was 42.25%. yeesh. So trimming will probably be needed. 

mapping next rna-seq data set

```bash
STAR --runThreadN 75 \
  --alignIntronMax 5000 \
  --genomeDir /home/jon/Working_Files/star/redundans_platanus-allee_step10_ajap-reference.masked_dfam \
  --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1139198_1.fq,/home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1139198_2.fq  \
  --outSAMtype BAM SortedByCoordinate 
```

cool, it finished. So I think I don't need to map them to the second masked redundans assembly as there is nothing actually different between them. So I will just map to the platanus-allee assembly and call it good. 

I am also just going to use the SRR1695477 bam even though only half mapped as I think it is probably. Maybe if gene modeling goes badly and I want to try and improve I might come back and do this again. 

indexing it

```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/ \
--genomeFastaFiles /home/jon/Working_Files/star/assemblies/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam.fa \
--limitGenomeGenerateRAM 468743531445 \
--genomeSAindexNbases 13
```

/home/jon/Working_Files/star/assemblies/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam.fa

# Sun 20 Dec 2020 11:20:45 AM PST

```bash
STAR --runThreadN 75 \
  --alignIntronMax 5000 \
  --genomeDir /home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam \
  --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1139198_1.fq,/home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1139198_2.fq  \
  --outSAMtype BAM SortedByCoordinate 
```

```bash
STAR --runThreadN 75 \
  --alignIntronMax 5000 \
  --genomeDir /home/jon/Working_Files/star/redundans_platanus-allee_step10_ajap-reference.masked_dfam \
  --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_1.fq,/home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_2.fq  \
  --outSAMtype BAM SortedByCoordinate 
```

so I think that star uses more memory for fragmented genomes. when running mapping on pear, it didn't top 10G but on this machine it is using 200G

sweet, it finished. running  the next rna-seq data set. 


creating index for platanus-allee assembly
```bash
STAR --runThreadN 75 \
> --runMode genomeGenerate \
> --genomeDir /home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/ \
> --genomeFastaFiles /home/jon/Working_Files/star/assemblies/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam.fa \
> --limitGenomeGenerateRAM 468743531445 \
> --genomeSAindexNbases 13
Dec 20 01:49:10 ..... started STAR run
Dec 20 01:49:10 ... starting to generate Genome files
Dec 20 02:08:33 ... starting to sort Suffix Array. This may take a long time...
Dec 20 02:21:08 ... sorting Suffix Array chunks and saving them to disk...
Dec 20 02:49:52 ... loading chunks from disk, packing SA...
Dec 20 02:55:39 ... finished generating suffix array
Dec 20 02:55:39 ... generating Suffix Array index
Dec 20 02:57:59 ... completed Suffix Array index
Dec 20 02:57:59 ... writing Genome to disk ...
Dec 20 03:03:00 ... writing Suffix Array to disk ...
Dec 20 03:03:12 ... writing SAindex to disk
Dec 20 03:03:14 ..... finished successfully
```

platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam with SRR1139198
```bash
STAR --runThreadN 75 \
  --alignIntronMax 5000 \
  --genomeDir /home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam  \
  --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1139198_1.fq,/home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1139198_2.fq   \
  --outSAMtype BAM SortedByCoordinate 
Dec 20 11:12:24 ..... started STAR run
Dec 20 11:12:24 ..... loading genome
Dec 20 11:15:19 ..... started mapping
Dec 20 12:07:18 ..... finished mapping
Dec 20 12:07:31 ..... started sorting BAM
Dec 20 12:09:30 ..... finished successfully
```

platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam with SRR1695477
```bash
STAR --runThreadN 75 \
>   --alignIntronMax 5000 \
>   --genomeDir /home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam \
>   --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_1.fq,/home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_2.fq  \
>   --outSAMtype BAM SortedByCoordinate 
Dec 20 13:25:34 ..... started STAR run
Dec 20 13:25:35 ..... loading genome
Dec 20 13:29:33 ..... started mapping
Dec 20 14:18:19 ..... finished mapping
Dec 20 14:18:33 ..... started sorting BAM
Dec 20 14:19:22 ..... finished successfully
```

reran redundans_platanus-allee_step10_ajap-reference.masked_dfam with SRR1695477 because I overwrote it I think
```bash
STAR --runThreadN 75 \
>   --alignIntronMax 5000 \
>   --genomeDir /home/jon/Working_Files/star/redundans_platanus-allee_step10_ajap-reference.masked_dfam \
>   --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_1.fq,/home/jon/Working_Files/sea_cuke_species_data/parastichopus_californicus_rna-seq/original_rna-seq_data/SRR1695477_2.fq  \
>   --outSAMtype BAM SortedByCoordinate 
Dec 20 12:25:46 ..... started STAR run
Dec 20 12:25:46 ..... loading genome
Dec 20 12:29:14 ..... started mapping
Dec 20 12:39:57 ..... finished mapping
Dec 20 12:40:05 ..... started sorting BAM
Dec 20 12:40:54 ..... finished successfully
```

cool, k I think I am ready to move on to braker2 gene modeling

# Tue 05 Jan 2021 11:04:07 PM PST

downloading apostichopus japonicus rna-seq data from ncbi. I don't think the californicus data is enough. 

```bash
prefetch -p -O /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data --option-file /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data/apostichopus_japonicus_rna-seq_accession_list
```

# Mon 11 Jan 2021 04:12:33 PM PST

so the braker runs were not great. I think it might have to do with the rna-seq datasets. So I am going to try mapping japonicus rna-seq data to the genome assembly. 

There are a lot of sra files. I think I will extract and then merge into one giant fastq for each bioproject. 

```bash
for file in /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data/PRJNA413998_genome_sequencing_and_assembly/*/*.sra
do
  fasterq-dump $file -e 70
done
```

well, that worked. Now to combine all those into two fastq files
```bash
cat *_1.fastq >> PRJNA413998_1.fastq
```

```bash
cat *_2.fastq > PRJNA413998_2.fastq
```

running the fastq files through fastqc to check quality before aligning with star. May take awhile. 


# Tue 12 Jan 2021 09:58:33 AM PST

looks pretty good. 
Now to see if I can put it through star. yikes. 


making genome index because I deleted it earlier
```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/ \
--genomeFastaFiles /home/jon/Working_Files/star/assemblies/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam.fa \
--limitGenomeGenerateRAM 468743531445 \
--genomeSAindexNbases 13
```


```bash
STAR --runThreadN 75 \
  --alignIntronMax 5000 \
  --genomeDir /home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam  \
  --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data/PRJNA413998_genome_sequencing_and_assembly/PRJNA413998_1.fastq,/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data/PRJNA413998_genome_sequencing_and_assembly/PRJNA413998_2.fastq   \
  --outSAMtype BAM SortedByCoordinate 
```

good ol' ulimit error probably.
setting ulimit to 15k
```bash
ulimit -n 15000
```

/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data/PRJNA413998_genome_sequencing_and_assembly/PRJNA413998_1.fastq

# Thu 14 Jan 2021 06:04:49 PM PST

wow.... it finished O.O 
```bash
Jan 12 17:11:46 ..... started STAR run
Jan 12 17:11:46 ..... loading genome
Jan 12 17:14:39 ..... started mapping
Jan 14 11:06:26 ..... finished mapping
Jan 14 11:06:44 ..... started sorting BAM
Jan 14 12:23:13 ..... finished successfully
```

not too bad
```bash
Started job on |  Jan 12 17:11:46
                             Started mapping on | Jan 12 17:14:39
                                    Finished on | Jan 14 12:23:13
       Mapping speed, Million of reads per hour | 60.59

                          Number of input reads | 2614123638
                      Average input read length | 110
                                    UNIQUE READS:
                   Uniquely mapped reads number | 1887370841
                        Uniquely mapped reads % | 72.20%
                          Average mapped length | 107.50
                       Number of splices: Total | 354423636
            Number of splices: Annotated (sjdb) | 0
                       Number of splices: GT/AG | 350399980
                       Number of splices: GC/AG | 607194
                       Number of splices: AT/AC | 147971
               Number of splices: Non-canonical | 3268491
                      Mismatch rate per base, % | 3.64%
                         Deletion rate per base | 0.19%
                        Deletion average length | 2.26
                        Insertion rate per base | 0.13%
                       Insertion average length | 1.83
                             MULTI-MAPPING READS:
        Number of reads mapped to multiple loci | 178952061
             % of reads mapped to multiple loci | 6.85%
        Number of reads mapped to too many loci | 4518585
             % of reads mapped to too many loci | 0.17%
                                  UNMAPPED READS:
  Number of reads unmapped: too many mismatches | 0
       % of reads unmapped: too many mismatches | 0.00%
            Number of reads unmapped: too short | 542853646
                 % of reads unmapped: too short | 20.77%
                Number of reads unmapped: other | 428505
                     % of reads unmapped: other | 0.02%
                                  CHIMERIC READS:
                       Number of chimeric reads | 0
                            % of chimeric reads | 0.00%
```

whelp, time to run it through braker2. 