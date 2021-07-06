# Thu 30 Apr 2020 12:21:56 PM PDT

### installation

```bash
conda create -n btk_env -c conda-forge -y python=3.6 docopt pyyaml ujson tqdm nodejs=10 yq;
conda activate btk_env;
conda install -c bioconda -y pysam seqtk;
conda install -c conda-forge -y geckodriver selenium pyvirtualdisplay;
pip install fastjsonschema;

#fetch code
mkdir -p ~/blobtoolkit;
cd ~/blobtoolkit;
git clone https://github.com/blobtoolkit/blobtools2;
git clone https://github.com/blobtoolkit/viewer;
git clone https://github.com/blobtoolkit/specification;
git clone https://github.com/blobtoolkit/insdc-pipeline;

#install viewer package
cd viewer;
npm install;
cd ..;

# Fetch ncbi taxdump
mkdir -p taxdump;
cd taxdump;
curl -L ftp://ftp.ncbi.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz | tar xzf -;
cd ..;

# fetch nt database
mkdir -p nt_v5
wget "ftp://ftp.ncbi.nlm.nih.gov/blast/db/v5/nt.??.tar.gz" -P nt_v5/ && \
        for file in nt_v5/*.tar.gz; \
            do tar xf $file -C nt_v5 && rm $file; \
        done

# Fetch and make uniprot database
mkdir -p uniprot

# This command is really weird and I don't understand it. curl is basically wget and the ftp urls are the same
wget -q -O uniprot/reference_proteomes.tar.gz ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/($curl -vs ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/ 2>&1 | awk '/tar.gz/ {print $9}')


cd uniprot
tar xf reference_proteomes.tar.gz

# extract and concatenate protein FASTA files
touch reference_proteomes.fasta.gz
find . -mindepth 2 | grep "fasta.gz" | grep -v 'DNA' | grep -v 'additional' | xargs cat >> reference_proteomes.fasta.gz

# extract and concatenate taxid mapping files
cd Reference_Proteomes_2019_11
echo "accession\taccession.version\ttaxid\tgi" >> reference_proteomes.taxid_map
mv reference_proteomes.taxid_map ../
zcat */*.idmapping.gz | grep "NCBI_TaxID" | awk '{print $1 "\t" $1 "\t" $3 "\t" 0}' >> reference_proteomes.taxid_map

# make diamond blast db with taxonomic information included
diamond makedb -p 16 --in reference_proteomes.fasta.gz --taxonmap reference_proteomes.taxid_map --taxonnodes ../taxdump/nodes.dmp -d reference_proteomes.dmnd
cd -

mkdir -p busco
wget -q -O eukaryota_odb9.gz "https://busco.ezlab.org/datasets/eukaryota_odb9.tar.gz" \
        && tar xf eukaryota_odb9.gz -C busco
```

# Sat 02 May 2020 10:14:16 AM PDT

get this error when decompressing. I think this means uniprot did not completely download. ballz. I also just delete the uniprot that I used with the previous blobtools. 
```bash
gzip: stdin: unexpected end of file
tar: Unexpected EOF in archive
tar: Unexpected EOF in archive
tar: Error is not recoverable: exiting now
```

# Mon 04 May 2020 11:24:12 PM PDT

grabbed an older version of uniprot off an external hard drive and ran the above commands

version: Reference_Proteomes_2019_11

```bash
#blasting against the refseq nt databse
blastn \
 -query /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
 -db blast_db_nt/nt \
 -outfmt "6 qseqid staxids bitscore std" \
 -max_target_seqs 10 \
 -max_hsps 1 \
 -evalue 1e-25 \
 -num_threads 35

#blasting against the uniprot database
diamond blastx \
 --query /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
 --db reference_proteomes.dmnd \
 --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
 --sensitive \
 --max-target-seqs 1 \
 --evalue 1e-25 \
 --threads 35 

# coverage run
./snap-aligner index /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa index/ -t35
./snap-aligner paired index /home/jon/Working_Files/pcali_raw_data/P_cali_g_1.fq /home/jon/Working_Files/pcali_raw_data/P_cali_g_2.fq -t 70  -o -bam output.bam

#interesting result from snap indexing
58901910(7%) seeds occur more than once, total of 160254189(19%) genome locations are not unique, 262601605(31%) bad seeds, 0 both complements used 1548560 no string
Hash table build took 179s

# snap alignment results
Loading index from directory... 12s.  828342972 bases, seed size 20
Aligning.
Total Reads    Aligned, MAPQ >= 10    Aligned, MAPQ < 10     Unaligned              Too Short/Too Many Ns  %Pairs	Reads/s   Time in Aligner (s)
617,348,676    422,341,387 (68.41%)   71,137,829 (11.52%)    123,869,390 (20.06%)   70 (0.00%)             60.32%%	185,804   3,323

```

# Tue 05 May 2020 12:02:40 PM PDT

