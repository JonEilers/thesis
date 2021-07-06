## Busco analysis of genome assemblies

### Busco run on Platanus-allee genome assembly

```
#eukaryote buscos
busco -c 5 -m genome -i ../out_consensusScaffold.fa -o busco_out_consensusScaffold.fa -l eukaryota_odb10

#metazoa buscos
busco -c 5 -m genome -i ../out_consensusScaffold.fa -o metazoa_busco_out_consensusScaffold.fa -l metazoa_odb10
```

results from Eukaryote dataset
```
# BUSCO version is: 4.0.2 
# The lineage dataset is: eukaryota_odb10 (Creation date: 2019-11-20, number of species: 70, number of BUSCOs: 255)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/out_consensusScaffold.fa
# BUSCO was run in mode: genome

	***** Results: *****

	C:78.4%[S:75.7%,D:2.7%],F:16.1%,M:5.5%,n:255	   
	200	Complete BUSCOs (C)			   
	193	Complete and single-copy BUSCOs (S)	   
	7	Complete and duplicated BUSCOs (D)	   
	41	Fragmented BUSCOs (F)			   
	14	Missing BUSCOs (M)			   
	255	Total BUSCO groups searched
```

results from the metazoa dataset
```
# The lineage dataset is: metazoa_odb10 (Creation date: 2019-11-20, number of species: 65, number of BUSCOs: 954)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/out_consensusScaffold.fa
# BUSCO was run in mode: genome

	***** Results: *****

	C:82.5%[S:78.6%,D:3.9%],F:12.3%,M:5.2%,n:954	   
	787	Complete BUSCOs (C)			   
	750	Complete and single-copy BUSCOs (S)	   
	37	Complete and duplicated BUSCOs (D)	   
	117	Fragmented BUSCOs (F)			   
	50	Missing BUSCOs (M)			   
	954	Total BUSCO groups searched
```

### Busco run on Masurca genome assembly

``` 
busco -c 40 -m genome -i ../masurca_p_cali.fasta -o euk_busco_masurca_pcali -l eukaryota_odb10
busco -c 40 -m genome -i ../masurca_p_cali.fasta -o metazoa_busco_masurca_pcali -l metazoa_odb10
```

results

```
# BUSCO version is: 4.0.2 
# The lineage dataset is: eukaryota_odb10 (Creation date: 2019-11-20, number of species: 70, number of BUSCOs: 255)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/masurca_p_cali.fasta
# BUSCO was run in mode: genome

	***** Results: *****

	C:69.8%[S:54.1%,D:15.7%],F:20.0%,M:10.2%,n:255	   
	178	Complete BUSCOs (C)			   
	138	Complete and single-copy BUSCOs (S)	   
	40	Complete and duplicated BUSCOs (D)	   
	51	Fragmented BUSCOs (F)			   
	26	Missing BUSCOs (M)			   
	255	Total BUSCO groups searched
```

results for metazoa
```
# The lineage dataset is: metazoa_odb10 (Creation date: 2019-11-20, number of species: 65, number of BUSCOs: 954)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/masurca_p_cali.fasta
# BUSCO was run in mode: genome

	***** Results: *****

	C:76.4%[S:61.6%,D:14.8%],F:13.6%,M:10.0%,n:954	   
	729	Complete BUSCOs (C)			   
	588	Complete and single-copy BUSCOs (S)	   
	141	Complete and duplicated BUSCOs (D)	   
	130	Fragmented BUSCOs (F)			   
	95	Missing BUSCOs (M)			   
	954	Total BUSCO groups searched
```

# Mon 03 Feb 2020 08:15:52 PM PST

### running busco analysis on the redundans assembly

```
busco -c 30 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o euk_busco_pcali_ajap.redundans.platanus.fa -l eukaryota_odb10
busco -c 30 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o metazoa_busco_pcali_ajap.redundans.platanus.fa -l metazoa_odb10

```

