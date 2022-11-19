```bash
./platanus_allee assemble -s 30 -t 70 -m 210 -f ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 2>assemble.log

./platanus_allee phase -t 70 -c out_contig.fa out_junctionKmer.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 2>phase.log

./platanus_allee consensus -t 70 -c out_primaryBubble.fa out_nonBubbleHomoCandidate.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 2>consensus.log
```



Output
```bash
--------------------------------
super weird, using the preprocessed (trimmed) reads, plat-allee maxes out the ram and cant finished. See output below

platanus_allee version: 2.0.2
./platanus_allee assemble -s 30 -t 70 -m 210 -f ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

K = 32, saving kmers from reads...
AVE_READ_LEN=142.65

KMER_EXTENSION:
K=32, KMER_COVERAGE=89.9424 (>= 6), COVERAGE_CUTOFF=6
K=62, KMER_COVERAGE=65.7752, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-inf
K=92, KMER_COVERAGE=41.608, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-10.508
K=107, KMER_COVERAGE=29.5244, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.1335
K=109, KMER_COVERAGE=27.9132, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.1843
K=110, KMER_COVERAGE=27.1077, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.0229
loading kmers...
connecting kmers...
removing branches...
BRANCH_DELETE_THRESHOLD=0.5
NUM_CUT=298589
NUM_CUT=3194
NUM_CUT=21
NUM_CUT=0
TOTAL_NUM_CUT=301804
extracting reads (containing kmer used in contig assemble)...
K = 62, loading kmers from contigs...
K = 62, saving additional kmers(not found in contigs) from reads...
COVERAGE_CUTOFF = 6

#### PROCESS INFORMATION ####
VmPeak:         200.807 GByte
VmHWM:            5.076 GByte

```
> note this failed

# 19 Nov 2019

```bash
./platanus_allee assemble -t 70 -m 352 -f ../P_cali_g_1.fq.trimmed  ../P_cali_g_2.fq.trimmed 2>assemble.log

platanus_allee version: 2.0.2
./platanus_allee phase -t 70 -c out_contig.fa out_junctionKmer.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

phase completed!

#### PROCESS INFORMATION ####
VmPeak:           0.023 GByte
VmHWM:            0.005 GByte

platanus_allee version: 2.0.2
./platanus_allee consensus -t 70 -c out_primaryBubble.fa out_nonBubbleHomoCandidate.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

```




# Mon 29 Jun 2020 11:19:45 PM PDT

running platanus-allee using the previously trimmed raw illumina data and the oxford nanopore
```bash
./platanus_allee assemble \
	-o pcali_hybrid_assembly \
	-t 75 \
	-m 360 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	/home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	2>assemble.log
```

# Tue 30 Jun 2020 08:24:57 PM PDT
and it finished
```bash
assemble completed!

#### PROCESS INFORMATION ####
VmPeak:         314.847 GByte
VmHWM:           23.129 GByte
```

stats
```bash
stats.sh pcali_hybrid_assembly_contig.fa 
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3104	0.1896	0.1898	0.3103	0.0000	0.0000	0.0000	0.3794	0.0710
Main genome scaffold total:         	7330953
Main genome contig total:           	7330953
Main genome scaffold sequence total:	1723.895 MB
Main genome contig sequence total:  	1723.895 MB  	0.000% gap
Main genome scaffold N/L50:         	1234532/278
Main genome contig N/L50:           	1234532/278
Main genome scaffold N/L90:         	7330953/110
Main genome contig N/L90:           	7330953/110
Max scaffold length:                	53.272 KB
Max contig length:                  	53.272 KB
Number of scaffolds > 50 KB:        	1
main genome in scaffolds > 50 KB: 	0.00
Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     7,330,953	     7,330,953	 1,723,895,422	 1,723,895,422	 100.00%
    100 	     7,330,953	     7,330,953	 1,723,895,422	 1,723,895,422	 100.00%
    250 	     1,425,797	     1,425,797	   913,222,329	   913,222,329	 100.00%
    500 	       540,845	       540,845	   613,617,081	   613,617,081	 100.00%
   1 KB 	       204,234	       204,234	   381,693,226	   381,693,226	 100.00%
 2.5 KB 	        34,010	        34,010	   129,252,006	   129,252,006	 100.00%
   5 KB 	         4,756	         4,756	    33,254,343	    33,254,343	 100.00%
  10 KB 	           435	           435	     5,842,498	     5,842,498	 100.00%
  25 KB 	            13	            13	       410,496	       410,496	 100.00%
  50 KB 	             1	             1	        53,272	        53,272	 100.00%
```

