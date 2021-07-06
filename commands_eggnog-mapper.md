# Sun 24 Jan 2021 08:39:48 PM PST

platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam w_pcalifornicus_rnaseq_and_orthodb
```bash
conda activate eggnog-mapper
./emapper.py \
	--cpu 75 \
	-m mmseqs \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb/braker/augustus.hints.aa \
	--itype proteins \
	--output platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam_w_pcalifornicus_rnaseq_and_orthodb \
	--output_dir /home/jon/Working_Files/eggnog-mapper/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_pcalifornicus_rnaseq_and_orthodb
```

wow
```bash
Total time: 1498.85 secs
FINISHED
```

there are 31759 annotated genes for this round. So this might actually work. moving onto the next set

platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam w_ajaponicus_rnaseq_and_orthodb
```bash
conda activate eggnog-mapper
./emapper.py \
	--cpu 75 \
	-m mmseqs \
	-i/home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_ajaponicus_rnaseq_and_orthodb/braker/augustus.hints.aa \
	--itype proteins \
	--output platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam_w_ajaponicus_rnaseq_and_orthodb \
	--output_dir /home/jon/Working_Files/eggnog-mapper/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_ajaponicus_rnaseq_and_orthodb
```

```bash
Total time: 1619.42 secs
FINISHED
```

found 34594 annotations - still better then before. Could they be real? dunno, there was a lot more rna-seq data and from multiple tissues and stages. 



platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam_w_orthodb_only
```bash
conda activate eggnog-mapper
./emapper.py \
	--cpu 75 \
	-m mmseqs \
	-i /home/jon/Working_Files/braker2/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_orthodb_only/braker/augustus.hints.aa \
	--itype proteins \
	--output platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam_w_orthodb_only \
	--output_dir /home/jon/Working_Files/eggnog-mapper/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/w_orthodb_only
```

```bash
Total time: 1407.31 secs
FINISHED
```

nice.

protein annotations: 30538