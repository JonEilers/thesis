
> previously used command is below 

bbduk.sh in=out_consensusScaffold.fa out=genome.filter.fa minlen=3000

# Mon 02 Mar 2020 10:21:58 PM PST

## filtering redundans pcali v ajap assembly to remove contigs <2500 bp and running stats and busco on it

```
bbduk.sh in=/home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked out=pcali_ajap.redundans.platanus.filter-2.5k.fa minlen=2500

Input:                  	304189 reads 		878550047 bases.
Total Removed:          	278656 reads (91.61%) 	169970912 bases (19.35%)
Result:                 	25533 reads (8.39%) 	708579135 bases (80.65%)

Time:                         	12.790 seconds.
Reads Processed:        304k 	23.78k reads/sec
Bases Processed:        878m 	68.69m bases/sec
```

### stats
```
stats.sh pcali_ajap.redundans.platanus.filter-2.5k.fa

A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3151	0.1849	0.1850	0.3149	0.3134	0.0000	0.0000	0.3700	0.0229

Main genome scaffold total:         	25533
Main genome contig total:           	62147
Main genome scaffold sequence total:	708.579 MB
Main genome contig sequence total:  	486.496 MB  	31.342% gap
Main genome scaffold N/L50:         	609/337.628 KB
Main genome contig N/L50:           	7271/14.798 KB
Main genome scaffold N/L90:         	6008/6.049 KB
Main genome contig N/L90:           	38575/3.204 KB
Max scaffold length:                	2.355 MB
Max contig length:                  	342.646 KB
Number of scaffolds > 50 KB:        	2184
% main genome in scaffolds > 50 KB: 	84.06%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	        25,533	        62,147	   708,579,135	   486,496,492	  68.66%
   1 KB 	        25,533	        62,147	   708,579,135	   486,496,492	  68.66%
 2.5 KB 	        25,533	        62,147	   708,579,135	   486,496,492	  68.66%
   5 KB 	         8,199	        44,731	   649,692,467	   427,636,002	  65.82%
  10 KB 	         3,280	        39,741	   617,382,791	   395,449,317	  64.05%
  25 KB 	         2,450	        38,709	   606,035,901	   385,142,600	  63.55%
  50 KB 	         2,184	        37,710	   595,619,844	   379,278,395	  63.68%
 100 KB 	         1,663	        34,538	   558,268,199	   358,924,679	  64.29%
 250 KB 	           862	        25,459	   428,192,419	   283,136,250	  66.12%
 500 KB 	           328	        13,594	   237,332,936	   161,594,785	  68.09%
   1 MB 	            28	         2,024	    36,623,048	    28,079,209	  76.67%
```

### busco

```
busco -c 75 -m genome -i pcali_ajap.redundans.platanus.filter-2.5k.fa -o euk_busco_pcali_ajap.redundans.platanus.filter-2.5k.fa -l eukaryota_odb10

busco -c 75 -m genome -i pcali_ajap.redundans.platanus.filter-2.5k.fa -o metazoa_busco_pcali_ajap.redundans.platanus.filter-2.5k.fa -l metazoa_odb10

	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:79.2%[S:75.3%,D:3.9%],F:12.9%,M:7.9%,n:255     |
	|202	Complete BUSCOs (C)                       |
	|192	Complete and single-copy BUSCOs (S)       |
	|10	Complete and duplicated BUSCOs (D)        |
	|33	Fragmented BUSCOs (F)                     |
	|20	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------

	--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:84.2%[S:75.4%,D:8.8%],F:8.5%,M:7.3%,n:954      |
	|803	Complete BUSCOs (C)                       |
	|719	Complete and single-copy BUSCOs (S)       |
	|84	Complete and duplicated BUSCOs (D)        |
	|81	Fragmented BUSCOs (F)                     |
	|70	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------


```

## filtering redundans pcali v ajap assembly to remove contigs <1000 bp and running stats and busco on it 

soooo I already did the filtering part previously. just running stats on it now. 