stats on the first platanus-allee genome contigs from last year using no nanopore reads. Note this used platanus-allee 2.0.2. Also, note this is for a diploid genome and hasn't been phased yet. 
```bash
stats.sh out_contig.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3112	0.1890	0.1890	0.3108	0.0000	0.0000	0.0000	0.3780	0.0671

Main genome scaffold total:         	4404942
Main genome contig total:           	4404942
Main genome scaffold sequence total:	1402.030 MB
Main genome contig sequence total:  	1402.030 MB  	0.000% gap
Main genome scaffold N/L50:         	739862/395
Main genome contig N/L50:           	739862/395
Main genome scaffold N/L90:         	3228290/135
Main genome contig N/L90:           	3228290/135
Max scaffold length:                	53.272 KB
Max contig length:                  	53.272 KB
Number of scaffolds > 50 KB:        	1
% main genome in scaffolds > 50 KB: 	0.00%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     4,404,942	     4,404,942	 1,402,030,309	 1,402,030,309	 100.00%
    100 	     4,404,942	     4,404,942	 1,402,030,309	 1,402,030,309	 100.00%
    250 	     1,425,803	     1,425,803	   913,214,391	   913,214,391	 100.00%
    500 	       540,777	       540,777	   613,573,571	   613,573,571	 100.00%
   1 KB 	       204,217	       204,217	   381,674,798	   381,674,798	 100.00%
 2.5 KB 	        33,999	        33,999	   129,231,490	   129,231,490	 100.00%
   5 KB 	         4,749	         4,749	    33,252,678	    33,252,678	 100.00%
  10 KB 	           435	           435	     5,864,592	     5,864,592	 100.00%
  25 KB 	            14	            14	       435,945	       435,945	 100.00%
  50 KB 	             1	             1	        53,272	        53,272	 100.00%
```

moving on to phasing I guess. 
```bash
./platanus_allee phase \
	-o pcali_hybrid_assembly \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_hybrid_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	-t 70 \
	-mapper /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/minimap2-2.0-r191/minimap2 \
	2>phase.log
```

# Thu 02 Jul 2020 12:13:59 PM PDT

the phase command ended with Error(13): Error, SolveDBG exception!!
platanus_allee solve_DBG command failed.

not sure why. Although I did just notice I forgot to change the output file name. errrp, hope it didn't mess up the contig file. 

running it again, without mapper specified
```bash
./platanus_allee phase \
	-o pcali_hybrid_phased \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_hybrid_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	-t 70 \
	2>phase_2.log
```

# Fri 03 Jul 2020 03:52:33 PM PDT
threw the error again. going to run it without the nanopore data
```bash
./platanus_allee phase \
	-o pcali_hybrid_phased \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_hybrid_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 \
	2>phase_2.log
```

huh, got the error again
```bash
dividing erroneous scaffolds based on base-level coverages ...
Segmentation fault (core dumped)
```

# Tue 07 Jul 2020 05:55:29 PM PDT

running the assemble command again without the nanopore data
```bash
./platanus_allee assemble \
	-o pcali_assembly \
	-t 60 \
	-m 330 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	2>pcali_illumina_assemble.log
```

# Wed 08 Jul 2020 01:14:09 PM PDT

assembling finished, moving on to phasing using illumina and the nanopore data
```bash
./platanus_allee phase \
	-o pcali_hybrid_phased \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_illumina_assembly/pcali_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	-t 70 \
	2>pcali_hybrid_phased.log
```

# Thu 09 Jul 2020 03:09:38 PM PDT
siiigh, got the error again. Now am going to try without the nanopore data
```bash
./platanus_allee phase \
	-o pcali_hybrid_phased_2 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_illumina_assembly/pcali_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 \
	2>pcali_hybrid_phased_2.log
```

same error

# Tue 21 Jul 2020 11:06:23 AM PDT
I haven't heard from the developer of platanus-allee after I emailed him about the error. Will try again soon.

running platanus 1.2.4 assembly
```bash
./platanus assemble \
	-o pcali_platanus_hybrid \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	/home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	-t 60 \
	-m 320
```

it complainined 
```bash
Error(4): Read file exception!!
DNA read seq length is too long.
```

running without the long read data
```bash
./platanus assemble \
	-o pcali_platanus_illumina-only \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 60 \
	-m 320 2> pcali_platanus_illumina-only.log
```


# Thu 23 Jul 2020 09:41:28 AM PDT
finally finished, yeesh. running the phasing part now
```bash
./platanus scaffold \
	-o pcali_platanus_illumina-only \
	-c /home/jon/Working_Files/platanus-allee/platanus_assembly/pcali_platanus_illumina-only_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 2> scaffold.log
```

