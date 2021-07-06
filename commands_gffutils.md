#Fri 28 Aug 2020 03:04:13 PM PDT

cleaning up the braker2.1.5 annotation gff3 for pcali-ajap-rnaseq-prot.
```bash
gff_cleaner --clean braker2.1.5_rnaseq_ajap-prot.gff3
gffcleaner 0.10.3
Input : braker2.1.5_rnaseq_ajap-prot.gff3
Output: braker2.1.5_rnaseq_ajap-prot_clean.gff
Replacing 'Anc_*' and blanks with '0's in 'score' column
Traceback (most recent call last):
  File "/home/jon/anaconda3/envs/biotools/bin/gff_cleaner", line 8, in <module>
    sys.exit(main())
  File "/home/jon/anaconda3/envs/biotools/lib/python3.6/site-packages/GFFUtils/cli/gff_cleaner.py", line 267, in main
    score_unexpected_values = sorted(list(score_unexpected_values))
TypeError: '<' not supported between instances of 'str' and 'float'
```


sooo, this isn't very useful, trying something else