```bash
# unpacking the nt stuff
for file in blast_db_nt/*.tar.gz; \
    do tar xf $file -C blast_db_nt && rm $file; \
done


#blasting against the refseq nt databse
blastn \
 -query /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
 -db blast_db_nt/nt \
 -outfmt "6 qseqid staxids bitscore std" \
 -max_target_seqs 10 \
 -max_hsps 1 \
 -evalue 1e-25 \
 -num_threads 70 > nt.out.tab


# rerunning diamond blast because I forgot to write it to a file
diamond blastx \
 --query /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
 --db reference_proteomes.dmnd \
 --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
 --sensitive \
 --max-target-seqs 1 \
 --evalue 1e-25 \
 --threads 35 > diamond.out
```

lol, so apparently the blobtoolkit2 also for interactive filtering. Meaning that I should not have used the filtered genome assembly. Ohwell, might fix that later

threw error about bam file
```bash
./blobtools create --fasta /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa --cov /home/jon/Working_Files/blobtoolkit/coverage/output.bam --hits /home/jon/Working_Files/blobtoolkit/uniprot/diamond.out --hits /home/jon/Working_Files/blobtoolkit/nt_v5/nt.out.tab --taxdump /home/jon/Working_Files/blobtoolkit/taxdump tmp/dataset_1
Loading sequences from /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa
 - processing scaffold01047: : 77404it [01:05, 1184.66it/s] 
[E::hts_idx_push] NO_COOR reads not in a single block at the end 259 -1
Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 215, in parse
    fields = parse_bam(file, **kwargs, cov_range=cov_range, read_cov_range=read_cov_range)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 59, in parse_bam
    pysam.index(parts[0])
  File "/home/jon/anaconda3/envs/btk_env/lib/python3.6/site-packages/pysam/utils.py", line 75, in __call__
    stderr))
pysam.utils.SamtoolsError: 'samtools returned with error 1: stdout=, stderr=samtools index: failed to create index for "/home/jon/Working_Files/blobtoolkit/coverage/output.bam"\n'
```

removed the --cov option and will add later
rerunning without it
```bash
./blobtools create --fasta /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa  --hits /home/jon/Working_Files/blobtoolkit/uniprot/diamond.out --hits /home/jon/Working_Files/blobtoolkit/nt_v5/nt.out.tab --taxdump /home/jon/Working_Files/blobtoolkit/taxdump tmp/dataset_1
```

oooook, more errors
```bash
./blobtools create --fasta /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa  --hits /home/jon/Working_Files/blobtoolkit/uniprot/diamond.out --hits /home/jon/Working_Files/blobtoolkit/nt_v5/nt.out.tab --taxdump /home/jon/Working_Files/blobtoolkit/taxdump tmp/dataset_1
Loading sequences from /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa
 - processing scaffold01047: : 77404it [01:11, 1080.96it/s] 
Parsing taxdump
bestsumorder
Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/hits.py", line 289, in parse
    blast = parse_blast(file, cols, None, index, float(kwargs['--evalue']), float(kwargs['--bitscore']))
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/hits.py", line 24, in parse_blast
    if evalue < float(row[cols['evalue']]):
IndexError: list index out of range
```
I think it is from parsing the blast file. So I guess I will remove that from the command for now
```bash
./blobtools create --fasta /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa  --hits /home/jon/Working_Files/blobtoolkit/uniprot/diamond.out --taxdump /home/jon/Working_Files/blobtoolkit/taxdump tmp/dataset_1
```

I believe this means it was a success
```bash
./blobtools create --fasta /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa  --hits /home/jon/Working_Files/blobtoolkit/uniprot/diamond.out --taxdump /home/jon/Working_Files/blobtoolkit/taxdump tmp/dataset_1
Loading sequences from /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa
 - processing scaffold01047: : 77404it [01:11, 1089.63it/s] 
Loading parsed taxdump
bestsumorder
```

viewing it. This starts up a local webpage that contains all the datasets in whatever folder is selected. In this case tmp
```bash
./blobtools host tmp
```

soo the snail plot is really cool.I am going to run this on the japonicus, parvimensis, and californicus genome assemblies now
```bash
./blobtools create --fasta /home/jon/Working_Files/parastichopus_parvimensis/GCA_000934455.1_Ppar_1.0_genomic.fna  tmp/parvimensis
./blobtools create --fasta /home/jon/Working_Files/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.fna tmp/japonicus
./blobtools create --fasta //home/jon/Working_Files/pcali_genome.platanus.fa tmp/californicus_plat-allee
./blobtools create --fasta //home/jon/Working_Files/pcali_genome.masurca.fasta tmp/californicus_masurca
./blobtools create --fasta //home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta tmp/californicus_redundans
```