```bash
stats.sh pcali_platanus_illumina-only_scaffold.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3130	0.1877	0.1877	0.3116	0.0065	0.0000	0.0000	0.3754	0.0713

Main genome scaffold total:         	1591660
Main genome contig total:           	1738101
Main genome scaffold sequence total:	851.760 MB
Main genome contig sequence total:  	846.246 MB  	0.647% gap
Main genome scaffold N/L50:         	68001/3.031 KB
Main genome contig N/L50:           	107241/1.957 KB
Main genome scaffold N/L90:         	868269/116
Main genome contig N/L90:           	1014691/116
Max scaffold length:                	78.543 KB
Max contig length:                  	42.197 KB
Number of scaffolds > 50 KB:        	17
% main genome in scaffolds > 50 KB: 	0.12%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     1,591,660	     1,738,101	   851,760,126	   846,245,778	  99.35%
     50 	     1,591,660	     1,738,101	   851,760,126	   846,245,778	  99.35%
    100 	     1,591,660	     1,738,101	   851,760,126	   846,245,778	  99.35%
    250 	       317,957	       464,398	   696,764,688	   691,250,340	  99.21%
    500 	       248,908	       392,771	   671,844,598	   666,421,078	  99.19%
   1 KB 	       176,061	       305,816	   619,262,791	   614,368,041	  99.21%
 2.5 KB 	        84,367	       178,502	   471,054,383	   467,513,653	  99.25%
   5 KB 	        33,761	        89,676	   293,600,365	   291,501,986	  99.29%
  10 KB 	         8,187	        28,817	   119,218,838	   118,447,998	  99.35%
  25 KB 	           367	         2,025	    11,891,348	    11,828,616	  99.47%
  50 KB 	            17	           112	     1,008,712	     1,005,634	  99.69%
```

gap closing

```bash
./platanus gap_close \
	-o pcali_illumina_assembly-only \
	-c /home/jon/Working_Files/platanus-allee/platanus_assembly/pcali_platanus_illumina-only_scaffold.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 2> gap_close.log
```

huh, didn't do much of anything

ok, grabbing stats for the platanus-allee contig assembly
```bash
stats.sh pcali_hybrid_assembly_contig.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3104	0.1896	0.1898	0.3103	0.0000	0.0000	0.0000	0.3794	0.0710

Main genome scaffold total:         	7330953
Main genome contig total:           	7330953
Main genome scaffold sequence total:	1723.895 MB
Main genome contig sequence total:  	1723.895 MB  	0.000% gap
Main genome scaffold N/L50:         	1234532/278
Main genome contig N/L50:           	1234532/278
Main genome scaffold N/L90:         	7330953/110
Main genome contig N/L90:           	7330953/110
Max scaffold length:                	53.272 KB
Max contig length:                  	53.272 KB
Number of scaffolds > 50 KB:        	1
% main genome in scaffolds > 50 KB: 	0.00%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     7,330,953	     7,330,953	 1,723,895,422	 1,723,895,422	 100.00%
    100 	     7,330,953	     7,330,953	 1,723,895,422	 1,723,895,422	 100.00%
    250 	     1,425,797	     1,425,797	   913,222,329	   913,222,329	 100.00%
    500 	       540,845	       540,845	   613,617,081	   613,617,081	 100.00%
   1 KB 	       204,234	       204,234	   381,693,226	   381,693,226	 100.00%
 2.5 KB 	        34,010	        34,010	   129,252,006	   129,252,006	 100.00%
   5 KB 	         4,756	         4,756	    33,254,343	    33,254,343	 100.00%
  10 KB 	           435	           435	     5,842,498	     5,842,498	 100.00%
  25 KB 	            13	            13	       410,496	       410,496	 100.00%
  50 KB 	             1	             1	        53,272	        53,272	 100.00%
```

and stats for the platanus contig assembly
```bash
stats.sh pcali_platanus_illumina-only_contig.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3113	0.1897	0.1898	0.3093	0.0000	0.0000	0.0000	0.3794	0.0670

Main genome scaffold total:         	3070124
Main genome contig total:           	3070124
Main genome scaffold sequence total:	1134.111 MB
Main genome contig sequence total:  	1134.111 MB  	0.000% gap
Main genome scaffold N/L50:         	341262/720
Main genome contig N/L50:           	341262/720
Main genome scaffold N/L90:         	2111863/129
Main genome contig N/L90:           	2111863/129
Max scaffold length:                	34.939 KB
Max contig length:                  	34.939 KB
Number of scaffolds > 50 KB:        	0
% main genome in scaffolds > 50 KB: 	0.00%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     3,070,124	     3,070,124	 1,134,110,621	 1,134,110,621	 100.00%
    100 	     3,070,124	     3,070,124	 1,134,110,621	 1,134,110,621	 100.00%
    250 	       957,910	       957,910	   816,375,397	   816,375,397	 100.00%
    500 	       488,048	       488,048	   655,128,749	   655,128,749	 100.00%
   1 KB 	       233,751	       233,751	   476,134,327	   476,134,327	 100.00%
 2.5 KB 	        50,498	        50,498	   197,279,008	   197,279,008	 100.00%
   5 KB 	         8,198	         8,198	    56,802,884	    56,802,884	 100.00%
  10 KB 	           687	           687	     8,789,979	     8,789,979	 100.00%
  25 KB 	             8	             8	       233,475	       233,475	 100.00%
```

