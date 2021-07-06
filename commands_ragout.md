# Mon 08 Jun 2020 12:03:08 PM PDT

runninng ragout wiht the progressive cactus output

requires a recipe file. example below is from github
```bash
#reference and target genome names (required)
.references = rf123,col,jkd,n315
.target = usa

#phylogenetic tree for all genomes (optional)
.tree = (rf122:0.02,(((usa:0.01,col:0.01):0.01,jkd:0.04):0.005,n315:0.01):0.01);

#paths to genome fasta files (required for Sibelia)
col.fasta = references/COL.fasta
jkd.fasta = references/JKD6008.fasta
rf122.fasta = references/RF122.fasta
n315.fasta = references/N315.fasta
usa.fasta = usa300_contigs.fasta

#synteny blocks scale (optional)
.blocks = small

#reference to use for scaffold naming (optional)
.naming_ref = rf122
```

for hal alignment
```bash
.references = miranda,simulans,melanogaster
.target = yakuba

#HAL alignment input. Sequences will be extracted from the alignment
.hal = genomes/alignment.hal
```

actual recipe file. note, pcali_contigs was misnamed, too late now. It used the pcali_platanus full genome assembly
```bash
#reference and target genome names (required)
.references = ajap,pparv
.target = pcali_contigs 

#HAL alignment input. Sequences will be extracted from the alignment
.hal = /home/jon/Working_Files/cactus/cuke.hal


*.draft = true
```

```bash
ragout -o /home/jon/Working_Files/ragout -s hal --refine --repeats -t 70 /home/jon/Working_Files/ragout/ragout_recipe_pcali.txt
```

throws this error
```bash
[13:31:09] INFO: Starting Ragout v2.3
Traceback (most recent call last):
  File "/home/jon/anaconda3/envs/ragout/bin/ragout", line 11, in <module>
    load_entry_point('ragout==2.3', 'console_scripts', 'ragout')()
  File "/home/jon/anaconda3/envs/ragout/lib/python3.7/site-packages/ragout/main.py", line 295, in main
    _run_ragout(args)
  File "/home/jon/anaconda3/envs/ragout/lib/python3.7/site-packages/ragout/main.py", line 172, in _run_ragout
    synteny_sizes = _get_synteny_scale(recipe, synteny_backend)
  File "/home/jon/anaconda3/envs/ragout/lib/python3.7/site-packages/ragout/main.py", line 145, in _get_synteny_scale
    scale = config.vals["blocks"][synteny_backend.infer_block_scale(recipe)]
  File "/home/jon/anaconda3/envs/ragout/lib/python3.7/site-packages/ragout/synteny_backend/hal.py", line 43, in infer_block_scale
    tokens = line.split(",")
TypeError: a bytes-like object is required, not 'str'
```

This is a known problem and is an open bug on github. one fix is to convert to a maf soooo
```bash
hal2maf cuke.hal cuke.maf
```

# Tue 09 Jun 2020 01:23:17 PM PDT

whelp, that took awhile. trying to run ragout again

```bash
ragout -o /home/jon/Working_Files/ragout -s maf --refine --repeats -t 70 /home/jon/Working_Files/ragout/ragout_recipe_pcali.txt
```