adding busco results
```bash
./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_busco_pcali_ajap.redundans.platanus.masked.filter-1k.fa/run_eukaryota_odb10/full_table.tsv  tmp/dataset_1
./blobtools add \
	--busco /home/jon/Working_Files/busco/metazoa_busco_pcali_ajap.redundans.platanus.masked.filter-1k.fa/run_metazoa_odb10/full_table.tsv \
	tmp/dataset_1


./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_busco_masurca_pcali/run_eukaryota_odb10/full_table.tsv \
	--busco /home/jon/Working_Files/busco/metazoa_busco_masurca_pcali/run_metazoa_odb10/full_table.tsv \
	tmp/californicus_masurca

./blobtools add \
	--busco /home/jon/Working_Files/busco/Eukaryote_busco_platanus_allee/run_eukaryota_odb10/full_table.tsv \
	--busco /home/jon/Working_Files/busco/metazoa_busco_platanus_allee/run_metazoa_odb10/full_table.tsv \
	tmp/californicus_plat-allee

./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_busco_pcali_ajap.redundans.platanus.fasta/run_eukaryota_odb10/full_table.tsv

./blobtools add \
	--busco /home/jon/Working_Files/busco/metazoa_busco_pcali_ajap.redundans.platanus.fa/run_metazoa_odb10/full_table.tsv \
	tmp/californicus_redundan 
```

ok, one of them is throwing this error
```bash
./blobtools add \
> --busco /home/jon/Working_Files/busco/metazoa_busco_pcali_ajap.redundans.platanus.fa/run_metazoa_odb10/full_table.tsv \
> tmp/californicus_redundan
Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/busco.py", line 60, in parse
    parsed.append(parse_busco(file, identifiers=kwargs['dependencies']['identifiers']))
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/busco.py", line 43, in parse_busco
    if not identifiers.validate_list(list(results.keys())):
AttributeError: 'bool' object has no attribute 'validate_list'

```

# Wed 06 May 2020 10:00:43 AM PDT
so I found out that the version of busco I used had some bugs. I reran busco. See below for results

erp, I deleted the assembly files too
```bash
./blobtools create --fasta /home/jon/Working_Files/parastichopus_parvimensis/GCA_000934455.1_Ppar_1.0_genomic.fna  cuke/parvimensis
./blobtools create --fasta /home/jon/Working_Files/apostichopus_japonicus/GCA_002754855.1_ASM275485v1_genomic.fna cuke/japonicus
./blobtools create --fasta //home/jon/Working_Files/pcali_genome.platanus.fa cuke/californicus_plat-allee
./blobtools create --fasta //home/jon/Working_Files/pcali_genome.masurca.fasta cuke/californicus_masurca
./blobtools create --fasta //home/jon/Working_Files/pcali_ajap.redundans.platanus.fasta cuke/californicus_redundan
```

```bash
./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_masurca_pcali/run_eukaryota_odb10/full_table.tsv \
	--busco /home/jon/Working_Files/busco/met_masurca_pcali/run_metazoa_odb10/full_table.tsv \
	cuke/californicus_masurca

./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_plat_pcali/run_eukaryota_odb10/full_table.tsv \
	--busco /home/jon/Working_Files/busco/met_plat_pcali/run_metazoa_odb10/full_table.tsv \
	cuke/californicus_plat-allee

./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_jap/run_eukaryota_odb10/full_table.tsv \
	--busco /home/jon/Working_Files/busco/met_jap/run_metazoa_odb10/full_table.tsv \
	cuke/japonicus

./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_parv/run_eukaryota_odb10/full_table.tsv \
	--busco /home/jon/Working_Files/busco/met_parv/run_metazoa_odb10/full_table.tsv \
	cuke/parvimensis

./blobtools add \
	--busco /home/jon/Working_Files/busco/euk_redun_pcali/run_eukaryota_odb10/full_table.tsv \
	--busco /home/jon/Working_Files/busco/met_redun_pcali/run_metazoa_odb10/full_table.tsv \
	cuke/californicus_redundan 

./blobtools host cuke
```

adding hit data
```bash
# sorting the bam final and indexing it
samtools sort -@ 8 /home/jon/Working_Files/minimap2/pcali_redundans.bam | samtools index -@ 8 - 

./blobtools add \
    --cov /home/jon/Working_Files/redundans/minimap2/pcali_redundans.bam \
    --threads 8 \
    cuke/californicus_redundan
```

# Fri 08 May 2020 10:25:18 AM PDT
the coverage data for californicus redundans is still not showing up. Blobtools says it has already been added. weird

```bash
./blobtools add \
    --cov /home/jon/Working_Files/minimap2/ajap.bam \
    --threads 10 \
    cuke/ajap
```
is throwing this error 
```bash
Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 215, in parse
    fields = parse_bam(file, **kwargs, cov_range=cov_range, read_cov_range=read_cov_range)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 51, in parse_bam
    ids = identifiers.values
AttributeError: 'bool' object has no attribute 'values'
```

