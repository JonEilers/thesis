
##run redundans with raw data, scaffold, and reference genome: results are kinda weird 

```bash
./redundans.py -i ../*.fq -t 70 -f ../Patanus-Allee/Platanus_allee_v2.0.2/out_consensusScaffold.fa --reference ../A_jap_genome.fasta --log LOG.txt -o cali_v_jap --resume
```

```bash
#run redundans without scaffold
./redundans.py -i ../*.fq -t 40 --reference ../A_jap_genome.fasta --log LOG.txt -o cali_v_jap --resume
```

> during gapclosing, it initially runs as multicore, but slowly reduces the number of cores until only one is doing anything. that one core will run for days. So I stop and resume the run to reset it and that seems to work.

> realized that redundans takes contigs for reference assisted genome assembly, not scaffolds. 

##redundans run with platanus-allee contigs, raw paired end data, and japonicus reference genome

```bash
./redundans.py -i ../*.fq -t 40 -f ../Patanus-Allee/assembly/out_contig.fa --reference ../A_jap_genome.fasta --log cali-contig_v_jap.txt -o cali-contig_v_jap
```

it worked! sweet. now to get some stats for it using bbtools

```bash
stats.sh in=scaffolds.filled.fa out=pcali_redundans_stats.txt gchist=pcali_redundans_gc shist=pcali_redundans_len 
```

stats results
```bash
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3140	0.1861	0.1862	0.3137	0.2528	0.0000	0.0000	0.3723	0.0513

Main genome scaffold total:         	304189
Main genome contig total:           	341022
Main genome scaffold sequence total:	878.550 MB
Main genome contig sequence total:  	656.423 MB  	25.283% gap
Main genome scaffold N/L50:         	908/232.994 KB
Main genome contig N/L50:           	15484/7.513 KB
Main genome scaffold N/L90:         	78509/985
Main genome contig N/L90:           	141289/686
Max scaffold length:                	2.355 MB
Max contig length:                  	342.646 KB
Number of scaffolds > 50 KB:        	2184
% main genome in scaffolds > 50 KB: 	67.80%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	       304,189	       341,022	   878,550,047	   656,422,679	  74.72%
    100 	       304,189	       341,022	   878,550,047	   656,422,679	  74.72%
    250 	       217,050	       253,883	   859,301,519	   637,174,151	  74.15%
    500 	       135,152	       171,982	   830,581,215	   608,458,355	  73.26%
   1 KB 	        77,404	       114,148	   789,640,472	   567,534,088	  71.87%
 2.5 KB 	        25,533	        62,147	   708,579,135	   486,496,492	  68.66%
   5 KB 	         8,199	        44,731	   649,692,467	   427,636,002	  65.82%
  10 KB 	         3,280	        39,741	   617,382,791	 
```


# Wed 15 Jul 2020 12:12:40 PM PDT

running with no contig assembly as input. 
```bash
./redundans.py \
	-i /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 \
	--reference /home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta \
	--log pcali_ajap_no-contig.txt \
	-o pcali_ajap_no-contig
```


# Tue 21 Jul 2020 11:05:09 AM PDT

so it got stuck. Redundans limits the amount of ram to use for platanus and it appears to choke when platanus needs more. I am going to run platanus by itself and then use the contig assemble from that for redundans. 

see the paltanus_allee commands for the assembly. 

# Fri 24 Jul 2020 10:56:48 AM PDT

kk, platanus finished. time to use the contigs with redundans

```bash
./redundans.py \
	-f /home/jon/Working_Files/platanus-allee/platanus_assembly/pcali_platanus_illumina-only_contig.fa \
	-i /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 \
	--reference /home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta \
	--log pcali-ajap_platanus.txt \
	-o pcali-ajap_platanus
```

looks like the nanopore data isn't going to be useful?
```bash
[WARNING] No alignments for /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq - /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq!
#Time elapsed: 1:52:36.886818
```


```bash
stats.sh scaffolds.filled.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3142	0.1863	0.1861	0.3134	0.3997	0.0000	0.0000	0.3724	0.0480

Main genome scaffold total:         	257865
Main genome contig total:           	498434
Main genome scaffold sequence total:	859.713 MB
Main genome contig sequence total:  	516.118 MB  	39.966% gap
Main genome scaffold N/L50:         	877/243.045 KB
Main genome contig N/L50:           	85424/1.688 KB
Main genome scaffold N/L90:         	75242/1.049 KB
Main genome contig N/L90:           	315524/447
Max scaffold length:                	2.332 MB
Max contig length:                  	34.939 KB
Number of scaffolds > 50 KB:        	2194
% main genome in scaffolds > 50 KB: 	69.36%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	       257,865	       498,434	   859,712,561	   516,118,196	  60.03%
    100 	       257,865	       498,434	   859,712,561	   516,118,196	  60.03%
    250 	       222,839	       463,408	   851,934,230	   508,339,865	  59.67%
    500 	       143,249	       383,817	   823,455,729	   479,861,379	  58.27%
   1 KB 	        79,171	       319,732	   777,811,294	   434,217,049	  55.83%
 2.5 KB 	        22,401	       262,929	   689,890,097	   346,301,245	  50.20%
   5 KB 	         6,357	       246,783	   635,967,818	   292,448,603	  45.98%
  10 KB 	         2,941	       243,162	   613,917,138	   270,626,557	  44.08%
  25 KB 	         2,481	       242,118	   607,471,195	   265,647,826	  43.73%
  50 KB 	         2,194	       239,132	   596,310,840	   262,460,156	  44.01%
 100 KB 	         1,668	       228,558	   558,091,596	   251,762,496	  45.11%
 250 KB 	           863	       183,357	   426,537,749	   203,976,981	  47.82%
 500 KB 	           325	       105,433	   233,689,894	   118,261,227	  50.61%
   1 MB 	            30	        19,031	    38,391,682	    21,542,982	  56.11%
```