checking if the nanopore data made a difference in the contig assembly
```bash
stats.sh pcali_assembly_contig.fa 
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3104	0.1897	0.1897	0.3102	0.0000	0.0000	0.0000	0.3794	0.0710

Main genome scaffold total:         	7329809
Main genome contig total:           	7329809
Main genome scaffold sequence total:	1723.767 MB
Main genome contig sequence total:  	1723.767 MB  	0.000% gap
Main genome scaffold N/L50:         	1234647/278
Main genome contig N/L50:           	1234647/278
Main genome scaffold N/L90:         	7329809/110
Main genome contig N/L90:           	7329809/110
Max scaffold length:                	53.272 KB
Max contig length:                  	53.272 KB
Number of scaffolds > 50 KB:        	1
% main genome in scaffolds > 50 KB: 	0.00%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     7,329,809	     7,329,809	 1,723,766,659	 1,723,766,659	 100.00%
    100 	     7,329,809	     7,329,809	 1,723,766,659	 1,723,766,659	 100.00%
    250 	     1,425,774	     1,425,774	   913,210,227	   913,210,227	 100.00%
    500 	       540,813	       540,813	   613,594,688	   613,594,688	 100.00%
   1 KB 	       204,212	       204,212	   381,669,471	   381,669,471	 100.00%
 2.5 KB 	        34,005	        34,005	   129,244,061	   129,244,061	 100.00%
   5 KB 	         4,746	         4,746	    33,235,415	    33,235,415	 100.00%
  10 KB 	           435	           435	     5,864,592	     5,864,592	 100.00%
  25 KB 	            14	            14	       435,945	       435,945	 100.00%
  50 KB 	             1	             1	        53,272	        53,272	 100.00%
```
looks like the illumina only assembly may be slightly more complete, for what it's worth. but also
looks like the platanus-allee contig assembly is better. going to use that for the ragtag scaffolding


going to try to phase the pcali_hybrid_assembly_contig.fa assembly
```bash
./platanus_allee phase \
	-o pcali_hybrid_phased_3 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_illumina_assembly/pcali_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 \
	-i 5  2>pcali_hybrid_phased_3.log
```


wait, ok, just realized I am missing PREFIX_junctionKmer.fa file which gets used in the phase step. So I am going to rerun the platanus-allee assemble step and then run the above phase step. weird that platanus-allee didn't create it during assembly. I probably deleted it......

```bash
./platanus_allee assemble \
	-o pcali_hybrid_assembly \
	-t 75 \
	-m 360 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	/home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	2>assemble.log
```


# Mon 27 Jul 2020 09:54:42 AM PDT

finished a few days ago. now to run the phasing step. there is no prefix_junctionkmer.fa Also, the developers of platanus emailed me a fixed version of platanus-allee v2.2.2 that was fixed in july. After this next phasing I will redo the assembly using the the fixed version. 

```bash
./platanus_allee phase \
	-o pcali_phased \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2/pcali_hybrid_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 \
	-i 5 2>pcali_phased.log
```

# Tue 28 Jul 2020 12:46:34 PM PDT

segmentation error again. Ok, now to try the fixed version
```bash
./platanus_allee assemble \
	-o pcali_hybrid_assembly \
	-t 75 \
	-m 360 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	/home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	2>assemble.log
```

# Wed 29 Jul 2020 03:47:38 PM PDT

success, moving onto phasing
```bash
./platanus_allee phase \
	-o pcali_phased \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2_fixed_20200709/pcali_hybrid_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/nanopore/basecalled_results/nanopore_data.filter200.trimmed90.fastq \
	-t 70 2>pcali_hybrid_phased.log
```

# Thu 30 Jul 2020 10:49:50 PM PDT
oook, it crashed again. going to try running it without the nanopore seeing as I will be gone over the weekend. 
```bash
./platanus_allee assemble \
	-o pcali_illumina_assembly \
	-t 60 \
	-m 330 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	2>assemble.log
```

# Mon 03 Aug 2020 12:46:33 PM PDT

k, now to phase it
```bash
./platanus_allee phase \
	-o pcali_illumina_phased \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2_fixed_20200709/pcali_illumina_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 2>pcali_illumina_phased.log
```