trying a different bam
```bash
./blobtools add \
    --cov /home/jon/Working_Files/minimap2/pcali_masurca.bam \
    --threads 70 \
    cuke/californicus_masurca
```
gives this error
```bash
[E::hts_idx_push] NO_COOR reads not in a single block at the end 13961 -1
Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 215, in parse
    fields = parse_bam(file, **kwargs, cov_range=cov_range, read_cov_range=read_cov_range)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 59, in parse_bam
    pysam.index(parts[0])
  File "/home/jon/anaconda3/envs/btk_env/lib/python3.6/site-packages/pysam/utils.py", line 75, in __call__
    stderr))
pysam.utils.SamtoolsError: 'samtools returned with error 1: stdout=, stderr=samtools index: failed to create index for "/home/jon/Working_Files/minimap2/pcali_masurca.bam"\n'
```

will index the bam file
```bash
samtools index -@ 70 /home/jon/Working_Files/minimap2/pcali_masurca.bam
```

```bash
#!/bin/bash

for i in *.bam

do

echo "Sorting: "$i        
samtools sort -@ 70 $i -o $i".sorted"
echo "Indexing: "$i"sorted"
samtools index -@ 70 $i".sorted" $i"sorted.bai"

done
```

# Sat 09 May 2020 11:41:01 AM PDT

sorting worked. It apparently produces a smaller file too due to better compression. I had the indexing written wrong though. redoing it
```bash
for i in *.bam.sorted

do
	echo "Indexing: " $i
	samtools index -@ 70 $i $i".bai"
done
```

success
```bash
./blobtools add \
    --cov /home/jon/Working_Files/minimap2/pcali_masurca.sorted.bam \
    --threads 70 \
    cuke/californicus_masurca
```

note to self: blobtools requires the bam files to also be named correctly. Also, it looks like I have to add blast hit results before the coverage info will show up. So without further ado, I shall run that

```bash
#blasting against the refseq nt database

for i in *".fa"

do
	echo "blastning against nt database with " $i
	blastn \
	 -query $i \
	 -db /home/jon/Working_Files/blobtoolkit/nt_v5/blast_db_nt/nt \
	 -outfmt "6 qseqid staxids bitscore std" \
	 -max_target_seqs 10 \
	 -max_hsps 1 \
	 -evalue 1e-25 \
	 -num_threads 70 > /home/jon/Working_Files/blobtoolkit/blastn_results/"$i".nt.out.tab

	echo "diamond blastxing against uniprot database with "$1 
	diamond blastx \
	 --query $i \
	 --db /home/jon/Working_Files/blobtoolkit/uniprot/reference_proteomes.dmnd \
	 --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
	 --sensitive \
	 --max-target-seqs 1 \
	 --evalue 1e-25 \
	 --threads 70 > /home/jon/Working_Files/blobtoolkit/blastx_results/"$1"diamond.out
done
 ```


 note to self, run mummer4 after this 
 also, fix id2go.py - remove spaces between GO terms and add a ";"

 lmao, I didn't need to write that script
"""
        Reads a gene id go term association file. The format of the file
        is as follows:
        AAR1	GO:0005575;GO:0003674;GO:0006970;GO:0006970;GO:0040029
        AAR2	GO:0005575;GO:0003674;GO:0040029;GO:0009845
        ACD5	GO:0005575;GO:0003674;GO:0008219
        ACL1	GO:0005575;GO:0003674;GO:0009965;GO:0010073
        ACL2	GO:0005575;GO:0003674;GO:0009826
        ACL3	GO:0005575;GO:0003674;GO:0009826;GO:0009965
        Also, the following format is accepted (gene ids are repeated):
        AAR1	GO:0005575
        AAR1    GO:0003674
        AAR1    GO:0006970
        AAR2	GO:0005575
        AAR2    GO:0003674
        AAR2    GO:0040029
        :param assoc_fn: file name of the association
        :return: dictionary having keys: gene id, values set of GO terms
"""

# Mon 11 May 2020 10:48:48 AM PDT

```bash
for i in *".fa"

do
	echo "diamond blastxing against uniprot database with "$1 
	diamond blastx \
	 --query $i \
	 --db /home/jon/Working_Files/blobtoolkit/uniprot/reference_proteomes.dmnd \
	 --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
	 --sensitive \
	 --max-target-seqs 1 \
	 --evalue 1e-25 \
	 --threads 70 \
	 --out /home/jon/Working_Files/blobtoolkit/blastx_results/"$1"diamond.out

done
```
> diamond has to be run using the biotools version, the new conda version is too new and complains about incompatible diamond databases

weird, it didn't work. whelp, new hard drives just arrived, so gonna add those first I think. 

# Thu 14 May 2020 02:25:37 PM PDT

added more hard drives to server, had to rebuild the raid, reinstall ubuntu, and restore the data. apparently I had deleted the uniprot database before backing up. oops