ok, now to use the contigs from the platanus-allee assembly which also included the nanopore data in the assembly.
```bash
./redundans.py \
	-f /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_hybrid_assembly/pcali_hybrid_assembly_contig.fa \
	-i /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 \
	--reference /home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta \
	--log pcali-ajap_platanus-allee-hybrid.txt \
	-o pcali-ajap_platanus-alle-hybrid
```

annnnd not surprisingly
```bash
[WARNING] No alignments for /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq - /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq!
#Time elapsed: 2:06:06.329378
```

```bash
stats.sh scaffolds.filled.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3139	0.1862	0.1862	0.3137	0.4054	0.0000	0.0000	0.3724	0.0532

Main genome scaffold total:         	403745
Main genome contig total:           	964304
Main genome scaffold sequence total:	886.790 MB
Main genome contig sequence total:  	527.257 MB  	40.543% gap
Main genome scaffold N/L50:         	904/239.788 KB
Main genome contig N/L50:           	142544/895
Main genome scaffold N/L90:         	118151/582
Main genome contig N/L90:           	641703/222
Max scaffold length:                	2.361 MB
Max contig length:                  	53.272 KB
Number of scaffolds > 50 KB:        	2280
% main genome in scaffolds > 50 KB: 	69.37%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	       403,745	       964,304	   886,790,406	   527,256,566	  59.46%
    100 	       403,745	       964,304	   886,790,406	   527,256,566	  59.46%
    250 	       288,990	       849,549	   861,361,309	   501,827,469	  58.26%
    500 	       138,818	       699,372	   809,287,629	   449,753,864	  55.57%
   1 KB 	        64,049	       624,572	   757,280,658	   397,748,765	  52.52%
 2.5 KB 	        16,791	       577,238	   685,466,895	   325,943,452	  47.55%
   5 KB 	         5,521	       565,803	   647,733,617	   288,308,821	  44.51%
  10 KB 	         2,975	       562,775	   631,207,583	   272,183,876	  43.12%
  25 KB 	         2,526	       561,428	   624,856,828	   267,330,736	  42.78%
  50 KB 	         2,280	       556,907	   615,123,249	   264,635,586	  43.02%
 100 KB 	         1,714	       533,994	   574,560,091	   253,165,652	  44.06%
 250 KB 	           881	       434,506	   437,865,939	   204,791,358	  46.77%
 500 KB 	           339	       257,955	   243,388,450	   120,446,104	  49.49%
   1 MB 	            30	        47,647	    39,068,216	    21,294,511	  54.51%
```

# Tue 20 Oct 2020 12:50:46 AM PDT

going to run redundans with the contigs from the kmer=10 assembly to see if that improves things. the nanopore data has already been shown to be useless so I won't use that. 

hmm, I forget if I ever figured out whether I should use the diploid contigs or the phased contigs. I guess it doesn't hurt to try both. 

diploid contigs run
```bash
./redundans.py \
  -f /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_step10_assembly/pcali_illumina_assembly_step10_contig.fa\
  -i /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq \
  /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
  -t 70 \
  --reference /home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta \
  --log pcali-ajap_redun_plat-allee_k10.txt \
  -o pcali-ajap_redun_plat-allee_k10
```

# Tue 27 Oct 2020 08:53:44 PM PDT

whelp that only took five days...

```bash
[WARNING] numpy or matplotlib missing! Cannot plot histogram
^[[A[WARNING] numpy or matplotlib missing! Cannot plot histogram
#Time elapsed: 5 days, 18:43:37.146062
```

huh, looks like it actually did worse then the other assemblies. ohwell. 

next up is to see what happens if I use the phased assembly I guess? 

# Sat 07 Nov 2020 03:06:04 PM PST

k, so i think I figured out where the masurca contigs end up so let's try a round of redundans with masurca contigs

```bash

# activate redundans environment as it requires python 2.7
conda activate redundan

# running redundans
./redundans.py \
  -f /home/jon/Working_Files/MaSuRCA-3.4.1/bin/mr.41.17.20.0.05.1.contigs.fa \
  -i /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq \
  /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
  -t 70 \
  --reference /home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta \
  --log pcali-ajap_redun_masurca_illumina-no-ref.txt \
  -o pcali-ajap_redun_masurca_illumina-no-ref

```

some notes first: I believe these will be haploid contigs, so it'll be interesting to see what redundan does with that compared to how it handles the platanus diploid contigs. 

# Fri 20 Nov 2020 04:29:08 PM PST

finaaaally finished. yeesh
```bash
[WARNING] numpy or matplotlib missing! Cannot plot histogram
^[[B^[[B^[[B[WARNING] numpy or matplotlib missing! Cannot plot histogram
#Time elapsed: 12 days, 13:10:53.884959
```

so it doesn't look great. the redundans output is less than 400 mb which is a far cry from the 900 mb the actual genome is. Not sure what happened there. Although it may be because the contigs were for a haploid genome assembly. I wonder what would happen if I gave it masurca diploid contigs (if those exist).

huh, there is a 1.8 gb fasta files called subassemblies. But I can't tell if it what I am looking for. soooo moving on for now. 