results for eukaryote
```
	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:78.4%[S:73.3%,D:5.1%],F:12.5%,M:9.1%,n:255     |
	|200	Complete BUSCOs (C)                       |
	|187	Complete and single-copy BUSCOs (S)       |
	|13	Complete and duplicated BUSCOs (D)        |
	|32	Fragmented BUSCOs (F)                     |
	|23	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------
```

results for metazoa
```
	--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:84.2%[S:75.6%,D:8.6%],F:9.2%,M:6.6%,n:954      |
	|803	Complete BUSCOs (C)                       |
	|721	Complete and single-copy BUSCOs (S)       |
	|82	Complete and duplicated BUSCOs (D)        |
	|88	Fragmented BUSCOs (F)                     |
	|63	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------
```

# Mon 02 Mar 2020 09:49:32 PM PST

running busco on the gene predictions sets

## braker2 ab initio gene predictions
```
busco -c 75 -m protein -i /home/jon/Working_Files/braker2/braker_denovo_pcali_ajap_redun_plat/augustus.ab_initio.aa -o euk_busco_braker_denovo_pcali_ajap_redun_plat -l eukaryota_odb10
busco -c 75 -m protein -i /home/jon/Working_Files/braker2/braker_denovo_pcali_ajap_redun_plat/augustus.ab_initio.aa -o metazoa_busco_braker_denovo_pcali_ajap_redun_plat -l metazoa_odb10


	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:77.7%[S:71.8%,D:5.9%],F:18.8%,M:3.5%,n:255     |
	|198	Complete BUSCOs (C)                       |
	|183	Complete and single-copy BUSCOs (S)       |
	|15	Complete and duplicated BUSCOs (D)        |
	|48	Fragmented BUSCOs (F)                     |
	|9	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------

	--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:80.4%[S:71.3%,D:9.1%],F:11.4%,M:8.2%,n:954     |
	|767	Complete BUSCOs (C)                       |
	|680	Complete and single-copy BUSCOs (S)       |
	|87	Complete and duplicated BUSCOs (D)        |
	|109	Fragmented BUSCOs (F)                     |
	|78	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------

```

## braker2 rna-seq gene predictions
```
busco -c 75 -m protein -i /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/rna-mode.augustus.hints.aa -o euk_busco_braker_rna_pcali_platredun -l eukaryota_odb10

busco -c 75 -m protein -i /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/rna-mode.augustus.hints.aa -o metazoa_busco_braker_rna_pcali_platredun -l metazoa_odb10


	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:80.0%[S:63.9%,D:16.1%],F:16.5%,M:3.5%,n:255    |
	|204	Complete BUSCOs (C)                       |
	|163	Complete and single-copy BUSCOs (S)       |
	|41	Complete and duplicated BUSCOs (D)        |
	|42	Fragmented BUSCOs (F)                     |
	|9	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------


	--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:85.6%[S:65.8%,D:19.8%],F:9.7%,M:4.7%,n:954     |
	|817	Complete BUSCOs (C)                       |
	|628	Complete and single-copy BUSCOs (S)       |
	|189	Complete and duplicated BUSCOs (D)        |
	|93	Fragmented BUSCOs (F)                     |
	|44	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------

```

## braker2 rna-seq + a. jap protein gene predictions

```
busco -c 75 -m protein -i /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa -o euk_busco_braker_pcali-rna_ajap-prot -l eukaryota_odb10

busco -c 75 -m protein -i /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa -o metazoa_busco_braker_pcali-rna_ajap-prot -l metazoa_odb10


	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:80.7%[S:67.8%,D:12.9%],F:16.5%,M:2.8%,n:255    |
	|206	Complete BUSCOs (C)                       |
	|173	Complete and single-copy BUSCOs (S)       |
	|33	Complete and duplicated BUSCOs (D)        |
	|42	Fragmented BUSCOs (F)                     |
	|7	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------

	--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:85.5%[S:69.6%,D:15.9%],F:9.9%,M:4.6%,n:954     |
	|816	Complete BUSCOs (C)                       |
	|664	Complete and single-copy BUSCOs (S)       |
	|152	Complete and duplicated BUSCOs (D)        |
	|94	Fragmented BUSCOs (F)                     |
	|44	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------

```