```bash
tar xf Reference_Proteomes_2019_11.tar.gz

touch reference_proteomes.fasta.gz
find . -mindepth 2 | grep "fasta.gz" | grep -v 'DNA' | grep -v 'additional' | xargs cat >> reference_proteomes.fasta.gz

echo "accession\taccession.version\ttaxid\tgi" > reference_proteomes.taxid_map
zcat */*.idmapping.gz | grep "NCBI_TaxID" | awk '{print $1 "\t" $1 "\t" $3 "\t" 0}' >> reference_proteomes.taxid_map

diamond makedb -p 70 --in reference_proteomes.fasta.gz --taxonmap reference_proteomes.taxid_map --taxonnodes ../taxdump/nodes.dmp -d reference_proteomes.dmnd
cd -
```

cool, now running diamond blastx
```bash
for i in *".fa"

do
	echo "diamond blastxing against uniprot database with "$i 
	diamond blastx \
	 --query $i \
	 --db /home/jon/Working_Files/blobtoolkit/uniprot/reference_proteomes.dmnd \
	 --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
	 --sensitive \
	 --max-target-seqs 1 \
	 --evalue 1e-25 \
	 --threads 70 \
	 --out /home/jon/Working_Files/blobtoolkit/blastx_results/"$i".diamond.out

done
```


# Fri 15 May 2020 05:05:21 PM PDT

that took a few days, ok. So now to try and add both the blast hits and the coverage data to, eventually


# Sun 17 May 2020 05:46:20 PM PDT

```bash
./blobtools add \
	--hits /home/jon/Working_Files/blobtoolkit/blastx_results/pcali_genome.masurca.fadiamond.out \
	--hits /home/jon/Working_Files/blobtoolkit/blastn_results/pcali_genome.masurca.fa.nt.out.tab \
    --threads 70 \
    --taxrule bestsumorder \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    cuke/californicus_masurca
```
it works!!!! just have to remember to include the taxdump thingy. Ok, now to do it for the rest of them. this is going to be a little tedious


note to self: if I have to reinstall ubuntu and restore from a backup, I will also need to reinstall git in order for this to work

```bash
./blobtools add \
	--hits /home/jon/Working_Files/blobtoolkit/blastx_results/ajap_assembly.fadiamond.out \
	--hits /home/jon/Working_Files/blobtoolkit/blastn_results/ajap_assembly.fa.nt.out.tab \
    --threads 70 \
    --taxrule bestsumorder \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    cuke/ajap
```

errors for both coverage and hits. weird.....oh, it's because I didn't add the genome assembly yet. 
```bash
Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/hits.py", line 271, in parse
    lengths = kwargs['dependencies']['length'].values
AttributeError: 'bool' object has no attribute 'values'

# and

Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 215, in parse
    fields = parse_bam(file, **kwargs, cov_range=cov_range, read_cov_range=read_cov_range)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 51, in parse_bam
    ids = identifiers.values
AttributeError: 'bool' object has no attribute 'values'
```

ok, moving on to the next one.

```bash
./blobtools add \
	--hits /home/jon/Working_Files/blobtoolkit/blastx_results/pcali_ajap.redundans.platanus.fadiamond.out \
	--hits /home/jon/Working_Files/blobtoolkit/blastn_results/pcali_ajap.redundans.platanus.fa.nt.out.tab \
    --threads 70 \
    --taxrule bestsumorder \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    cuke/californicus_redundan
```

sigh, complaint about the mapping file again
```bash
Loading mapping data from /home/jon/Working_Files/minimap2/pcali_redundans.bam.sorted as pcali_redundans.bam
Traceback (most recent call last):
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 153, in <module>
    main()
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/add.py", line 120, in main
    meta=meta)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 215, in parse
    fields = parse_bam(file, **kwargs, cov_range=cov_range, read_cov_range=read_cov_range)
  File "/home/jon/Working_Files/blobtoolkit/blobtools2/lib/cov.py", line 64, in parse_bam
    with pysam.AlignmentFile(parts[0], "r%s" % f_char) as aln:
  File "pysam/libcalignmentfile.pyx", line 741, in pysam.libcalignmentfile.AlignmentFile.__cinit__
  File "pysam/libcalignmentfile.pyx", line 838, in pysam.libcalignmentfile.AlignmentFile._open
AssertionError: invalid file opening mode `rs`
```

trying again with the mapping file. ballz. it is because the naming was wrong. Now I have to swap the bam.sort to sort.bam on all the files.....
```bash
./blobtools add \
	--cov /home/jon/Working_Files/minimap2/pcali_redundans.sorted.bam \
	cuke/californicus_redundan
```

oook, next up issss.....
```bash
./blobtools add \
	--hits /home/jon/Working_Files/blobtoolkit/blastx_results/pcali_genome.platanus.fadiamond.out \
	--hits /home/jon/Working_Files/blobtoolkit/blastn_results/pcali_genome.platanus.fa.nt.out.tab \
	--cov /home/jon/Working_Files/minimap2/pcali_plat-allee.sorted.bam \
    --threads 70 \
    --taxrule bestsumorder \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    cuke/californicus_plat-allee
```