wow, that was fast
```bash
[13:34:42] INFO: Starting Ragout v2.3
[13:34:42] INFO: Running withs synteny block sizes '[10000, 500, 100]'
[13:34:42] WARNING: Maf support is deprecated and will be removed in future releases. Use hal istead.
[13:34:42] INFO: Converting MAF to synteny
[13:34:42] INFO: Running maf2synteny module
	Reading maf file
	Started initial compression
	Simplification with 30 500
	Simplification with 100 5000
	Simplification with 500 50000
	Simplification with 5000 500000
[14:21:38] INFO: Inferring phylogeny from synteny blocks data
[14:21:38] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/100/blocks_coords.txt
[14:22:01] INFO: "pparv" synteny blocks coverage: 78.79%
[14:22:02] INFO: "ajap" synteny blocks coverage: 82.06%
[14:22:03] INFO: "pcali_contigs" synteny blocks coverage: 83.98%
[14:23:39] INFO: Inferred tree: ('pparv' : 10897.5, ('ajap' : 30458.0, 'pcali_contigs' : 11999.0) : 10897.5)
[14:23:39] INFO: 'pparv' is chosen as a naming reference
[14:23:39] INFO: Processing permutation files
[14:23:39] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/10000/blocks_coords.txt
[14:23:39] INFO: "pparv" synteny blocks coverage: 23.1%
[14:23:39] INFO: "ajap" synteny blocks coverage: 11.36%
[14:23:39] INFO: "pcali_contigs" synteny blocks coverage: 53.7%
[14:23:39] WARNING: Some genomes have low synteny block coverage - results can be inaccurate. Possible reasons: 
	1. Genome is too distant from others
	2. Synteny block parameters are chosen incorrectly
	Try to change synteny blocks paramsters or remove this genome from the comparison
[14:23:40] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/500/blocks_coords.txt
[14:23:49] INFO: "pparv" synteny blocks coverage: 66.19%
[14:23:49] INFO: "ajap" synteny blocks coverage: 67.11%
[14:23:50] INFO: "pcali_contigs" synteny blocks coverage: 71.15%
[14:24:22] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/100/blocks_coords.txt
[14:24:47] INFO: "pparv" synteny blocks coverage: 78.79%
[14:24:48] INFO: "ajap" synteny blocks coverage: 82.06%
[14:24:49] INFO: "pcali_contigs" synteny blocks coverage: 83.98%
[14:26:21] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/100/blocks_coords.txt
[14:26:49] INFO: "pparv" synteny blocks coverage: 78.79%
[14:26:50] INFO: "ajap" synteny blocks coverage: 82.06%
[14:26:51] INFO: "pcali_contigs" synteny blocks coverage: 83.98%
[14:27:49] INFO: Resolving repeats
[14:38:02] INFO: Reading contigs file
[14:38:29] INFO: Detecting chimeric adjacencies
[14:39:58] INFO: Stage "10000"
[14:39:58] INFO: Removing chimeric adjacencies
[14:39:58] INFO: Inferring missing adjacencies
[14:39:59] INFO: Stage "500"
[14:39:59] INFO: Removing chimeric adjacencies
[14:40:02] INFO: Inferring missing adjacencies
[14:40:20] INFO: Removing chimeric adjacencies
[14:40:23] INFO: Merging two iterations
[14:40:38] INFO: Stage "100"
[14:40:38] INFO: Removing chimeric adjacencies
[14:40:48] INFO: Inferring missing adjacencies
[14:41:18] INFO: Removing chimeric adjacencies
[14:41:23] INFO: Merging two iterations
[14:42:22] INFO: Stage "refine"
[14:42:22] INFO: Removing chimeric adjacencies
[14:42:57] INFO: Inferring missing adjacencies
[14:44:58] INFO: Removing chimeric adjacencies
[14:45:22] INFO: Merging two iterations
[14:45:31] INFO: Building assembly graph
	Reading FASTA
	Building FM-index
	Overapping
	Kmer size is set to 33
[14:52:25] INFO: Refining with assembly graph
[14:55:03] INFO: Generating FASTA output
[14:55:05] INFO: Assembly statistics:

	Scaffolds:		306
	Used fragments:		6115
	Scaffolds length:	106639993

	Unplaced fragments:	678964
	Unplaced length:	765851219 (91.30%)
	Introduced Ns length:	33521794 (31.43%)

	Fragments N50:		8569
	Assembly N50:		552435

[14:55:05] INFO: Done!
```

huh, not a lot of long scaffolds. gonna by removing the ajap assembly from the analysis

```bash
ragout -o /home/jon/Working_Files/ragout -s maf --refine --overwrite --repeats -t 70 /home/jon/Working_Files/ragout/ragout_recipe_pcali.txt
```

