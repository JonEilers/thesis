# Wed 06 May 2020 10:45:38 AM PDT

updated multiqc
```bash
conda install -c bioconda multiqc==1.8
```

running multiqc on the busco directory while ignoring the old busco runs
```bash
multiqc -o /home/jon/Working_Files/multiqc \
	/home/jon/Working_Files/busco/euk_jap/short_summary.specific.eukaryota_odb10.euk_jap.txt \
	/home/jon/Working_Files/busco/euk_masurca_pcali/short_summary.specific.eukaryota_odb10.euk_masurca_pcali.txt \
	/home/jon/Working_Files/busco/euk_parv/short_summary.specific.eukaryota_odb10.euk_parv.txt \
	/home/jon/Working_Files/busco/euk_plat_pcali/short_summary.specific.eukaryota_odb10.euk_plat_pcali.txt \
	/home/jon/Working_Files/busco/euk_redun_pcali/short_summary.specific.eukaryota_odb10.euk_redun_pcali.txt \
	/home/jon/Working_Files/busco/met_jap/short_summary.specific.metazoa_odb10.met_jap.txt \
	/home/jon/Working_Files/busco/met_masurca_pcali/short_summary.specific.metazoa_odb10.met_masurca_pcali.txt \
	/home/jon/Working_Files/busco/met_parv/short_summary.specific.metazoa_odb10.met_parv.txt \
	/home/jon/Working_Files/busco/met_plat_pcali/short_summary.specific.metazoa_odb10.met_plat_pcali.txt \
	/home/jon/Working_Files/busco/met_redun_pcali/short_summary.specific.metazoa_odb10.met_redun_pcali.txt
```

multiqc was not finding the busco short summary files. This is due to a pattern recognition bug that was documneted on github. The fix has been applied to the github stuff, but not to the conda builds. The following file has been updated on my computer: search_patterns.yaml
```bash
busco:
    fn: 'short_summary_*'
    contents: 'BUSCO version is:'
    num_lines: 1

# should be
busco:
    fn: 'short_summary*'
    contents: 'BUSCO version is:'
    num_lines: 1
```


# Mon 23 Nov 2020 10:18:23 AM PST

annnd done. that took awhile. Now going to run multiqc to generate a report of busco analysis of all genome assemblies
```bash
multiqc -s -m busco .
```

# Tue 01 Dec 2020 11:07:00 PM PST
now to run one for samtools stats

```bash
multiqc -s -m samtools .
```