# Tue 04 Aug 2020 06:20:18 PM PDT
crashed again. I just read a paper in which they used different increments and that impacted the number of proteins predicted. I am tempted to try this. Starting with k=2 and using the latest platanus-allee version.

```bash
./platanus_allee assemble \
	-o pcali_illumina_k2_step3_assembly \
	-t 75 \
	-m 360 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-s 3 2>assemblek2step3.log
```

# Tue 11 Aug 2020 07:41:04 PM PDT
kk, time to phase it
```bash
./platanus_allee phase \
	-o pcali_illumina_phased_k2_step3 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2_fixed_20200709/pcali_illumina_k2_step3_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 2>pcali_illumina_k2_step3_phased.log
```
# Wed 12 Aug 2020 02:52:26 PM PDT

screen wouldn't refresh. starting over
```bash
./platanus_allee phase \
	-o pcali_illumina_phased_k2_step3 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.2.2_fixed_20200709/pcali_illumina_k2_step3_assembly_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 2>pcali_illumina_k2_step3_phased.log
```
# Fri 14 Aug 2020 12:51:14 PM PDT

annd same problem. k, looks like I will try using the original platanus allee v2 but with the above k and step numbers.

```bash
./platanus_allee assemble \
	-o pcali_illumina_k2_step3_assembly \
	-t 75 \
	-m 360 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-s 3 2>assemblek2step3.log
```

# Tue 18 Aug 2020 09:33:24 PM PDT

soo, it aborted or something

```bash
extracting reads (containing kmer used in contig assemble)...
K = 65, loading kmers from contigs...
K = 65, saving additional kmers(not found in contigs) from reads...
terminate called recursively
terminate called recursively
```
and from the command line
```bash
Aborted (core dumped)
```

trying again, but with a bigger step size
```bash
./platanus_allee assemble \
	-o pcali_illumina_step5_assembly \
	-t 75 \
	-m 360 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-s 5 2>assemble_step5.log
```

# Thu 20 Aug 2020 12:02:37 AM PDT
ok, it happened again. I need to back up the server as I am planning on moving it to yakima. But I am wondering if this is a memory issue. I should run platanus as I did the first time successfully and see if it gives me another error. 

# Sun 23 Aug 2020 08:27:04 PM PDT

running platanus allee normally
```bash
./platanus_allee assemble \
	-o pcali_illumina_assembly_step10 \
	-t 75 \
	-m 360 \
	-f /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_1.trim.fq  \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/P_cali_g_2.trim.fq \
	-s 10 2>assemble_step10.log
```

it could also be that I do not have enough memory for the small step size? Will try 10 and see what happens. In fact, for the previous two runs it fails at the 12th step. The default step size is 20, which gives 7 steps and uses 314gb of ram. If I use a step size of 10, that would be 32,42,52,62,72,82,92,102,107,109,110 or close to this. which would be 11 steps. worth a shot anyways. 


recording memory usage once per a minute for two days 
```bash
for i in `seq 0 2880`; do
  echo `cat /proc/meminfo | grep Active: | sed 's/Active: //g'` >> usage.txt
  sleep 1m
done
```


# Sun 30 Aug 2020 12:56:20 PM PDT
huh, it appears I didn't record the phasing commands when I ran this. weird. 

so here it is
```bash
./platanus_allee phase \
	-o pcali_illumina_phased_step10 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_assembly_step10_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 2>pcali_illumina_step10_phased.log

```


k, phasing finished yesterday evening. Now for the consensus sequence

```bash
./platanus_allee consensus \
	 -t 70 \
	 -c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step10_nonBubbleHomoCandidate.fa \
	 /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step10_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	 2>consensus_pcali_step10.log
```

oook, something weird happened. maybe I closed this commands file without saving or something, but the above assembly command above is wrong. the raw read input files are incorrect and the command would not have worked. weird


