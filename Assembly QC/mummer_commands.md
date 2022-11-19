## mummer4 commands

these were commands from a previous run
```
nucmer -t 70 -p p_cali_nucmer A_jap_genome.fasta Patanus-Allee/Platanus_allee_v2.0.2/out_consensusScaffold.fa

mummerplot -p p_cali_nucmer p_cali_nucmer.delta 

dnadiff -d ../p_cali_nucmer.delta
```

## mummer4 japonicus vs cali using filtered genome

```
nucmer -t 35 -p pcali_filtered_nucmer ../A_jap_genome.fasta ../genome.filter.fa 
mummerplot -l -s --filter large -p pcali_filtered_nucmer p_cali_nucmer.delta
dnadiff -d pcali_filtered_nucmer.delta

```

the mummerplot is throwing this error and not producing an image. however the gp file is produced
```
filtered_nucmer.delta
gnuplot 5.2 patchlevel 7
Writing filtered delta file p_cali_nucmer.filter
Reading delta file p_cali_nucmer.filter
Writing plot files p_cali_nucmer.fplot, p_cali_nucmer.rplot
Writing gnuplot script p_cali_nucmer.gp
Forking mouse listener
Rendering plot to screen
WARNING: Unable to run 'false -geometry 900x900+0+0 -title mummerplot p_cali_nucmer.gp', Inappropriate ioctl for device
```

found that the mummerplot python file needed to have the gnuplot variable set on line 27. via an issue on github
gnuplot started an interactive session, but there was too much noise in the image. 

## delta-filter 

so when the png images are generated therei s too much noise. So maybe a little filtering will up.

```
delta-filter -l 10000 -q -r pcali_filtered_nucmer.delta

mummerplot -l -p p_cali_nucmer pcali_deltafiltered_nucmer.delta
```

it sorta worked, but still unintelligible 

## nucdiff

nucdiff --delta_file pcali_filtered_nucmer.delta ../../A_jap_genome.fasta ../../out_consensusScaffold.fa /nucdiff pcali_nucdiff

# Mon 24 Feb 2020 10:23:35 PM PST

whelp, I am not sure when and what I did above. specifically what filtered genome I used, but I am assuming it was the 3kbp one. However, I need to run the ajap genome against the redundans and the platanus_allee genome assemblies to get some stats. soooo, time to do it. 

mummer4 with ajap genome vs platanus_allee assembly
```

nucmer -t 70 -p pcali-platanus_v_ajap /home/jon/Working_Files/Ajap_genome.fasta /home/jon/Working_Files/pcali_genome.platanus.fa

dnadiff -d /home/jon/Working_Files/mummer/pcali-platanus_v_ajap.delta
```

mummer4 with ajap genome vs platanus_allee_redundans assembly
```

nucmer -t 70 -p pcali-platanus-redundans_v_ajap /home/jon/Working_Files/Ajap_genome.fasta /home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta

dnadiff -d /home/jon/Working_Files/mummer/pcali-platanus-redundans_v_ajap.delta
```

interesting results. I think I should filter reads less than 1kbp again and run these again. but that will be for another day



# Sat 16 May 2020 12:10:40 AM PDT

running mummer4 on all the assemblies. Focus on comparing the assemblies to each and to parv and ajap

```bash
for i in *".fa"

do
	echo "running nucmer with ajap vs" $i 
	nucmer -t 70 -p  ajap2"$i" /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/ajap_assembly.fa $i
	nucmer -t 70 -p  parv2"$i" /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pparv_assembly.fa $i
done

nucmer -t 70 -p  redun2plat /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_ajap.redundans.platanus.fa /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_genome.platanus.fa

nucmer -t 70 -p  plat2masurca /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_genome.platanus.fa /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_genome.masurca.fa

nucmer -t 70 -p  plat2plat1000k /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_genome.platanus.fa /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_ajap.redundans.platanus.filter-1k.fa

```


running nucdiff on a delta file
```bash
nucdiff --delta_file /home/jon/Working_Files/mummer/genome_assembly_comparison/ajap_pcali-plat/ajap2plat.delta \
		--proc 5 \
		 /home/jon/Working_Files/mummer/genome_assembly_comparison/ajap_pcali-plat/ajap_assembly.fa \
		 /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_genome.platanus.fa \
		 /home/jon/Working_Files/mummer/genome_assembly_comparison/ajap_pcali-plat \
		 ajap2plat
```

this is an interesting quirk. so I moved the assemblies to the mummer folder for running this, which is what I used in the nucdiff command, but it complains if it can't the file in the location where it was originally run. 
```bash
Run NUCmer...


Find differences...

ERROR:  Could not open file  /home/jon/Working_Files/blobtoolkit/genome_assemblies/ajap_assembly.fa 
ERROR:  Could not open file  /home/jon/Working_Files/blobtoolkit/genome_assemblies/ajap_assembly.fa 
```

