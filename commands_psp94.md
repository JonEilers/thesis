# Fri 28 Aug 2020 12:21:17 AM PDT

command to extract region of a scaffold containing the psp94 gene cluster in parastichopus californicus
```bash
samtools faidx /home/jon/Working_Files/genome_assemblies/old_pcali_ajap.redundans.platanus.filter-1k.fasta scaffold00549:210934-374934 -o psp94_region_pcali.fa
```

kk, so sticking the above region into ciider produced a lot of binding sites. Not very useful. So I am extracting the  10k bp up stream from each of the psp94 genes. But in order to do that I need to fix the braker2 gff3 file using gff3tools and then load the gff3 into jbrowse so I can snag the coordinates and other needed info for inputing into bedtools. 

the bedtools file will look like

```bash
scaffold00549	252768	255132	jg7342.t1	3	+
scaffold00549	268088	270465	jg7343.t1	3	+
scaffold00549	281638	285231	jg7344.t1	3	+
scaffold00549	291126	293671	jg7345.t1	3	+
scaffold00549	297717	301169	jg7346.t1	3	+
scaffold00549	305328	308601	jg7347.t1	3	+
scaffold00549	313837	318582	jg7348.t1	4	+
scaffold00549	320860	332139	jg7349.t1	7	+
scaffold00549	336364	341065	jg7350.t1	3	-
scaffold00549	344500	348579	jg7351.t1	3	+
scaffold00549	352967	356775	jg7352.t1	3	+
scaffold00549	359203	363702	jg7353.t1	4	+
scaffold00549	388841	392710	jg7354.t1	3 	+
```


bash bedtools command
```bash

#generating genome file for bedtools
cut -f 1,2 /home/jon/Working_Files/genome_assemblies/old_pcali_ajap.redundans.platanus.masked.filter-1k.fa.fai > chrom.sizes

bedtools flank \
	-i /home/jon/Working_Files/psp94/pcali_psp94.bed \
	-g /home/jon/Working_Files/psp94/chrom.sizes \
	-l 2000 \
	-r 0 \
	-s > pcali_psp94_genes.2kb.promoters.bed

bedtools getfasta \
	-fi /home/jon/Working_Files/genome_assemblies/old_pcali_ajap.redundans.platanus.masked.filter-1k.fa \
	-bed /home/jon/Working_Files/psp94/pcali_psp94_genes.2kb.promoters.bed \
	-fo pcali_psp94_genes.2kb.promoters.bed.fa
```

