### using minimap2 to map reads to genome assembly

```
./minimap2-2.17_x64-linux/minimap2 -t 70 -a out_consensusScaffold.fa ../P_cali_g_1.fq ../P_cali_g_2.fq -o pcali.sam
```

# Wed 06 May 2020 11:53:59 AM PDT

downloading the most recent version
```bash
curl -L https://github.com/lh3/minimap2/releases/download/v2.17/minimap2-2.17_x64-linux.tar.bz2 | tar -jxvf -
./minimap2-2.17_x64-linux/minimap2
```

going to map the raw reads to the three assemblies
```bash

./minimap2 -t 70 -a /home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq > pcali_redundans.sam

./minimap2 -t 70 -a /home/jon/Working_Files/pcali_genome.masurca.fasta /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq > pcali_masurca.sam

./minimap2 -t 70 -a /home/jon/Working_Files/pcali_genome.platanus.fa /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq > pcali_plat-allee.sam

./minimap2 -t 70 -a /home/jon/Working_Files/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.fna /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq > ajap.sam

./minimap2 -t 70 -a /home/jon/Working_Files/parastichopus_parvimensis/GCA_000934455.1_Ppar_1.0_genomic.fna /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq > pparv.sam

```


yikes, huge same file. Needs to be compressed to be bam using samtools
```bash
./minimap2 -t 70 -a /home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq | samtools view -S -b - > pcali_redundans.bam

./minimap2 -t 70 -a /home/jon/Working_Files/pcali_genome.masurca.fasta /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq | samtools view -S -b - > pcali_masurca.bam

./minimap2 -t 70 -a /home/jon/Working_Files/pcali_genome.platanus.fa /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq | samtools view -S -b - > pcali_plat-allee.bam

./minimap2 -t 70 -a /home/jon/Working_Files/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.fna /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq | samtools view -S -b - > ajap.bam

./minimap2 -t 70 -a /home/jon/Working_Files/parastichopus_parvimensis/GCA_000934455.1_Ppar_1.0_genomic.fna /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq | samtools view -S -b - > pparv.bam

```

# Thu 20 Aug 2020 10:14:27 AM PDT
deleted most of the above output as it was taking up too much room. May revisit this again

# Sun 22 Nov 2020 11:28:13 PM PST

probably going to use minimap2 to create alignments for a lot of assemblies. In order to keep file size down, I will try the suggested approach below

example
```bash
minimap2 -ax sr ../contigs.longer500.fasta ../../4PF-0-10_1_R1.fq ../../4PF-0-10_1_R2.fq | samtools sort -o 4PF_mapping_aln.bam
```


creating a loop over all the assemblies
```bash
for file in *.fa
do
	echo "{file}"

	minimap2 \
	-x sr \
	-t 70 \
	-a ${file} \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_1.fq.gz \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_2.fq.gz \
	| samtools sort -o "${file}"_aln.bam
done
```


saving script to minimap2.sh
```bash
for file in *.bam
do
	samtools stats -@70 $file > $file.stats
done
```


# Sat 05 Dec 2020 11:07:12 AM PST

getting avg coverage info
```bash
for file in *.bam
do
	echo $file 
	echo "calculating avg coverage"
	samtools depth -a $file | awk '{c++;s+=$3}END{print s/c}'
	echo "calculating breadth of coverage"
	samtools depth -a $file | awk '{c++; if($3>0) total+=1}END{print (total/c)*100}' 
done
```

I think the breadth of coverage is how much of the genome is covered by at least one read. So it had better be close to 100%. But I think the next question is how much of the genome is covered by at least 60X or something like that. 

I think I could have also used bedtools for this and it may have been faster. 

this is taken from https://sarahpenir.github.io/bioinformatics/awk/calculating-mapping-stats-from-a-bam-file-using-samtools-and-awk/


TO-DO
```bash
for file in *.bam
do
	samtools depth -a $file | awk '{c++; if($3>60) total+=1}END{print (total/c)*100}' 
done
```

# Tue 08 Dec 2020 04:17:06 PM PST

results from the first round. I should have formatted it differently. lol
```bash
assembly_name																			avg_cov		cov_breadth	cov@60X
masurca_illumina_no-reference_yes-link.fa_aln.bam 										111.469		99.952		78.9231
masurca_illumina_yes-reference_yes-link.fa_aln.bam 										124.364		99.9431		86.3322
masurca_nanopore_illumina_no-reference_no-link.fa_aln.bam 								323.253		99.9977		97.4017
masurca_nanopore_illumina_yes-reference_yes-link.fa_aln.bam 							124.142		99.9432		86.3771
pcali_ragout.fa_aln.bam 																104.333		95.4021		76.5046
platanus-allee_illumina_nanopore_step20.fa_aln.bam 										108.823		99.3322		79.792
platanus-allee_illumina_step10.fa_aln.bam 												108.906		99.325		79.8471
platanus-allee_illumina_step20.fa_aln.bam 												108.647		99.3189		79.664
redundans_marsurca-without-reference_illumina_ajap-reference.fa_aln.bam 				106.119		53.9819		52.0836
redundans_platanus-allee_step10_ajap-reference.fa_aln.bam 								95.1615		77.0319		58.0622
redundans_platanus-allee_step20_ajap-reference_and_nanopore.fa_aln.bam 					92.7877		60.4011		53.5232
redundans_platanus-allee-with-nanopore_step20_ajap-reference_no-nanopore.fa_aln.bam 	96.0181		60.4341		54.9773
```


# Thu 10 Dec 2020 08:01:34 PM PST

soooo, just realized I might be able to improve the genome assemblies using racon and minimap2. 

mapping nanopore reads to genome assemby
```bash
minimap2 \
	-t 70 \
	-ax map-ont \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/pcali_ragout.fa \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq > ont2ragout_aln.sam
```

that was fast
```bash
[M::mm_idx_gen::29.683*1.72] collected minimizers
[M::mm_idx_gen::35.203*3.49] sorted minimizers
[M::main::35.346*3.48] loaded/built the index for 679270 target sequence(s)
[M::mm_mapopt_update::36.879*3.38] mid_occ = 378
[M::mm_idx_stat] kmer size: 15; skip: 10; is_hpc: 0; #seq: 679270
[M::mm_idx_stat::37.514*3.34] distinct minimizers: 50654474 (53.96% are singletons); average occurrences: 3.068; average spacing: 5.613
[M::worker_pipeline::40.274*4.75] mapped 21169 sequences
[M::main] Version: 2.17-r941
[M::main] CMD: minimap2 -t 70 -ax map-ont /home/jon/Working_Files/genome_assemblies/californicus_genomes/pcali_ragout.fa /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq
[M::main] Real time: 40.614 sec; CPU: 191.629 sec; Peak RSS: 7.616 GB
```
sigh, it's in sam format and not bam. 