> note: I ran busco on some length filtered redundan assemblies but the command and results are in the length filter markdown file in the working files directory

# Wed 06 May 2020 12:48:54 AM PDT

sooooo, apparently the version of busco I used for all of the above analyses contained a bug which resulted in the full_table.tsv file incorrectly listing fragmented buscos as complete. that version was busco 4.0.2. I am updating the conda package to busco 4.0.6 and rerunning all of the above analyses. Or at least most of them for now.

```bash

# Masurca 
busco -c 70 -m genome -i ../pcali_genome.masurca.fasta -o euk_masurca_pcali -l eukaryota_odb10
busco -c 70 -m genome -i ../pcali_genome.masurca.fasta -o met_masurca_pcali -l metazoa_odb10

# platanus-allee
busco -c 70 -m genome -i ../pcali_genome.platanus.fa -o euk_plat_pcali -l eukaryota_odb10
busco -c 70 -m genome -i ../pcali_genome.platanus.fa -o met_plat_pcali -l metazoa_odb10

# redundans
busco -c 70 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o euk_redun_pcali -l eukaryota_odb10
busco -c 70 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o met_redun_pcali -l metazoa_odb10

# apostichopus japonicus
busco -c 70 -m genome -i /home/jon/Working_Files/parastichopus_parvimensis/GCA_000934455.1_Ppar_1.0_genomic.fna -o euk_parv -l eukaryota_odb10
busco -c 70 -m genome -i /home/jon/Working_Files/parastichopus_parvimensis/GCA_000934455.1_Ppar_1.0_genomic.fna -o met_parv -l metazoa_odb10


# apostichopus japonicus
busco -c 70 -m genome -i /home/jon/Working_Files/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.fna -o euk_jap -l eukaryota_odb10
busco -c 70 -m genome -i /home/jon/Working_Files/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.fna -o met_jap -l metazoa_odb10

#done
```

# Mon 15 Jun 2020 03:19:43 PM PDT

running busco euk and metazoa on the pcali_ragout.fa assembly

```bash
busco -c 70 -m genome -i /home/jon/Working_Files/genome_assemblies/pcali_ragout.fa  -o euk_ragout_pcali -l eukaryota_odb10
busco -c 70 -m genome -i /home/jon/Working_Files/genome_assemblies/pcali_ragout.fa  -o met_ragout_pcali -l metazoa_odb10
```
INFO:	

	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:78.4%[S:75.7%,D:2.7%],F:16.1%,M:5.5%,n:255     |
	|200	Complete BUSCOs (C)                       |
	|193	Complete and single-copy BUSCOs (S)       |
	|7	Complete and duplicated BUSCOs (D)        |
	|41	Fragmented BUSCOs (F)                     |
	|14	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------
INFO:	BUSCO analysis done. Total running time: 668 seconds
INFO:	Results written in /home/jon/Working_Files/busco/euk_ragout_pcali


INFO:	

	--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:83.5%[S:79.6%,D:3.9%],F:11.4%,M:5.1%,n:954     |
	|796	Complete BUSCOs (C)                       |
	|759	Complete and single-copy BUSCOs (S)       |
	|37	Complete and duplicated BUSCOs (D)        |
	|109	Fragmented BUSCOs (F)                     |
	|49	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------
INFO:	BUSCO analysis done. Total running time: 2026 seconds
INFO:	Results written in /home/jon/Working_Files/busco/met_ragout_pcali

# Sun 22 Nov 2020 01:19:32 PM PST

running busco on all the genome assemblies. This uses auto-lineage so it auto detects the best dataset. However, it select fly so I may rerun it with metazoa and eukaryota after this. 
```bash
for file in *.fa
do
	echo "${file}"
	
	busco \
	-m genome \
	-i ${file} \
	-o auto_euk_"${file}" \
	--auto-lineage-euk \
	-c 70 \
	--long
done

```

# Mon 23 Nov 2020 10:18:23 AM PST

annnd done. that took awhile. Now going to run multiqc to generate a report
```bash
multiqc -s -m busco .
```