# Mon 15 Jun 2020 03:16:25 PM PDT

running blobtoolkit on the pcali_ragout.fa assembly which used both parvimensis and japonicus as references. 

```bash
./blobtools create --fasta /home/jon/Working_Files/genome_assemblies/pcali_ragout.fa cuke/californicus_ragout

./blobtools add \
  --busco /home/jon/Working_Files/busco/euk_ragout_pcali/run_eukaryota_odb10/full_table.tsv \
  --busco /home/jon/Working_Files/busco/met_ragout_pcali/run_metazoa_odb10/full_table.tsv \
  cuke/californicus_ragout
```

```bash
docker run -d --rm --name btk \
          -v /home/jon/Working_Files/tmp/cuke:/home/jon/Working_Files/tmp/cuke \
          -p 8000:8000 -p 8080:8080 \
          -e VIEWER=true \
          genomehubs/blobtoolkit:latest
```

# Fri 24 Jul 2020 02:45:41 PM PDT

```bash
./blobtools create --fasta /home/jon/Working_Files/genome_assemblies/Ajap_genome.fasta  cuke/apostichopus_japonicus
./blobtools host cuke
```

blobtools host isn't working. something about npm stuff not liking ubuntu. Trying singularity
```bash
singularity pull docker://genomehubs/blobtoolkit
```


# Thu 03 Dec 2020 12:50:48 AM PST

trying docker blobtoolkit

fetching taxdump dataset
```bash
mkdir -p taxdump;
cd taxdump;
curl -L ftp://ftp.ncbi.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz | tar xzf -;
cd ..;
```

downloading nt database
```bash
mkdir -p nt
wget "ftp://ftp.ncbi.nlm.nih.gov/blast/db/nt.??.tar.gz" -P nt/ && \
        for file in nt/*.tar.gz; \
            do tar xf $file -C nt && rm $file; \
        done
```

downloading uniprot proteomes
```bash
mkdir -p uniprot
wget -q -O uniprot/reference_proteomes.tar.gz \
 ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/$(curl \
     -vs ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/ 2>&1 | \
     awk '/tar.gz/ {print $9}')
cd uniprot
tar xf reference_proteomes.tar.gz

touch reference_proteomes.fasta.gz
find . -mindepth 2 | grep "fasta.gz" | grep -v 'DNA' | grep -v 'additional' | xargs cat >> reference_proteomes.fasta.gz

echo "accession\taccession.version\ttaxid\tgi" > reference_proteomes.taxid_map
zcat */*.idmapping.gz | grep "NCBI_TaxID" | awk '{print $1 "\t" $1 "\t" $3 "\t" 0}' >> reference_proteomes.taxid_map

diamond makedb -p 16 --in reference_proteomes.fasta.gz --taxonmap reference_proteomes.taxid_map --taxonnodes ../taxdump/nodes.dmp -d reference_proteomes.dmnd
cd -
```

this is generating a minimal datasets for use with the viewer
```bash
docker run -it --rm --name btk \
           -u $UID:$GROUPS \
           -v /home/jon/Working_Files/blobtoolkit/docker/datasets:/blobtoolkit/datasets \
           -v /home/jon/Working_Files/blobtoolkit/docker/data:/blobtoolkit/data \
           -v /home/jon/Working_Files/blobtoolkit/docker/taxdump:/blobtoolkit/taxdump \
           genomehubs/blobtoolkit:latest \
           ./blobtools2/blobtools create \
           --fasta data/pcali_ragout.fa \
           --taxid 2032702 \
           --taxdump taxdump \
           datasets/ragout
```

hosting the interactive viewer with minimal dataset
```bash
docker run --name btk \
    -v /home/jon/Working_Files/blobtoolkit/docker/datasets:/blobtoolkit/datasets \
    -p 8000:8000 -p 8080:8080 \
    -e VIEWER=true \
    genomehubs/blobtoolkit:latest
```

huh, that throws some weird errors and doesn't work. It creates the container and is running, but the viewer doesn't load. 

Anyways, moving on to generating uniprot and nt hits

# Sat 05 Dec 2020 11:35:21 AM PST

```bash
for assembly in *.fa
do
  blastn -db /home/jon/Working_Files/blobtoolkit/docker/nt/nt \
         -query $assembly \
         -outfmt "6 qseqid staxids bitscore std" \
         -max_target_seqs 10 \
         -max_hsps 1 \
         -evalue 1e-25 \
         -num_threads 70 \
         -out $assembly.ncbi.blastn.out

  diamond blastx \
          --query $assembly \
          --db /home/jon/Working_Files/blobtoolkit/docker/uniprot/reference_proteomes.dmnd \
          --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
          --sensitive \
          --max-target-seqs 1 \
          --evalue 1e-25 \
          --threads 70 \
          > $assembly.diamond.blastx.out
done
```