results
```bash
[17:02:18] INFO: Starting Ragout v2.3
[17:02:18] INFO: Running withs synteny block sizes '[10000, 500, 100]'
[17:02:18] WARNING: Maf support is deprecated and will be removed in future releases. Use hal istead.
[17:02:18] INFO: Converting MAF to synteny
[17:02:18] INFO: Running maf2synteny module
	Reading maf file
	Started initial compression
	Simplification with 30 500
	Simplification with 100 5000
	Simplification with 500 50000
	Simplification with 5000 500000
[17:49:35] INFO: Inferring phylogeny from synteny blocks data
[17:49:35] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/100/blocks_coords.txt
[17:49:58] INFO: "pparv" synteny blocks coverage: 78.79%
[17:49:59] INFO: "pcali_contigs" synteny blocks coverage: 83.98%
[17:51:09] INFO: Inferred tree: ('pcali_contigs' : 11415.5, 'pparv' : 11415.5)
[17:51:09] INFO: 'pparv' is chosen as a naming reference
[17:51:10] INFO: Processing permutation files
[17:51:10] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/10000/blocks_coords.txt
[17:51:10] INFO: "pparv" synteny blocks coverage: 23.1%
[17:51:10] INFO: "pcali_contigs" synteny blocks coverage: 53.7%
[17:51:10] WARNING: Some genomes have low synteny block coverage - results can be inaccurate. Possible reasons: 
	1. Genome is too distant from others
	2. Synteny block parameters are chosen incorrectly
	Try to change synteny blocks paramsters or remove this genome from the comparison
[17:51:10] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/500/blocks_coords.txt
[17:51:19] INFO: "pparv" synteny blocks coverage: 66.19%
[17:51:19] INFO: "pcali_contigs" synteny blocks coverage: 71.15%
[17:51:46] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/100/blocks_coords.txt
[17:52:11] INFO: "pparv" synteny blocks coverage: 78.79%
[17:52:12] INFO: "pcali_contigs" synteny blocks coverage: 83.98%
[17:53:28] INFO: Reading /home/jon/Working_Files/ragout/maf-workdir/100/blocks_coords.txt
[17:53:54] INFO: "pparv" synteny blocks coverage: 78.79%
[17:53:55] INFO: "pcali_contigs" synteny blocks coverage: 83.98%
[17:54:37] INFO: Resolving repeats
[17:59:49] INFO: Reading contigs file
[18:00:17] INFO: Detecting chimeric adjacencies
^[[A[18:02:40] INFO: Stage "10000"
[18:02:40] INFO: Removing chimeric adjacencies
[18:02:40] INFO: Inferring missing adjacencies
[18:02:40] INFO: Stage "500"
[18:02:40] INFO: Removing chimeric adjacencies
[18:02:47] INFO: Inferring missing adjacencies
[18:03:12] INFO: Removing chimeric adjacencies
[18:03:21] INFO: Merging two iterations
[18:03:31] INFO: Stage "100"
[18:03:31] INFO: Removing chimeric adjacencies
[18:03:53] INFO: Inferring missing adjacencies
[18:04:49] INFO: Removing chimeric adjacencies
[18:05:02] INFO: Merging two iterations
[18:05:33] INFO: Stage "refine"
[18:05:33] INFO: Removing chimeric adjacencies
[18:06:05] INFO: Inferring missing adjacencies
[18:07:38] INFO: Removing chimeric adjacencies
[18:08:02] INFO: Merging two iterations
[18:08:05] INFO: Building assembly graph
	Reading FASTA
	Building FM-index
	Overapping
	Kmer size is set to 33
[18:14:57] INFO: Refining with assembly graph
[18:17:19] INFO: Generating FASTA output
[18:17:20] INFO: Assembly statistics:

	Scaffolds:		218
	Used fragments:		1477
	Scaffolds length:	21668262

	Unplaced fragments:	683495
	Unplaced length:	819470894 (97.69%)
	Introduced Ns length:	2315524 (10.69%)

	Fragments N50:		8569
	Assembly N50:		104613

[18:17:20] INFO: Done!
```

Even worse, wow. I think I will rerun progressive cactus with the contigs and see what happens? Actually, originally I set them all to be outgroups, but this time I will run ajap as the only outgroup as it is the most contigious and also is the actual outgroup. 

# Thu 11 Jun 2020 12:50:50 PM PDT

running ragout on a new maf file which used only the ajap as the outgroup for aligning.

```bash
ragout -o /home/jon/Working_Files/ragout -s maf --refine --overwrite --repeats -t 70 /home/jon/Working_Files/ragout/ragout_recipe_pcali.txt
```