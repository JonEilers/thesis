# Mon 15 Jun 2020 07:58:55 PM PDT

created bitacora conda env with the various bits installed

creating protein profile for the psp-94 like genes 
```bash
mafft --auto /home/jon/ajap_psp94.aa.fa > ajap_psp94.aln
hmmbuild ajap_psp94.hmm ajap_psp94.aln
# editted ./runBITACORA.sh for the various relevant bits
./runBITACORA.sh
```



```bash
Done, output files have been saved in FPDB/FPDB_genomic_and_annotated_proteins_trimmed_idseqsclustered.fasta, FPDB/FPDB_genomic_and_annotated_proteins_trimmed_idseqsclustered.gff3 and FPDB/FPDB_genomic_and_annotated_proteins_trimmed_idseqsclustered_table.txt


Cleaning output folders

BITACORA completed without errors :)
Mon 15 Jun 2020 08:47:25 PM PDT
```