setting up a portainer instance for managing docker images/containers
```bash
docker run -d -p 9000:9000 --name=port --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```


trying to create a blobtools dataset again and use the viewer
```bash
docker run -it --rm --name btk \
           -u $UID:$GROUPS \
           -v /home/jon/Working_Files/blobtoolkit/docker/datasets:/blobtoolkit/datasets \
           -v /home/jon/Working_Files/blobtoolkit/docker/data:/blobtoolkit/data \
           -v /home/jon/Working_Files/blobtoolkit/docker/taxdump:/blobtoolkit/taxdump \
           genomehubs/blobtoolkit:latest \
           ./blobtools2/blobtools create \
           --fasta data/pcali_ragout.fa \
           --taxid 2032702 \
           --taxdump taxdump \
           datasets/ragout
```

trying to host the interactive viewer with minimal dataset
```bash
docker run -d --rm --name btk \
    -v /home/jon/Working_Files/blobtoolkit/docker/:/blobtoolkit/datasets \
    -p 8000:8000 -p 8080:8080 \
    -e VIEWER=true \
    genomehubs/blobtoolkit:latest
```

nope, same problem. going to try with just plain ol' blobtools

creating new dataset
```bash
./blobtools create \
    --fasta /home/jon/Working_Files/blobtoolkit/docker/data/pcali_ragout.fa \
    --taxid 2032702 \
    --taxdump /home/jon/Working_Files/blobtoolkit/docker/taxdump \
    /home/jon/Working_Files/blobtoolkit/datasets/ragout
```

starting viewer
```bash
./blobtools view --interactive /home/jon/Working_Files/blobtoolkit/datasets/
```

# Wed 09 Dec 2020 09:44:22 AM PST
sooo apparently the diamond blastx command was missing something. hopefully the blastn output is fine

```bash
Error: staxids output field requires setting the --taxonmap parameter.
```
```bash
for assembly in *.fa
do
diamond blastx \
          --query $assembly \
          --db /home/jon/Working_Files/blobtoolkit/docker/uniprot/reference_proteomes.dmnd \
          --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
          --taxonmap /home/jon/Working_Files/prot.accession2taxid.gz \
          --taxonnodes /home/jon/Working_Files/blobtoolkit/docker/taxdump/nodes.dmp \
          --taxonnames /home/jon/Working_Files/blobtoolkit/docker/taxdump/names.dmp \
          --sensitive \
          --max-target-seqs 1 \
          --evalue 1e-25 \
          --threads 70 \
          > $assembly.diamond.blastx.out
done
```

lol, gotta remake the uniprot database
```bash
Error: Database was built with an older version of Diamond and is incompatible
```

```bash
diamond makedb -p 70 --in reference_proteomes.fasta.gz --taxonmap reference_proteomes.taxid_map --taxonnodes ../taxdump/nodes.dmp -d reference_proteomes.dmnd
```

huh, I didn't realize it would do the whole taxonomy thing when making the database
```bash
Loading taxonomy...  [44.341s]
Accession mappings = 57103363
Loading taxonomy nodes...  [1.277s]
Writing taxon id lists...  [29.144s]
53810826 sequences mapped to taxonomy, 53810826 total mappings.
Building taxonomy nodes...  [0.007s]
2793950 taxonomy nodes processed.
Number of nodes assigned to rank:
no rank           722611
superkingdom      4
kingdom           11
subkingdom        1
superphylum       1
phylum            268
subphylum         33
superclass        6
class             428
subclass          156
infraclass        18
cohort            5
subcohort         3
superorder        55
order             1653
suborder          372
infraorder        130
parvorder         26
superfamily       862
family            9447
subfamily         3046
tribe             2206
subtribe          507
genus             96164
subgenus          1605
section           437
subsection        21
series            9
species group     329
species subgroup  124
species           1870677
subspecies        25032
varietas          8484
forma             566
strain            44298
biotype           17
clade             885
forma specialis   740
genotype          20
isolate           1317
morph             12
pathogroup        5
serogroup         138
serotype          1216
subvariety        5

Closing the input file...  [0.011s]
Closing the database file...  [0.046s]
Database hash = 2a56d9dd0eb1cf29820c4704352dc377
Processed 53810826 sequences, 19792462395 letters.
Total time = 790.135s
```

does this mean I don't need to the taxonmap stuff? 

```bash
for assembly in *.fa
do
diamond blastx \
          --query $assembly \
          --db /home/jon/Working_Files/blobtoolkit/docker/uniprot/reference_proteomes.dmnd \
          --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
          --sensitive \
          --max-target-seqs 1 \
          --evalue 1e-25 \
          --threads 70 \
          > $assembly.diamond.blastx.out
done
```

lmao, apppparently. 

# Thu 10 Dec 2020 06:52:47 PM PST