```

stats.sh pcali_ajap.redundans.platanus.masked.filter-1k.fa

A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3148	0.1853	0.1854	0.3146	0.2813	0.0000	0.0000	0.3706	0.0291

Main genome scaffold total:         	77404
Main genome contig total:           	114148
Main genome scaffold sequence total:	789.640 MB
Main genome contig sequence total:  	567.534 MB  	28.128% gap
Main genome scaffold N/L50:         	739/290.681 KB
Main genome contig N/L50:           	10505/10.695 KB
Main genome scaffold N/L90:         	26381/2.449 KB
Main genome contig N/L90:           	69440/1.766 KB
Max scaffold length:                	2.355 MB
Max contig length:                  	342.646 KB
Number of scaffolds > 50 KB:        	2184
% main genome in scaffolds > 50 KB: 	75.43%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	        77,404	       114,148	   789,640,472	   567,534,088	  71.87%
    500 	        77,404	       114,148	   789,640,472	   567,534,088	  71.87%
   1 KB 	        77,404	       114,148	   789,640,472	   567,534,088	  71.87%
 2.5 KB 	        25,533	        62,147	   708,579,135	   486,496,492	  68.66%
   5 KB 	         8,199	        44,731	   649,692,467	   427,636,002	  65.82%
  10 KB 	         3,280	        39,741	   617,382,791	   395,449,317	  64.05%
  25 KB 	         2,450	        38,709	   606,035,901	   385,142,600	  63.55%
  50 KB 	         2,184	        37,710	   595,619,844	   379,278,395	  63.68%
 100 KB 	         1,663	        34,538	   558,268,199	   358,924,679	  64.29%
 250 KB 	           862	        25,459	   428,192,419	   283,136,250	  66.12%
 500 KB 	           328	        13,594	   237,332,936	   161,594,785	  68.09%
   1 MB 	            28	         2,024	    36,623,048	    28,079,209	  76.67%

```

```
busco -c 75 -m genome -i pcali_ajap.redundans.platanus.masked.filter-1k.fa -o euk_busco_pcali_ajap.redundans.platanus.masked.filter-1k.fa -l eukaryota_odb10

busco -c 75 -m genome -i pcali_ajap.redundans.platanus.masked.filter-1k.fa -o metazoa_busco_pcali_ajap.redundans.platanus.masked.filter-1k.fa -l metazoa_odb10

	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:79.2%[S:74.9%,D:4.3%],F:12.5%,M:8.3%,n:255     |
	|202	Complete BUSCOs (C)                       |
	|191	Complete and single-copy BUSCOs (S)       |
	|11	Complete and duplicated BUSCOs (D)        |
	|32	Fragmented BUSCOs (F)                     |
	|21	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------

		--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:84.5%[S:75.8%,D:8.7%],F:9.0%,M:6.5%,n:954      |
	|806	Complete BUSCOs (C)                       |
	|723	Complete and single-copy BUSCOs (S)       |
	|83	Complete and duplicated BUSCOs (D)        |
	|86	Fragmented BUSCOs (F)                     |
	|62	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------
```

# Tue 03 Mar 2020 11:44:04 AM PST

I guess I forgot to create a 1000bp filtered non-masked redundans assembly. gonna fix that now. 
```
bbduk.sh in=/home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta out=pcali_ajap.redundans.platanus.filter-1k.fasta minlen=1000

Input:                  	304189 reads 		878550047 bases.
Total Removed:          	226785 reads (74.55%) 	88909575 bases (10.12%)
Result:                 	77404 reads (25.45%) 	789640472 bases (89.88%)
```

# Wed 18 Mar 2020 05:06:19 PM PDT
indexing the fasta file

```
samtools faidx /home/jon/Working_Files/pcali_ajap.redundans.platanus.filter-1k.fasta -o /home/jon/Working_Files/pcali_ajap.redundans.platanus.filter-1k.fasta.fai
```

mkdir hdf5 \
wget http://www.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-1.10.1/src/hdf5-1.10.1.tar.gz \
tar xzf hdf5-1.10.1.tar.gz \
cd hdf5-1.10.1 \
./configure --enable-cxx --prefix hdf5 \
make && make install