ok, some stats
```bash
stats.sh out_consensusScaffold.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3122	0.1884	0.1884	0.3110	0.0009	0.0000	0.0000	0.3768	0.0740

Main genome scaffold total:         	1353780
Main genome contig total:           	1416102
Main genome scaffold sequence total:	1131.890 MB
Main genome contig sequence total:  	1130.846 MB  	0.092% gap
Main genome scaffold N/L50:         	54105/4.954 KB
Main genome contig N/L50:           	68370/3.896 KB
Main genome scaffold N/L90:         	607663/219
Main genome contig N/L90:           	669416/219
Max scaffold length:                	125.291 KB
Max contig length:                  	118.461 KB
Number of scaffolds > 50 KB:        	126
% main genome in scaffolds > 50 KB: 	0.68%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     1,353,780	     1,416,102	 1,131,890,429	 1,130,845,830	  99.91%
    100 	     1,353,780	     1,416,102	 1,131,890,429	 1,130,845,830	  99.91%
    250 	       388,349	       450,671	   974,129,705	   973,085,106	  99.89%
    500 	       270,752	       332,958	   932,307,988	   931,264,761	  99.89%
   1 KB 	       185,588	       245,179	   872,297,003	   871,294,381	  99.89%
 2.5 KB 	       103,310	       150,545	   739,951,721	   739,143,638	  99.89%
   5 KB 	        53,565	        85,169	   563,264,054	   562,716,346	  99.90%
  10 KB 	        19,736	        35,243	   326,613,670	   326,345,527	  99.92%
  25 KB 	         2,105	         4,709	    69,938,044	    69,894,361	  99.94%
  50 KB 	           126	           356	     7,657,217	     7,653,850	  99.96%
 100 KB 	             4	            24	       459,422	       459,154	  99.94%


assembly_stats out_consensusScaffold.fa
{
  "Contig Stats": {
    "L10": 5134,
    "L20": 14013,
    "L30": 26740,
    "L40": 44375,
    "L50": 69038,
    "N10": 15930,
    "N20": 10574,
    "N30": 7562,
    "N40": 5468,
    "N50": 3859,
    "gc_content": 37.68388719763137,
    "longest": 118461,
    "mean": 796.832134877222,
    "median": 213.0,
    "sequence_count": 1419187,
    "shortest": 110,
    "total_bps": 1130853807
  },
  "Scaffold Stats": {
    "L10": 4042,
    "L20": 11030,
    "L30": 21054,
    "L40": 34868,
    "L50": 54103,
    "N10": 20164,
    "N20": 13437,
    "N30": 9652,
    "N40": 7005,
    "N50": 4954,
    "gc_content": 37.68388719763137,
    "longest": 125291,
    "mean": 836.0962852162095,
    "median": 205.0,
    "sequence_count": 1353780,
    "shortest": 110,
    "total_bps": 1131890429
  }
}
```

so for using long read data with platanus allee, it is incorporated in the phasing and scaffolding steps. so I will use both the step 10 and step 20 contig assemblies for this

running platanus-allee phasing with illumina and the nanopore data on the step20 contigs
```bash
./platanus_allee phase \
	-o pcali_illumina_nanopore_phased_step20 \
	-c /home/jon/Working_Files/platanus-allee/assembly/out_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	-t 70 2>pcali_illumina_nanopore_step20_phased.log
```

/home/jon/Working_Files/platanus-allee/assembly/out_contig.fa


# Sat 05 Sep 2020 12:14:29 AM PDT

finished earlier today. running scaffolding

```bash
./platanus_allee consensus \
	 -t 70 \
	 -c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step20_nonBubbleHomoCandidate.fa \
	 /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step20_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	 2>consensus_pcali_nanopore_step20.log
```

# Sat 12 Sep 2020 02:56:19 PM PDT

stats for step 20 with nanopore data
```bash
stats.sh out_consensusScaffold.fa
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3118	0.1884	0.1885	0.3113	0.0009	0.0000	0.0000	0.3769	0.0751

Main genome scaffold total:         	1378493
Main genome contig total:           	1439310
Main genome scaffold sequence total:	1133.378 MB
Main genome contig sequence total:  	1132.334 MB  	0.092% gap
Main genome scaffold N/L50:         	55081/4.846 KB
Main genome contig N/L50:           	69309/3.839 KB
Main genome scaffold N/L90:         	618126/219
Main genome contig N/L90:           	678187/219
Max scaffold length:                	125.288 KB
Max contig length:                  	97.424 KB
Number of scaffolds > 50 KB:        	120
% main genome in scaffolds > 50 KB: 	0.65%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	     1,378,493	     1,439,310	 1,133,377,786	 1,132,334,345	  99.91%
    100 	     1,378,493	     1,439,310	 1,133,377,786	 1,132,334,345	  99.91%
    250 	       394,745	       455,558	   972,485,196	   971,441,795	  99.89%
    500 	       272,548	       333,191	   929,070,444	   928,029,117	  99.89%
   1 KB 	       186,383	       244,396	   868,391,273	   867,390,489	  99.88%
 2.5 KB 	       103,068	       148,888	   734,522,567	   733,717,379	  99.89%
   5 KB 	        53,182	        83,722	   557,360,699	   556,816,482	  99.90%
  10 KB 	        19,499	        34,414	   322,047,135	   321,782,630	  99.92%
  25 KB 	         1,998	         4,473	    66,619,838	    66,577,062	  99.94%
  50 KB 	           120	           335	     7,345,845	     7,342,197	  99.95%
 100 KB 	             4	            26	       459,264	       458,840	  99.91%

```