yup, it is in the header of the delta file
```bash
/home/jon/Working_Files/blobtoolkit/genome_assemblies/ajap_assembly.fa /home/jon/Working_Files/blobtoolkit/genome_assemblies/pcali_ajap.redundans.platanus.fa
NUCMER
>MRZV01000114.1 scaffold00064 1116238 622954
2235 4464 1 2229 211 211 0
171
1
```

and threw this error
```bash
Traceback (most recent call last):
  File "/home/jon/anaconda3/envs/nucdiff/bin/nucdiff", line 6, in <module>
    from nucdiff.nucdiff import main
  File "/home/jon/anaconda3/envs/nucdiff/lib/python3.8/site-packages/nucdiff/nucdiff.py", line 103, in <module>
    main()
  File "/home/jon/anaconda3/envs/nucdiff/lib/python3.8/site-packages/nucdiff/nucdiff.py", line 101, in main
    START(args)
  File "/home/jon/anaconda3/envs/nucdiff/lib/python3.8/site-packages/nucdiff/nucdiff.py", line 58, in START
    struct_dict,end_err_dict,unmapped_list,uncovered_dict=FIND_ERRORS_ASSEMBLY(file_ref,file_contigs, working_dir, nucmer_opt, prefix,proc_num, filter_opt,delta_file,reloc_dist,coord_file ) #class Errors
  File "/home/jon/anaconda3/envs/nucdiff/lib/python3.8/site-packages/nucdiff/find_errors.py", line 33, in FIND_ERRORS_ASSEMBLY
    nuc.FIND_ERR_INSIDE_FRAG(proc_num,file_contigs)
  File "/home/jon/anaconda3/envs/nucdiff/lib/python3.8/site-packages/nucdiff/class_nucmer.py", line 185, in FIND_ERR_INSIDE_FRAG
    seq=contigs_dict[cont_name][start_seq-1 :end_seq-1   +1]
KeyError: 'scaffold00001'
```

# Sat 16 May 2020 09:30:54 AM PDT

okkkk, now onto dnadiff for those delta files
```bash
for i in *".delta"

do
	d=$(echo "$i" | cut -f 1 -d '.')
	mkdir $d
	cd $d
	dnadiff -d ../$i
	cd ..
done
```

fun quirk - if the name of assemblies don't match the name in the file header, dnadiff complains. lol



# Mon 25 May 2020 04:30:39 PM PDT

trying nucdiff again

```bash
nucdiff --delta_file /home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/delta_files/ajap2pcali_genome_platanus.fa.delta \
	--proc 20 \
	/home/jon/Working_Files/mummer/genome_assembly_comparison/ajap_pcali-plat/ajap_assembly.fa \
	/home/jon/Working_Files/mummer/genome_assembly_comparison/genome_assemblies/pcali_genome_platanus.fa \
	/home/jon/Working_Files/mummer/genome_assembly_comparison/ajap_pcali-plat \
	ajap2pcali_genome_platanus.fa
```

# Sun 25 Apr 2021 12:10:57 PM PDT

ok, here I am again...lol

running mummer4 to compare:
Reference	Query
ajap 		plat-10
Ajap 		plat-20
Ajap 		redun-plat-10
Ajap 		masurca-yes-ref,link
Ajap 		masurca-no-red,link
plat-10		plat-20
plat-10		redun-plat-10


/home/jon/Working_Files/genome_assemblies/californicus_genomes/masurca_nanopore_illumina_yes-reference_yes-link.fa

/home/jon/Working_Files/genome_assemblies/californicus_genomes/redundans_platanus-allee-with-nanopore_step20_ajap-reference_no-nanopore.fa

/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_nanopore_step20.fa





/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta


```bash
# ajap 		plat-10
nucmer \
	-t 70 \
	-p plat10_2_ajap \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step10.fa

# Ajap 		plat-20
nucmer \
	-t 70 \
	-p plat20_2_ajap \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20.fa

# Ajap 		redun-plat-10
nucmer \
	-t 70 \
	-p redun-plat-10_2_ajap \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/redundans_platanus-allee_step10_ajap-reference.fa

# Ajap 		masurca-yes-ref,link
nucmer \
	-t 70 \
	-p masurca-yes-ref_2_ajap \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/masurca_illumina_yes-reference_yes-link.fa

# Ajap 		masurca-no-red,link
nucmer \
	-t 70 \
	-p masurca-no-ref_2_ajap \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/masurca_illumina_no-reference_yes-link.fa

# plat-10		plat-20
nucmer \
	-t 70 \
	-p plat20_2_plat10 \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step10.fa \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20.fa

# plat-10		redun-plat-10
nucmer \
	-t 70 \
	-p redun-plat-10_2_plat-10 \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step10.fa \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/redundans_platanus-allee_step10_ajap-reference.fa





```



Note: run promer on the protein gene models between the various runs/ajap


nucdiff to deal with later
```bash

nucdiff \
	--delta_file /home/jon/Working_Files/mummer/plat10_2_ajap.delta \
	--proc 70 \
	/home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
	/home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step10.fa \
	/home/jon/Working_Files/mummer/nucdiff \
	plat10_2_ajap


```