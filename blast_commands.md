### blasting p-cali genome for genes


make blast db 
```
makeblastdb -in ../Patanus-Allee/assembly/out_consensusScaffold.fa -title "p_cali_genome" -dbtype nucl
```

blast ajap survivin protein sequence against the pcali genome assembly database
```
tblastn -db out_consensusScaffold.fa -query survivin_ajap.aa.fa -num_threads 60 -out survivin.blast.txt -evalue 1e-5 -word_size 3
```

blast ajap mortalin protein sequence against the pcali genome assembly database
```
tblastn -db out_consensusScaffold.fa -query mortalin.ajap.aa.fa -num_threads 60 -out mortalin.blast.txt -evalue 1e-5 -word_size 3
```

blast ajap telomerase protein sequence against the pcali genome assembly database
```
tblastn -db out_consensusScaffold.fa -query telomerase_ajap.aa.fa -num_threads 60 -out telomerase.blast.txt -evalue 1e-5 -word_size 3
```

# Thu 13 Feb 2020 02:23:45 PM PST

command to extract all the files
```
find -name "*tar.gz" -exec tar xvzf '{}' \;
```

command to blast the predicted proteins against nr.
```
diamond blastp -d /home/jon/blast_db_nr/nr -q /home/jon/Working_Files/braker2/braker_denovo/augustus.ab_initio.aa -o /home/jon/Working_Files/blast
```
## blastp

ehhh, changed my mind, am gonna try using blastp. 
```
blastp -query /home/jon/Working_Files/braker2/braker_denovo/augustus.ab_initio.aa -db /home/jon/blast_db_nr/nr -out pcali_blastp_nr.txt -evalue 1e-30 -num_alignments 1 -num_threads 75
```

# Fri 14 Feb 2020 06:48:27 PM PST

after running it over night and then checking how many predicted proteins had been aligned, I realized I need to use diamond if I want it to finish anytime soon. 

```
grep ".t1" /home/jon/blast_db_nr/pcali_blastp_nr.txt | wc -l
```

output: 1908
out of 47,000 proteins.....

> note: when blasting against the refseq nr database, most of the hits are (not suprisingly) for apostichopus japonicus. However, the names are mostly "hypothetical protein" or "putative protein" without actually saying what the protein is most similar to. So when blasting against the database, I should include the top five alignment hits so that close proteins from other organisms show up and I can pull the names from there rather than japonicus. Also should look into why they did that. I am guessing they should a similarity cut off or the like and didn't pull names if the proteins were not close enough.

>further note to self, these are the PSP-94 genes identified in pcali so far: g29538.t1
, g34543.t1, g46895.t1

>rhodpsin motif containing proteins: g17012.t1, g31017.t1, g18609.t1, g11368.t1, g45821.t1, g20809.t1 and lots more

# Sun 16 Feb 2020 08:13:58 PM PST
blast for mortalin against the latest assembly


```

makeblastdb -in /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa -title "pcali_ajap.redundans.platanus.masked.filter-1k" -dbtype nucl


tblastn -db /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa -query /home/jon/Working_Files/blast/mortalin.ajap.aa.fa -num_threads 60 -out mortalin.blast.pcali.plat.redun.1k-fil.txt -evalue 1e-5 -word_size 3
```

output shows 12 fragments. japonicus has 13 or 14 I think. interesting as this was assembled using japonicus as a reference. 



# Tue 18 Feb 2020 10:55:15 AM PST

command to blast the predicted proteins against nr.
```
# making diamond blast database
diamond makedb --in nr.gz -d nr

diamond blastp -f 100 -b25.0 -c 1 --threads 75 -d /home/jon/Working_Files/nr.dmnd -q /home/jon/Working_Files/braker2/braker_denovo_pcali_ajap_redun_plat/augustus.ab_initio.aa -o /home/jon/Working_Files/blast
```

huh, apparently makedb works on gziped fasta files

that was fast...

```
diamond view -f 6 -a blast.daa > blast.tbl
```

cool, so now to do it for the gene predictions made using rna-seq data

```
diamond blastp -f 100 -b25.0 -c 1 --threads 75 -d /home/jon/Working_Files/nr.dmnd -q /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/augustus.hints.aa -o /home/jon/Working_Files/blast

diamond view -f 6 -a blast.daa > blast.tbl
```

results
```
Total time = 3190.51s
Reported 736094 pairwise alignments, 740003 HSPs.
37924 queries aligned.
```

# Fri 21 Feb 2020 05:50:04 PM PST

blasting the pcali rna-seq ajap protein gene predictions
```
diamond blastp -f 100 -b25.0 -c 1 --threads 75 -d /home/jon/Working_Files/nr.dmnd -q /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa -o /home/jon/Working_Files/blast

diamond view -f 6 -a blast.daa > pcali-rna.ajap-prot.tbl
```

# Tue 25 Feb 2020 12:22:14 AM PST

```
diamond blastx \
        --query /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
        --db /home/jon/Working_Files/nr.dmnd \
        --outfmt 100 \
        --sensitive \
        --max-target-seqs 1 \
        --evalue 1e-25 \
        --threads 70 \
        > pcali_ajap.redundans.platanus.masked.filter-1k.diamond-nr.out
```

# Tue 25 Feb 2020 11:36:56 AM PST

niiiice, it finished. 
```
Total time = 37629.8s
Reported 15803 pairwise alignments, 20214 HSPs.
15803 queries aligned.
```
buuuut, nothing was written to the file. weird. Running again, but without the "write to" commands

```
diamond blastx \
        --query /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
        --db /home/jon/Working_Files/nr.dmnd \
        --outfmt 100 \
        --out pcali_ajap.redundans.platanus.masked.filter-1k.diamond-nr.daa \
        --sensitive \
        --max-target-seqs 1 \
        --evalue 1e-25 \
        --threads 70 
        ```

results
```
Total time = 37348.9s
Reported 15803 pairwise alignments, 20214 HSPs.
15803 queries aligned.
```

excellent, it saved to a diamond archive.

So the e-value for the blastx run is actually pretty low. I am thinking I will run it with an evalue more like 1e-10 and also increase the number of max-target-seqs to increase the likelihood of getting more than the "hypothetical apostichopus japonicus gene"

```
diamond blastx \
        --query /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
        --db /home/jon/Working_Files/nr.dmnd \
        --outfmt 100 \
        --out pcali_ajap.redundans.platanus.masked.filter-1k.eval-10.diamond-nr.daa \
        --sensitive \
        --max-target-seqs 5 \
        --evalue 1e-10 \
        --threads 70 
```

that's better...
```
Total time = 37671.8s
Reported 111084 pairwise alignments, 158439 HSPs.
32795 queries aligned.
```