```bash
assembly_stats out_consensusScaffold.fa

{
  "Contig Stats": {
    "L10": 5184,
    "L20": 14139,
    "L30": 27008,
    "L40": 44896,
    "L50": 69961,
    "N10": 15785,
    "N20": 10499,
    "N30": 7476,
    "N40": 5397,
    "N50": 3797,
    "gc_content": 37.686751171198274,
    "longest": 97424,
    "mean": 785.0546598871028,
    "median": 213.0,
    "sequence_count": 1442374,
    "shortest": 110,
    "total_bps": 1132342430
  },
  "Scaffold Stats": {
    "L10": 4110,
    "L20": 11186,
    "L30": 21342,
    "L40": 35423,
    "L50": 55077,
    "N10": 19904,
    "N20": 13299,
    "N30": 9502,
    "N40": 6864,
    "N50": 4846,
    "gc_content": 37.686751171198274,
    "longest": 125288,
    "mean": 822.1861017792619,
    "median": 204.0,
    "sequence_count": 1378493,
    "shortest": 110,
    "total_bps": 1133377786
  }
}
```

sigh, realized I fucked up again. When phasing I need to include the junctionKmer.fa. If i don't then it doesn't phase correctly. I messed up the last two phasing steps for the step 10 and step 20 with nanopore data. Thats gonna eat up some time again. 

Also, I should run a step10 phase with nanopore just to compare things. I would like to add a step 30, but I am not sure I should spend the time to assemble and run two phasing/consensus steps


# Tue 15 Sep 2020 08:37:17 PM PDT

k, redoing the phasing steps for step 10 and step 20 with nanopore data for both. 

running platanus-allee phasing

First command is with illumina and the nanopore data on the step20 contigs

The second command is with illumina only
```bash
date +%T
echo platanus-allee phasing with illumina and nanopore data

./platanus_allee phase \
	-o pcali_illumina_nanopore_phased_step20 \
	-c /home/jon/Working_Files/platanus-allee/assembly/out_contig.fa \
	/home/jon/Working_Files/platanus-allee/assembly/out_junctionKmer.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	-t 60 \
	2>pcali_illumina_nanopore_step20_phased.log

date +%T
echo platanus-allee consensus with illumina and nanopore

./platanus_allee consensus \
	-o pcali_illumina_nanopore_consensus_step20 \
	-t 60 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step20_nonBubbleHomoCandidate.fa \
	/home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step20_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	2>consensus_pcali_nanopore_step20.log

date

./platanus_allee phase \
	-o pcali_illumina_phased_step20 \
	-c /home/jon/Working_Files/platanus-allee/assembly/out_contig.fa \
	/home/jon/Working_Files/platanus-allee/assembly/out_junctionKmer.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 60 \
	2>pcali_illumina_step20_phased.log

date

./platanus_allee consensus \
	-o pcali_illumina_consensus_step20
	-t 60 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step20_nonBubbleHomoCandidate.fa \
	/home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step20_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	2>consensus_pcali_illumina_step20.log

date 

```



recording memory usage once per a minute for 7 days or until I kill it
```bash
for i in `seq 0 10080`; do
  date >> test.txt
  echo `cat /proc/meminfo | grep Active: | sed 's/Active: //g'` >> test.txt 
  sleep 1m
done
```

TO-BE_RUN phasing the step10 assembly
```bash

#without the long read data

./platanus_allee phase \
	-o pcali_illumina_phased_step10 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_assembly_step10_contig.fa \
	/home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_step10_assembly/pcali_illumina_assembly_step10_junctionKmer.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 70 2>pcali_illumina_step10_phased.log

./platanus_allee consensus \
	 -t 70 \
	 -c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step10_nonBubbleHomoCandidate.fa \
	 /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step10_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	 2>consensus_pcali_step10.log

# with the long read data
./platanus_allee phase \
	-o pcali_illumina_nanopore_phased_step10 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_assembly_step10_contig.fa \
	/home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_step10_assembly/pcali_illumina_assembly_step10_junctionKmer.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	-t 70 2>pcali_illumina_nanopore_step10_phased.log

./platanus_allee consensus \
	 -t 70 \
	 -c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step10_nonBubbleHomoCandidate.fa \
	 /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step10_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	 2>consensus_pcali_illumina_nanopore_step10.log

```

# Fri 18 Sep 2020 04:25:18 PM PDT
phasing completed at 14:06:34 today consensus was started immeediately after


# Sun 20 Sep 2020 10:03:18 PM PDT

looks like the server crashed yesterday for whatever reason. Consensus for the illumina plus nanopore data finished, but phasing for only illumina did, sooo gonna restart that I guess. 

record memory usage and date for 7 days
```bash
for i in `seq 0 10080`; do
	a=$(date)
	a+=$(echo `cat /proc/meminfo | grep Active: | sed 's/Active: //g'`)	 
	echo $a >> memory_usage_illumina_phasing.txt
	sleep 1m
done
```

