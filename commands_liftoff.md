# Sun 25 Apr 2021 01:42:40 PM PDT

using Liftoff to try annotating the pcali genome using the ajap genome. I am gonna laugh if this turns out to be better
```bash
# ajap 2 pcali plat-allee 10 
liftoff \
	-p 70 \
	-o ajap_2_pcali_liftoff \
	-u unmapped \
	-g /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.gff \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step10.fa \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta

# ajap 2 pcali redun plat 10
liftoff \
	-p 40 \
	-o ajap_2_redun_pcali_liftoff \
	-u unmapped \
	-g /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.gff \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/redundans_platanus-allee_step10_ajap-reference.fa \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta 
```


liftoff -p 70 -o ajap_2_pcali_liftoff -u unmapped -g /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.gff /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta /home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step10.fa