diamond blastx just finished. Moving on to trying to visualize all of this using blobtools

```bash
docker run -d --rm --name btk \
    -v /home/jon/Working_Files/blobtoolkit/docker/:/blobtoolkit/datasets \
    -p 8000:8000 -p 8080:8080 \
    -e VIEWER=true \
    genomehubs/blobtoolkit:latest
```

nope, same problem. going to try with just plain ol' blobtools

creating new dataset
```bash
./blobtools create \
    --fasta /home/jon/Working_Files/blobtoolkit/data/pcali_ragout.fa \
    --taxid 2032702 \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    /home/jon/Working_Files/blobtoolkit/datasets/ragout
```

adding blast hits
```bash
./blobtools add \
    --hits /home/jon/Working_Files/blastn_hits/pcali_ragout.fa.ncbi.blastn.out \
    --hits /home/jon/Working_Files/diamond_blastx_hits/pcali_ragout.fa.diamond.blastx.out \
    --taxrule bestsumorder \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    /home/jon/Working_Files/blobtoolkit/datasets/ragout
```

adding coverage data
```bash
./blobtools add \
    --cov /home/jon/Working_Files/minimap2/raw_reads_2_assembly/pcali_ragout.fa_aln.bam \
    /home/jon/Working_Files/blobtoolkit/datasets/ragout
```

adding busco results
```bash
./blobtools add \
    --busco /home/jon/Working_Files/busco/auto_euk_pcali_ragout.fa/run_eukaryota_odb10/full_table.tsv \
    --busco /home/jon/Working_Files/busco/auto_euk_pcali_ragout.fa/run_metazoa_odb10/full_table.tsv \
    /home/jon/Working_Files/blobtoolkit/datasets/ragout
```


starting viewer
```bash
./blobtools2/blobtools view --interactive /home/jon/Working_Files/blobtoolkit/datasets/ragout
```

cool that worked

gonna create one for ragout with nanopore polishing
```bash
./blobtools2/blobtools create \
    --fasta /home/jon/Working_Files/genome_assemblies/californicus_genomes/pcali_ragout_racon-nanopore.fa \
    --taxid 2032702 \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    /home/jon/Working_Files/blobtoolkit/datasets/ragout_racon-nanopore
```

```bash
./blobtools2/blobtools view --interactive /home/jon/Working_Files/blobtoolkit/datasets/ragout_racon-nanopore
```

adding platanus-allee step 20 assembly, both unpolished and polished
```bash
./blobtools2/blobtools create \
    --fasta /home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20_racon-nanopore.fa \
    --taxid 2032702 \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    /home/jon/Working_Files/blobtoolkit/datasets/platanus-allee_illumina_step20_racon-nanopore

./blobtools2/blobtools create \
    --fasta /home/jon/Working_Files/genome_assemblies/californicus_genomes/platanus-allee_illumina_step20.fa \
    --taxid 2032702 \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    /home/jon/Working_Files/blobtoolkit/datasets/platanus-allee_illumina_step20  
```

trying to view them all
```bash
./blobtools2/blobtools host --port 8081 \
    --api-port 8001 \
    --hostname localhost \
    --viewer ../viewer \
    /home/jon/Working_Files/blobtoolkit/datasets
```

huh, didn't do a thing. dunno. Just moving on with visualizing the rest of the data
```bash
for files in *.fa
do
  /home/jon/Working_Files/blobtoolkit/blobtools2/blobtools create \
  --fasta $files \
  --taxid 2032702 \
  --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
  /home/jon/Working_Files/blobtoolkit/datasets/$files
done
```


adding the rest of the data
```bash
for file in *.fa
do
  /home/jon/Working_Files/blobtoolkit/blobtools2/blobtools add \
    --hits /home/jon/Working_Files/blastn_hits/$file.ncbi.blastn.out \
    --hits /home/jon/Working_Files/diamond_blastx_hits/$file.diamond.blastx.out \
    --cov /home/jon/Working_Files/minimap2/raw_reads_2_assembly/$file_aln.bam \
    --taxrule bestsumorder \
    --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
    /home/jon/Working_Files/blobtoolkit/datasets/$file
done

```

```bash
for file in *.fa
do
  /home/jon/Working_Files/blobtoolkit/blobtools2/blobtools add \
    --cov /home/jon/Working_Files/minimap2/raw_reads_2_assembly/$file\_aln.bam \
    /home/jon/Working_Files/blobtoolkit/datasets/$file
done

```

# Fri 11 Dec 2020 12:04:12 PM PST

awesome, that finished. Now lets see if I can't figure out the busco stuff
```bash
./blobtools2/blobtools add \
    --busco /home/jon/Working_Files/busco/auto_euk_pcali_ragout.fa/run_eukaryota_odb10/short_summary.txt\
    /home/jon/Working_Files/blobtoolkit/datasets/pcali_ragout.fa
```