runing phasing on illumina step 20 

```bash
date >> platanus_allee_time.txt
echo "running platanus_allee phasing with illumina data only" >> platanus_allee_time.txt

./platanus_allee phase \
	-o pcali_illumina_phased_step20 \
	-c /home/jon/Working_Files/platanus-allee/assembly/out_contig.fa \
	/home/jon/Working_Files/platanus-allee/assembly/out_junctionKmer.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 60 \
	2>pcali_illumina_step20_phased.log

date >> platanus_allee_time.txt
echo "completed phasing and starting consensus" >> platanus_allee_time.txt

./platanus_allee consensus \
	-o pcali_illumina_consensus_step20 \
	-t 60 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step20_nonBubbleHomoCandidate.fa \
	/home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step20_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	2>consensus_pcali_illumina_step20.log

date >> platanus_allee_time.txt
echo "finished consensus" >> platanus_allee_time.txt

```

started on Sun 20 Sep 2020 10:46:46 PM PDT

note to self - next time just write the dates to a file with context

# Wed 23 Sep 2020 07:19:32 PM PDT

I was lame and missed a "nanopore" in one of the non-nanopore commands. Had to restart it. 

```bash
Sun 20 Sep 2020 10:51:34 PM PDT
running platanus_allee phasing with illumina data only
Wed 23 Sep 2020 02:05:36 PM PDT
completed phasing 
Wed 23 Sep 2020 06:51:01 PM PDT
starting consensus
Wed 23 Sep 2020 09:15:17 PM PDT
finished consensus
```

soooo it took 3ish days for phasing and 3 hrs for consensus. cool.
moving onto phasing/consensus for the step10 assembly

```bash

#running this as a bash script

#without the long read data

date >> platanus_allee_time.txt
echo "running platanus_allee phasing with illumina data only" >> platanus_allee_time.txt

./platanus_allee phase \
	-o pcali_illumina_phased_step10 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_step10_assembly/pcali_illumina_assembly_step10_contig.fa \
	/home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_step10_assembly/pcali_illumina_assembly_step10_junctionKmer.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-t 60 2>pcali_illumina_step10_phased.log

date >> platanus_allee_time.txt
echo "completed phasing and starting consensus" >> platanus_allee_time.txt

./platanus_allee consensus \
	 -t 60 \
	 -c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step10_nonBubbleHomoCandidate.fa \
	 /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_phased_step10_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	 2>pcali_consensus_step10.log

date >> platanus_allee_time.txt
echo "finished consensus" >> platanus_allee_time.txt


# with the long read data

date >> platanus_allee_time.txt
echo "running platanus_allee phasing with illumina and nanopore data" >> platanus_allee_time.txt

./platanus_allee phase \
	-o pcali_illumina_nanopore_phased_step10 \
	-c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_step10_assembly/pcali_illumina_assembly_step10_contig.fa \
	/home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_step10_assembly/pcali_illumina_assembly_step10_junctionKmer.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	-t 60 2>pcali_illumina_nanopore_step10_phased.log

date >> platanus_allee_time.txt
echo "completed phasing and starting consensus" >> platanus_allee_time.txt

./platanus_allee consensus \
	 -t 60 \
	 -c /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step10_nonBubbleHomoCandidate.fa \
	 /home/jon/Working_Files/platanus-allee/Platanus_allee_v2.0.2/pcali_illumina_nanopore_phased_step10_primaryBubble.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_1.trim.fq /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/pcali_2.trim.fq \
	-p /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq \
	 2>pcali_consensus_illumina_nanopore_step10.log

date >> platanus_allee_time.txt
echo "finished consensus" >> platanus_allee_time.txt

```

recording memory usage for 7 days - although this will probably take two weeks
```bash
for i in `seq 0 10080`; do
	a=$(date)
	a+=$(echo `cat /proc/meminfo | grep Active: | sed 's/Active: //g'`)	 
	echo $a >> memory_usage_illumina_nanopore_step10_phasing.txt
	sleep 1m
done
```


# Tue 29 Sep 2020 02:59:13 PM PDT

looks like there was another power interruption on sunday. thankfully one of the phase/consensus steps finished

```bash
Thu 24 Sep 2020 12:24:33 AM PDT
running platanus_allee phasing with illumina data only
Sat 26 Sep 2020 02:07:16 PM PDT
completed phasing and starting consensus
Sat 26 Sep 2020 04:26:16 PM PDT
finished consensus
```

rerunning the illumina plus nanopore step 10 phasing - phasing failed


# Mon 05 Oct 2020 10:21:03 PM PDT

ok, taking another look at masurca results and deciding if it is time to move onto genome assembly comparisons and qc. 

