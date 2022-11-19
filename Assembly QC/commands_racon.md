
# Thu 10 Dec 2020 08:13:34 PM PST

setting up racon for polishing assemblies. 

```bash
git clone --recursive https://github.com/lbcb-sci/racon.git racon
cd racon
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make
```

ookkk, time to try racon
```bash
./racon -u -t 70 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq /home/jon/Working_Files/genome_assemblies/californicus_genomes/ont2ragout_aln.sam /home/jon/Working_Files/genome_assemblies/californicus_genomes/pcali_ragout.fa > /home/jon/Working_Files/genome_assemblies/californicus_genomes/pcali_ragout_racon-nanopore.fa
```

huh, so it definitely improved the assembly a little. Largest read became 2mb. might try it on some of the other assemblies. 

will try it on the platanus assemblies
```bash
#running minimap with the nanopore data
minimap2 \
	-t 70 \
	-ax map-ont \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20.fa \
	/home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq > ont2platanus-allee_illumina_step20_aln.sam
```

success
```bash
[M::mm_idx_gen::34.707*1.58] collected minimizers
[M::mm_idx_gen::39.255*3.25] sorted minimizers
[M::main::39.571*3.23] loaded/built the index for 684764 target sequence(s)
[M::mm_mapopt_update::41.142*3.15] mid_occ = 379
[M::mm_idx_stat] kmer size: 15; skip: 10; is_hpc: 0; #seq: 684764
[M::mm_idx_stat::41.881*3.11] distinct minimizers: 50655411 (53.97% are singletons); average occurrences: 3.068; average spacing: 5.397
[M::worker_pipeline::46.314*5.86] mapped 21169 sequences
[M::main] Version: 2.17-r941
[M::main] CMD: minimap2 -t 70 -ax map-ont /home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20.fa /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq
[M::main] Real time: 46.988 sec; CPU: 271.897 sec; Peak RSS: 7.517 GB
```

running racon with that. 
```bash
./racon -u -t 70 /home/jon/Working_Files/sea_cuke_species_data/pcali_raw_data/nanopore_data.filter200.trimmed90.fastq /home/jon/Working_Files/genome_assemblies/californicus_genomes/ont2platanus-allee_illumina_step20_aln.sam /home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20.fa > /home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20_racon-nanopore.fa
```
