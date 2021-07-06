
```bash
braker.pl --genome=../assembly/out_consensusScaffold.fa --makehub --cores 10 --esmode --GENEMARK_PATH=/home/jon/genemark --email jon.eilers@wallawalla.edu
```

```bash
./BRAKER/scripts/braker.pl --genome=../assembly/genome.filter.fa --makehub --cores 48 --esmode --GENEMARK_PATH=/home/jon/genemark --email jon.eilers@wallawalla.edu
```

throws an error saying no genes were found in such and such a file. Need to remember to have bash write output to a file


test run on P_cali genome assembly unfiltered - this commands doesn't look like what I used earlier, but I deleted the files already. oops. Ah, yes, it is missing the path to augustus config folder.

```bash
./BRAKER/scripts/braker.pl --genome=../assembly/genome.filter.fa --species=Parastichopus_californicus --makehub --cores 48 --bam=../../parastichopus_californicus/hisat2/rna-seq2.bam,../../parastichopus_californicus/hisat2/rna-seq1.bam --GENEMARK_PATH=/home/jon/genemark --email jon.eilers@wallawalla.edu
```

It works! When looking at it on igv there were a lot of scaffolds that were 200 bp long with nothing on it. So I filtered the genome assembly for reads longer than 3000bp. Should help a lot. Rerunning braker2. 

```bash
./BRAKER/scripts/braker.pl --genome=../assembly/genome.filter.fa --species=Parastichopus_californicus --esmode --makehub --cores 48  --GENEMARK_PATH=/home/jon/genemark --email jon.eilers@wallawalla.edu --useexisting
```

started throwing augustus errors so I stopped it and ran on single core. it threw another augustus error. Weird. am cloning the augustus git and will set the config file to that again, hopefully that fixes it. Actually, it might also be because of the previous parastichopus californicus gene models in the augustus config folder. I deleted it and am rerunning it. 

```bash
./BRAKER/scripts/braker.pl --genome=../assembly/genome.filter.fa --species=Parastichopus_californicus --esmode --makehub --cores 48  --GENEMARK_PATH=/home/jon/genemark --email jon.eilers@wallawalla.edu 
```

# Thu 30 Jan 2020 01:25:38 AM PST

> notes: make sure to output gff3, also, don't bother with makehub. vague recollections that braker2 didn't like the rna-seq alignment i provided and that I was going to look at that. 


# Mon 03 Feb 2020 08:03:39 PM PST

gonna try running braker2 again. 
```bash
./BRAKER/scripts/braker.pl --genome=/home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked --species=Parastichopus_californicus --esmode --makehub --gff3 --softmasking --cores 48  --GENEMARK_PATH=/home/jon/genemark --email jon.eilers@wallawalla.edu
```

so braker2 threw some errors.
```bash
ERROR in file ./BRAKER/scripts/braker.pl at line 6046
Failed to execute: perl /home/jon/genemark/gmes_petap.pl --verbose --cores=30 --ES --gc_donor 0.001 --sequence=/home/jon/Working_Files/braker2/braker/genome.fa  --soft_mask=1000 1>/home/jon/Working_Files/braker2/braker/GeneMark-ES.stdout 2>/home/jon/Working_Files/braker2/braker/errors/GeneMark-ES.stderr
The most common problem is an expired or not present file ~/.gm_key!
```

the braker2 genemark error file says something about yaml missing. I suspect it is because I haven't reinstalled the following perl packages: 
   YAML
   Hash::Merge
   Logger::Simple
   Parallel::ForkManager

hmm, ok they are all up to date now. nope. still getting the same error. so I tried installing something else and rerunning
```bash
sudo apt-get install libconfig-yaml-perl 
```

huh, now it's complaining about hash/merge not being present. okk. Am doing it for the other three then
```bash
 sudo apt-get install -y liblog-agent-logger-perl 
 sudo apt-get install libconfig-yaml-perl
 sudo apt-get install libparallel-forkmanager-perl
 ```

 it would appear that worked. However, for future reference, the @inc tool is what looks for perl modules in specific directories. There is the possibility that the conda perl directory wasn't added when I installed reinstalled ubuntu and restored the home directory. So next, I should look into updating the @inc to look at conda

 ok, so it didn't work again. but this time the error file in braker was empty. weird. so I took the command that apparently failed and am running it.

```bash
 ERROR in file ./BRAKER/scripts/braker.pl at line 6046
Failed to execute: perl /home/jon/genemark/gmes_petap.pl --verbose --cores=30 --ES --gc_donor 0.001 --sequence=/home/jon/Working_Files/braker2/braker/genome.fa  --soft_mask=1000 1>/home/jon/Working_Files/braker2/braker/GeneMark-ES.stdout 2>/home/jon/Working_Files/braker2/braker/errors/GeneMark-ES.stderr
The most common problem is an expired or not present file ~/.gm_key!
```

the command
```bash
perl /home/jon/genemark/gmes_petap.pl --verbose --cores=30 --ES --gc_donor 0.001 --sequence=/home/jon/Working_Files/braker2/braker/genome.fa  --soft_mask=1000 1>/home/jon/Working_Files/braker2/braker/GeneMark-ES.stdout 2>/home/jon/Working_Files/braker2/braker/errors/GeneMark-ES.stderr
```

well it is running fine so far. weird. 
the braker2 github page has a list of conda packages to install for braker2 dependecies so I am going down the list now. 

added this to .profile
```bash
PERL5LIB=$HOME/anaconda3/env/biotools/bin; export PERL5LIB
```


```bash
./BRAKER/scripts/braker.pl --genome=/home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked --species=Parastichopus_californicus --esmode --makehub --gff3 --softmasking --cores 48  --GENEMARK_PATH=/home/jon/genemark/gmes_petap.pl --email jon.eilers@wallawalla.edu
```

sigh, ok, so genemark stdout says this
```bash
check before run
create directories
commit input data
data report
commit training data
training data report
prepare initial model
get GC of sequence
GC 37
build initial ES model
running step ES_A
running gm.hmm on local multi-core system
2223 contigs in training
concatenate predictions: /home/jon/Working_Files/braker2/braker/GeneMark-ES/run/ES_A_1
training level ES_A: /home/jon/Working_Files/braker2/braker/GeneMark-ES/run/ES_A_1
Initial	2896
Internal	4588
Terminal	3053
Single	674
Intron:	7753	42798343
Intergenic:	870	4705209
running gm.hmm on local multi-core system
2223 contigs in training
concatenate predictions: /home/jon/Working_Files/braker2/braker/GeneMark-ES/run/ES_A_2
training level ES_A: /home/jon/Working_Files/braker2/braker/GeneMark-ES/run/ES_A_2
Initial	4391
Internal	14379
Terminal	4690
Single	559
Intron:	19272	69972263
Intergenic:	1722	8578836
running step ES_B
error, file not found /home/jon/genemark//gmes_petap.pl: run/ES_A.mod
```

# Wed 05 Feb 2020 12:59:13 AM PST

oooook, where to begin. I was bad and didn't write everything I did down here. 

so short story. I modified my .profile file to include paths to genemark, braker, and perl/conda paths. Whether that did anything I don't know. the genemark path did, the braker and conda/perl I am not so sure. 

```bash
PERL5LIB=$HOME/anaconda3/env/biotools/bin; export PERL5LIB

PATH=/home/jon/Working_Files/braker2/BRAKER/scripts:$PATH
GENEMARK_PATH=/home/jon/genemark 
export GENEMARK_PATH
export PATH
```

the second thing I did was to copy all the braker github repository scripts into the biotools conda bin. ubuntu then asked if I wanted to replace the already existing files. which I said yes. That seemed to fix whatever problem was happening above.

Finally, there is a weird bug in the filterGenesIn_mRNAname.pl which requires the below fix

```bash
$_ =~ m/transcript_id \"(.*)\"/

to

$_ =~ m/transcript_id \"(.*?)\"
```
after doing all this the built in test scripts test1.sh and test8.sh successfully completed. 

> a few notes: apparently max core used is now 8 for some scripts, but others will use as many there are contigs. Also, if genemark isn't showing up in path, then .profile needs to be ```source ~/.profile```

sooo without further ado,

```bash
braker.pl --genome=/home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked --species=Parastichopus_californicus --esmode --makehub --gff3 --softmasking --cores 8 --email jon.eilers@wallawalla.edu
```

lol, I ran it from home directory. sigh..... also, I need to rerun the platanus-allee assembly on braker as I lost the data when the ssd crashed on my other computer

hell ya! worked. number of predicted genes was still way too high though

# Fri 07 Feb 2020 08:27:36 PM PST

## running braker2 with rna-seq data and the pcali-plat-redun-masked-filtered.1k genome

```bash
 braker.pl --species=Parastichopus californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam --softmasking --cores 40 --makehub --gff3 --email jon.eilers@wallawalla.edu
```

# Tue 11 Feb 2020 08:26:40 PM PST

yesterday when I got home I found braker2.pl was still running. I realized it was because most of the command hadn't pasted to the command line so it was running on single core with no masking. I fixed it and reran the software. It threw the error below

```bash
ERROR in file /home/jon/anaconda3/envs/biotools/bin/braker.pl at line 7729
Failed to execute: /home/jon/anaconda3/envs/biotools/bin/python3 /home/jon/anaconda3/envs/biotools/bin/fix_in_frame_stop_codon_genes.py -g /home/jon/Working_Files/braker2/braker/genome.fa -t /home/jon/Working_Files/braker2/braker/augustus.hints.gtf -b /home/jon/Working_Files/braker2/braker/bad_genes.lst -o augustus.hints_fix_ifs_ -s Parastichopus -m on --UTR off --print_utr off -a /home/jon/anaconda3/envs/biotools/config/ -C /home/jon/anaconda3/envs/biotools/bin -A /home/jon/anaconda3/envs/biotools/bin/ -S /home/jon/anaconda3/envs/biotools/bin/ -H /home/jon/Working_Files/braker2/braker/hintsfile.gff -e /home/jon/anaconda3/envs/biotools/bin/cfg/rnaseq.cfg  > /home/jon/Working_Files/braker2/braker/fix_in_frame_stop_codon_genes_augustus.hints.log 2> /home/jon/Working_Files/braker2/braker/errors/fix_in_frame_stop_codon_genes_augustus.hints.err
```

I ran the above command and it worked so I don't know what happened. I think I will try to rerun it and see what happens. 

```bash
 braker.pl --species=Parastichopus californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam --softmasking --cores 30 --makehub --gff3 --email jon.eilers@wallawalla.edu --useexisting
```

It threw the same error. I poked around the braker2 github issues page and found this error has been discussed and determined to be an augustus issue. strange it wasn't thrown when running braker2 on the test data. Conda has an updated Augustus as of 4 days ago so I shall give that a shot. 

new command
```bash
 braker.pl --species=Parastichopus californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam --softmasking --cores 75 --makehub --gff3 --email jon.eilers@wallawalla.edu --useexisting
```


used for counting genes/proteins in file
```bash
grep ">" augustus.ab_initio.aa | wc -l
```
ok, so the problem is in the fix_in_frame_stop_codon_genes.py augustus script. when I updated the conda version, it still threw the error and when I looked, the script was still the same one that was causing the problem. so I checked out the augustus github repo and found the script had been fixed back in september shortly after the problem was discovered. So I replaced the old conda augustus script with the new and viola. 

# Sat 15 Feb 2020 06:42:25 AM PST

it worked! 
results
```bash
grep ">" augustus.hints.aa | wc -l
43795
```

the count is lower, but I wonder if that is because I filtered out reads shorter than 1kb in length. I should probably run braker2 esmode on the filtered assembly to find out. 

# Sun 16 Feb 2020 08:35:40 PM PST

just planning for the next step once some stuff finishes running

braker2 with protein hints from apostichopus japonicus and rna-seq from p. californicus. 
> note: the A. japonicus proteins are from uniprot and are predicted from the genome I think. The actual proteome data from the original genome sequencing project was never uploaded to any databases even though ncbi has a bioproject page for it. 
```bash

braker.pl --species=Parastichopus_californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam  \
  --softmasking --cores 75 --makehub --gff3 --email jon.eilers@wallawalla.edu \
  --prot_seq=/home/jon/Working_Files/apostichopus_japonicus/ajap_protein_uniprot.fasta \
  --prg=gth

```

there is also an option to use proteins of close evolutionary distance for extending gene training.

# Wed 19 Feb 2020 09:29:59 AM PST

ran the above command last night. it stopped this morning,

```bash
# Wed Feb 19 04:38:52 2020: ERROR: in file /home/jon/anaconda3/envs/biotools/bin/braker.pl at line 6835
Training gene file in genbank format /home/jon/Working_Files/braker/train.f.gb does not contain any training genes. Possible known cause: no training genes with sufficient extrinsic evidence support or of sufficient length were produced by GeneMark-ES/ET. If you think this is the cause for your problems, consider running BRAKER with different evidence or without any evidence (--esmode) for training.
```

this looks like the weird error I used to get. so I checked the  filterGenesIn_mRNAname.pl file and it contained the bug again. weird. I wonder if I tried to update braker2 via conda recently or something?

the fix is below
```bash
$_ =~ m/transcript_id \"(.*)\"/

to

$_ =~ m/transcript_id \"(.*?)\"
```

am rerunning it now. 

yaaaaay, a new type of error
```bash
ln: failed to create symbolic link '/home/jon/Working_Files/braker/traingenes.gtf': File exists
ERROR in file /home/jon/anaconda3/envs/biotools/bin/braker.pl at line 6654
failed to execute: ln -s /home/jon/Working_Files/braker/GeneMark-ET/genemark.gtf /home/jon/Working_Files/braker/traingenes.gtf!
```

ah ballz, it's complaining because a file already exists. because I didn't delete the first failed run. siggggh. ok. rerunning after deleting the braker folder. 

# Thu 20 Feb 2020 01:20:04 PM PST

ok, now I got an error I haven't seen
```bash
ERROR in file /home/jon/anaconda3/envs/biotools/bin/braker.pl at line 11111
Failed to execute: /home/jon/anaconda3/envs/biotools/bin/python3 /home/jon/anaconda3/envs/biotools/bin/make_hub.py -g /home/jon/Working_Files/braker/genome.fa -e jon.eilers@wallawalla.edu -l hub_Par -L Parastichopus_californicus -X /home/jon/Working_Files/braker -P  > /home/jon/Working_Files/braker/makehub.log 2> /home/jon/Working_Files/braker/errors/makehub.err
```

eh, everything looks like it is there except for the makehub file. however, the braker.log doesn't say it completed so I am going to run it again with same command minus the makehub option and see what happens. There was also an update a few hours ago so I will do the whole copy/paste to the conda biotools bin. Living dangerously

```bash

braker.pl --species=Parastichopus_californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
  --softmasking --cores 75 --gff3 \
  --prot_seq=/home/jon/Working_Files/apostichopus_japonicus/ajap_protein_uniprot.fasta \
  --prg=gth \
  --useexisting

```


ooook, there is a new conda braker2 update. so I am going to try it. The filterGenesIn_mRNAname.pl is actually an augustus script and the most recent conda package did not update it. super weird. 

ok, I am going to download the most recent braker2 clone and augustus clone and copy/paste the scripts files into the conda biotools bin directory. Augustus requires compilation, which I really can't do on this computer atm, but I think the copy/pasting the scripts should be fine. 

Also found that the fix Dr. Hoff added to the filtergenes script is actually different then the original fix someone used and which I have been using. hooopefully that doesn't matter. the makefile doesn't do anything to the script directory. fingers crossed. 

so it threw an error about helpmod not exporting something. I checked the most recent conda braker2 and it is version 2.4.1 so I updated that which should replace the files I copied/pasted from braker2. apparently braker 2.5 is a major update and may require some more odds and ends. The maintainer of the conda braker2 package said she marked the package for update a few hrs ago and will check dependencies after, which means I will have to wait a few days. I am going to go ahead and just run as is and see what happens. Augustus may not like me though. 

and running it now. so far so good

am poking the various braker2 runs I have so far. see results below

ab initio: 47357
pcali rna-seq: 43795
pcali rna-seq + ajap prot: 43057

> note: #!/home/jon/anaconda3/envs/biotools/bin/perl is found at the top of the conda scripts whereas #!/usr/bin/env perl is found at the top of the github scripts. I suspect that is why helpmod is unable to export. in the future I should try swapping those out before I copy/paste? Also, this makes me suspect that this run will fail. 

# Fri 21 Feb 2020 09:40:16 AM PST
  
 it finished with 43057 genes predicted sooo cool. Note braker2 2.1.5 is now available on conda. I am going to update it. 


# Wed 26 Feb 2020 11:33:38 AM PST

am bored so I think I will run braker2 with the utr setting on and see what happens. I don't know if the rna-seq data was stranded, but hopefully it was. also, this is using braker2 2.1.5

```bash

braker.pl --species=Parastichopus_californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
  --softmasking --cores 75 --gff3 \
  --prot_seq=/home/jon/Working_Files/apostichopus_japonicus/ajap_protein_uniprot.fasta \
  --prg=gth \
  --useexisting \
  --UTR=on

```


ballz........ was looking at the commands running via htop and noticed utr was off. thought that was weird until I read the braker2 instructions

```bash
This flag only works if --softmasking is also enabled, and if the only extrinsic evidence provided are bam files. This is an experimental feature!
```

so I need to stop and restart the run without the protein data. and so I will.


```bash

braker.pl --species=Parastichopus_californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
  --softmasking --cores 75 --gff3 \
  --useexisting \
  --UTR=on

```

# Mon 02 Mar 2020 08:29:04 PM PST

this is some bash code for getting average read length
```bash
stats.sh /home/jon/Working_Files/braker/augustus.hints.codingseq

A C G T N IUPAC Other GC  GC_stdev
0.3005  0.2177  0.2419  0.2399  0.0007  0.0000  0.0000  0.4596  0.0428

Main genome scaffold total:           47357
Main genome contig total:             47361
Main genome scaffold sequence total:  39.506 MB
Main genome contig sequence total:    39.478 MB   0.073% gap
Main genome scaffold N/L50:           7234/1.404 KB
Main genome contig N/L50:             7232/1.404 KB
Main genome scaffold N/L90:           29273/348
Main genome contig N/L90:             29068/349
Max scaffold length:                  66.432 KB
Max contig length:                    66.426 KB
Number of scaffolds > 50 KB:          1
% main genome in scaffolds > 50 KB:   0.17%


Minimum   Number          Number          Total           Total           Scaffold
Scaffold  of              of              Scaffold        Contig          Contig  
Length    Scaffolds       Contigs         Length          Length          Coverage
--------  --------------  --------------  --------------  --------------  --------
    All           47,357          47,361      39,506,433      39,477,765    99.93%
     50           47,310          47,314      39,504,732      39,476,102    99.93%
    100           46,318          46,322      39,423,153      39,395,417    99.93%
    250           35,484          35,488      37,460,067      37,443,084    99.95%
    500           22,607          22,609      32,841,510      32,834,368    99.98%
   1 KB           11,777          11,777      25,129,317      25,127,194    99.99%
 2.5 KB            2,616           2,616      11,304,009      11,303,669   100.00%
   5 KB              555             555       4,469,877       4,469,791   100.00%
  10 KB              104             104       1,509,210       1,509,172   100.00%
  25 KB                5               5         215,142         215,132   100.00%
  50 KB                1               1          66,432          66,426    99.99%

 ```

 > for the utr braker2 run as seen above
 44300 sequences, total length 14444026 or 326.0502 amino acids per a gene on average or 978.15 nucleic acid bases per a gene

ok, running it on the braker2 run using rna-seq data and ajap protein data
```bash
stats.sh /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/augustus.hints.codingseq

A C G T N IUPAC Other GC  GC_stdev
0.3018  0.2169  0.2411  0.2402  0.0006  0.0000  0.0000  0.4580  0.0404

Main genome scaffold total:           43057
Main genome contig total:             43072
Main genome scaffold sequence total:  40.253 MB
Main genome contig sequence total:    40.230 MB   0.058% gap
Main genome scaffold N/L50:           6959/1.497 KB
Main genome contig N/L50:             6957/1.497 KB
Main genome scaffold N/L90:           27278/396
Main genome contig N/L90:             27245/395
Max scaffold length:                  63.792 KB
Max contig length:                    63.786 KB
Number of scaffolds > 50 KB:          1
% main genome in scaffolds > 50 KB:   0.16%


Minimum   Number          Number          Total           Total           Scaffold
Scaffold  of              of              Scaffold        Contig          Contig  
Length    Scaffolds       Contigs         Length          Length          Coverage
--------  --------------  --------------  --------------  --------------  --------
    All           43,057          43,072      40,253,328      40,229,788    99.94%
     50           43,024          43,039      40,252,071      40,228,565    99.94%
    100           42,541          42,556      40,212,519      40,189,475    99.94%
    250           35,716          35,731      38,926,569      38,910,004    99.96%
    500           22,973          22,983      34,320,492      34,312,576    99.98%
   1 KB           12,364          12,368      26,743,623      26,740,873    99.99%
 2.5 KB            2,788           2,791      12,234,051      12,233,363    99.99%
   5 KB              616             617       5,002,209       5,001,974   100.00%
  10 KB              118             118       1,726,755       1,726,715   100.00%
  25 KB                7               7         313,629         313,612    99.99%
  50 KB                1               1          63,792          63,786    99.99%

```

running it on braker2 gene predictions using rna-seq
```bash
 stats.sh /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/augustus.hints.codingseq

A C G T N IUPAC Other GC  GC_stdev
0.3020  0.2171  0.2414  0.2395  0.0004  0.0000  0.0000  0.4585  0.0403

Main genome scaffold total:           43795
Main genome contig total:             43798
Main genome scaffold sequence total:  43.312 MB
Main genome contig sequence total:    43.294 MB   0.043% gap
Main genome scaffold N/L50:           6946/1.596 KB
Main genome contig N/L50:             6943/1.596 KB
Main genome scaffold N/L90:           27559/417
Main genome contig N/L90:             27512/417
Max scaffold length:                  45.363 KB
Max contig length:                    45.359 KB
Number of scaffolds > 50 KB:          0
% main genome in scaffolds > 50 KB:   0.00%


Minimum   Number          Number          Total           Total           Scaffold
Scaffold  of              of              Scaffold        Contig          Contig  
Length    Scaffolds       Contigs         Length          Length          Coverage
--------  --------------  --------------  --------------  --------------  --------
    All           43,795          43,798      43,312,407      43,293,603    99.96%
     50           43,763          43,766      43,311,258      43,292,488    99.96%
    100           43,286          43,289      43,272,210      43,253,839    99.96%
    250           36,780          36,783      42,029,658      42,016,489    99.97%
    500           24,216          24,216      37,494,540      37,488,393    99.98%
   1 KB           13,323          13,323      29,722,956      29,720,811    99.99%
 2.5 KB            3,208           3,208      14,347,797      14,347,390   100.00%
   5 KB              747             747       6,155,967       6,155,853   100.00%
  10 KB              139             139       2,154,879       2,154,836   100.00%
  25 KB               14              14         537,222         537,204   100.00%
```

and once more on the ab initio gene predictions
```bash
stats.sh /home/jon/Working_Files/braker2/braker_denovo_pcali_ajap_redun_plat/augustus.ab_initio.codingseq

A C G T N IUPAC Other GC  GC_stdev
0.3005  0.2177  0.2419  0.2399  0.0007  0.0000  0.0000  0.4596  0.0428

Main genome scaffold total:           47357
Main genome contig total:             47361
Main genome scaffold sequence total:  39.506 MB
Main genome contig sequence total:    39.478 MB   0.073% gap
Main genome scaffold N/L50:           7234/1.404 KB
Main genome contig N/L50:             7232/1.404 KB
Main genome scaffold N/L90:           29273/348
Main genome contig N/L90:             29068/349
Max scaffold length:                  66.432 KB
Max contig length:                    66.426 KB
Number of scaffolds > 50 KB:          1
% main genome in scaffolds > 50 KB:   0.17%


Minimum   Number          Number          Total           Total           Scaffold
Scaffold  of              of              Scaffold        Contig          Contig  
Length    Scaffolds       Contigs         Length          Length          Coverage
--------  --------------  --------------  --------------  --------------  --------
    All           47,357          47,361      39,506,433      39,477,765    99.93%
     50           47,310          47,314      39,504,732      39,476,102    99.93%
    100           46,318          46,322      39,423,153      39,395,417    99.93%
    250           35,484          35,488      37,460,067      37,443,084    99.95%
    500           22,607          22,609      32,841,510      32,834,368    99.98%
   1 KB           11,777          11,777      25,129,317      25,127,194    99.99%
 2.5 KB            2,616           2,616      11,304,009      11,303,669   100.00%
   5 KB              555             555       4,469,877       4,469,791   100.00%
  10 KB              104             104       1,509,210       1,509,172   100.00%
  25 KB                5               5         215,142         215,132   100.00%
  50 KB                1               1          66,432          66,426    99.99%

```

> note: the braker2 runs using rna-seq and rna-seq + ajap protein data using an assembly that had scaffolds less than 1kbp removed. Also the braker2 run using utr was braker 2.1.5 whereas the rest used braker2 2.1.4


### seqkit stats

```bash
seqkit stats -a -T /home/jon/Working_Files/braker2/braker_denovo_pcali_ajap_redun_plat/augustus.ab_initio.aa

seqkit stats -a -T /home/jon/Working_Files/braker2/braker_rna_pcali_platredun/rna-mode.augustus.hints.aa

seqkit stats -a -T /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa

> this is the utr braker2 run with braker2 2.1.5 and rna-seq data with utr set to on
seqkit stats -a -T /home/jon/Working_Files/braker/augustus.hints.aa 

                        format  type  num_seqs  sum_len    min_len avg_len   max_len Q1  Q2  Q3  sum_gap N50 Q20(%)  Q30(%)
 ab initio              FASTA Protein 47357     13135670   2       277.4     22143   83  156 331 0       468  0.00    0.00
 rna-seq                FASTA Protein 43795     14402746   3       328.9     15120   103 190 389 0       532  0.00    0.00
 rna-seq + ajap protein FASTA Protein 43057     13385167   3       310.9     21263   101 181 371 0       499  0.00    0.00
 rna-seq w/ UTR         FASTA Protein 44300     14444026   3       326.1     21264   103 187 386 0       529  0.00    0.00
```


sooo those are some tiny proteins....gonna need to take a look at those

```bash
bbduk.sh ignorejunk in=/home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/pcali-rna.ajap-prot.augustus.hints.aa out=short_protein_predictions maxlen=50 minlen=0

Input:                    43057 reads     13385167 bases.
Total Removed:            40945 reads (95.09%)  13303463 bases (99.39%)
Result:                   2112 reads (4.91%)  81704 bases (0.61%)


seqkit stats -a -T short_protein_predictions > short_protein_predictions.txt

format  type    num_seqs  sum_len min_len avg_len max_len Q1  Q2  Q3  sum_gap N50 Q20(%)  Q30(%)
FASTA   Protein 2112      81704   3       38.7    50      33  40  46  0       43  0.00    0.00

```

# Thu 05 Mar 2020 05:57:06 PM PST

gonna use agat to take a quick peak at the braker2 gff3 stats

```bash
#ab initio
agat_sq_stat_basic.pl -i /home/jon/Working_Files/braker2/braker_denovo_pcali_ajap_redun_plat/braker2_abinitio.genepredictions.pcali_ajap_redundan.platanus.gff3  -g /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > braker2_abinitio.genepredictions.pcali_ajap_redundan.platanus.gff3.stats

# with rna-seq
agat_sq_stat_basic.pl -i //home/jon/Working_Files/braker2/braker_rna_pcali_platredun/braker2_pcali-rna.genepredictions.pcali_ajap_redundan.platanus.gff3  -g /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > braker2_pcali-rna.genepredictions.pcali_ajap_redundan.platanus.gff3.stats

# with rna-seq and a-jap protein data
agat_sq_stat_basic.pl -i /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/braker2_pcali-rna_ajap-protein.genepredictions.pcali_ajap_redundan.platanus.gff3  -g /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > braker2_pcali-rna_ajap-protein.genepredictions.pcali_ajap_redundan.platanus.gff3.stats

# with rna-seq and utr on and braker 2.1.5
agat_sq_stat_basic.pl -i /home/jon/Working_Files/braker2/braker_rna-utr.pcali_ajap_redundans.platanus/augustus.hints_utr.gff  -g /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > augustus.hints_utr.gff.stats

```

hmmmm, just realized that braker.gff3 and augustus.hints.gff3 are different and that braker.gff3 is probably the one I want. I need to double check. If so, I need to rerun the braker2 with protein because it didn't produce a braker.gff3 file. In the mean time, I am gonna run the agat on one of the braker.gff3 files to check. 

```bash
# with rna-seq
agat_sq_stat_basic.pl -i //home/jon/Working_Files/braker2/braker_rna_pcali_platredun/braker.gff3  -g /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > braker.gff3.stats
```

Rerunning braker with rna-seq and rna-seq plus protein
```bash
braker.pl --species=Parastichopus_californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
  --softmasking --cores 75 --gff3 \
  --prot_seq=/home/jon/Working_Files/apostichopus_japonicus/ajap_protein_uniprot.fasta \
  --prg=gth \
  --useexisting
  ```

# Fri 06 Mar 2020 03:58:50 PM PST

and success

checking stats now
```bash
  agat_sq_stat_basic.pl -i //home/jon/braker/braker.gff3  -g /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > braker_new.gff3.stats
```
nothing really changed, huh.

running updated braker2 on rna-seq now
```bash
braker.pl --species=Parastichopus_californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
  --softmasking --cores 75 --gff3 \
  --useexisting
  ```

yup still fast, running the agat script on it
```bash
  agat_sq_stat_basic.pl -i //home/jon/braker/braker.gff3  -g /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa > braker_new_new.gff3.stats
```

# Sat 07 Mar 2020 02:06:38 PM PST
am gonna try a different script
```bash
report_gff3_statistics.py -i /home/jon/Working_Files/braker2.1.5_rnaseq/braker2.1.5_rnaseq.gff3 -o braker2.1.5_rnaseq_gff3.stats

#running it on an older braker2 run
report_gff3_statistics.py -i /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/braker2_pcali-rna_ajap-protein.genepredictions.pcali_ajap_redundan.platanus.gff3 -o ~/biocode_braker2_rnaseq_gff3.stats 2> ~/biocode_error.log
```

ok, so after looking at the output error log and seeing that it is essentially complaining about the order of features I think the gff3 file is not in order. going to use gffutils to sort it

```bash
gff3_sort --gff_file /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/braker2_pcali-rna_ajap-protein.genepredictions.pcali_ajap_redundan.platanus.gff3 --output_gff /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/braker2_pcali-rna_ajap-protein.genepredictions.pcali_ajap_redundan.platanus.sorted.gff3

# now running the stats script on it
report_gff3_statistics.py -i /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/braker2_pcali-rna_ajap-protein.genepredictions.pcali_ajap_redundan.platanus.sorted.gff3 -o ~/biocode_braker2_rnaseq_gff3.sorted.stats 2> ~/biocode_error_sorted.log

```

hmmm, ok, still throwing the same errors. Trying a biocode script for this
```bash
gff3sort.pl /home/jon/Working_Files/braker2/braker_pcali-rna_ajap-prot/braker.gff3 >braker.sort.gff3

# now running the stats script on it
report_gff3_statistics.py -i /home/jon/braker.sort.gff3 -o /home/jon/braker.sort.stats 2> ~/biocode_error_sorted.log
```

huh, well, it did sort the gff3. When looking at the features that were skipped in the stats program it was only the features that had a number at the end. Ah, so introns, start, and stop codons are not recognized by the gensas parser. So the scripts have other issues when looking at stats. 

# Wed 18 Mar 2020 01:44:17 PM PDT

while looking at some genes in apollo I noticed that some of the genes contained softmasked regions and had no ncbi blast hits that were genes. I looked on braker2 github and saw someone had previously asked about this particular problem. Apparently a .cfg file in the braker2 cfg folder can be edited to reduce this problem. I am going to give it a try and see what happens. 

```bash
# original
nonexonpart      1          1  M    1  1e+100  RM  1     1.15 E 1    1    W 1    1  P 1 1

# changed
nonexonpart      1          1  M    1  1e+100  RM  1     2.15 E 1    1    W 1    1  P 1 1


# rerunning braker
braker.pl --species=Parastichopus_californicus --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
  --softmasking --cores 75 --gff3 \
  --prot_seq=/home/jon/Working_Files/apostichopus_japonicus/ajap_protein_uniprot.fasta \
  --prg=gth \
  --useexisting
```

The cfg files in the braker species folder appear to not have the 2.15 in it. Which makes me think I need to edit it in the species cfg. 


# Thu 19 Mar 2020 04:20:29 PM PDT

so I have been trying to align/map protein to the genome and so far the programs have been a pain to run solo. Am going to run braker2 with spaln rather than gth. 
```bash
# rerunning braker
braker.pl --species=Parastichopus_californicus_noexon2.15 --genome=/home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
  --bam=/home/jon/Working_Files/star/Aligned.sortedByCoord.out.bam \
  --softmasking --cores 75 --gff3 \
  --prot_seq=/home/jon/Working_Files/apostichopus_japonicus/ajap_protein_uniprot.fasta \
  --prg=spaln 
```

exporting paths to spaln stuff
```bash
export ALN_DBS=/home/jon/anaconda3/envs/biotools/share/spaln/alndbs
export ALN_TAB=/home/jon/anaconda3/envs/biotools/share/spaln/table
```

error message thrown when running braker2
```bash
ERROR in file /home/jon/anaconda3/envs/biotools/bin/braker.pl at line 5289
failed to execute: perl /home/jon/anaconda3/envs/biotools/bin/startAlign.pl --genome=/home/jon/braker/genome.fa --prot=/home/jon/Working_Files/apostichopus_japonicus/ajap_protein_uniprot.fasta --ALIGNMENT_TOOL_PATH=/home/jon/anaconda3/envs/biotools/bin --prg=spaln --CPU=75 >> /home/jon/braker/errors/startAlign.stdout 2>>/home/jon/braker/errors/startAlign.stderr!
```

# Sun 20 Dec 2020 02:48:18 PM PST

making Augustus and Braker2 from github clones


also downloading echinoderm related orthoDB datasets

List of them
- Acanthaster planci - crown of thorns
- Strongylocentrotus purpuratus - purple sea urchin 
- Branchiostoma floridae - lancelet 
- Branchiostoma belcheri - lancelet
- Ciona intestinalis - vase tunicate
- Saccoglossus kowalevskii - acorn worm

downloading the metazoa orthoDB
```bash
wget https://v100.orthodb.org/download/odb10_metazoa_fasta.tar.gz
```

will also add the apostichopus japonicus genes to it. probably use the official release from ncbi instead of the uniprot dataset. 


cool, I added the A. japonicus proteins to the orthodb. Hopefully it doesn't complain about the names

trying to run braker2 now
```bash
./../BRAKER/scripts/braker.pl \
  --genome=/home/jon/Working_Files/star/assemblies/redundans_platanus-allee_step10_ajap-reference.masked_repeatmodeler-fam.fa \
  --prot_seq=/home/jon/Working_Files/braker2/metazoa/proteins.fasta \
  --bam=/home/jon/Working_Files/star/redundans_platanus-allee_step10_ajap-reference.masked_dfam/SRR1695477_untrimmed/SRR1695477_2_redun.bam,/home/jon/Working_Files/star/redundans_platanus-allee_step10_ajap-reference.masked_dfam/SRR1695477_untrimmed_mapped/SRR1695477_2_reduns.bam \
  --etpmode \
  --softmasking \
  --gff3 \
  --cores=70 \
  --species parastichopus_californicus \
  --PROTHINT_PATH=/home/jon/Working_Files/braker2/ProtHint/bin \
  --AUGUSTUS_CONFIG_PATH=/home/jon/Working_Files/braker2/Augustus/config

```

# Tue 22 Dec 2020 11:43:17 PM PST

wow.....that took awhile


# Mon 04 Jan 2021 10:13:36 PM PST

annd back to it. So way too many genes were predicted. I need to look into that. But for now, I'll run braker on the platanus-allee step 10 assembly next. 

```bash
./../BRAKER/scripts/braker.pl \
  --genome=//home/jon/Working_Files/star/assemblies/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam.fa \
  --prot_seq=/home/jon/Working_Files/braker2/metazoa/proteins.fasta \
  --bam=/home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/SRR1695477_untrimmed/SRR1695477_2_platanus-allee-step10_untrimmed.bam,/home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/SRR1139198_untrimmed/SRR1139198_2_platanus-allee-step10_untrimmed.bam \
  --etpmode \
  --softmasking \
  --gff3 \
  --cores=48 \
  --species parastichopus_californicus_using_platanus-allee-step10 \
  --PROTHINT_PATH=/home/jon/Working_Files/braker2/ProtHint/bin \
  --AUGUSTUS_CONFIG_PATH=/home/jon/Working_Files/braker2/Augustus/config

```

# Thu 07 Jan 2021 11:14:44 PM PST

annnnd finished

Still downloading the japonicus rna-seq data. Time to think about 

```bash
redundans assembly gene models: 53942
platanus assembly gene models: 52677
```

gotta figured out a way to get this lower. One thought was using blast and interpro hits to filter out extraneous stuff. But maybe using the japonicus data will help. 

# Thu 14 Jan 2021 06:27:19 PM PST

k, so i mapped an absurd amount of apostichopus japonicus rna-seq data to the platanus-allee step 10 parastichopus californicus genome assembly. Now to see if that improves the gene models at all.


```bash
./../../BRAKER/scripts/braker.pl \
  --genome=//home/jon/Working_Files/star/assemblies/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam.fa \
  --prot_seq=/home/jon/Working_Files/braker2/metazoa/proteins.fasta \
  --bam=/home/jon/Working_Files/star/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam/apostichopus_japonicus_rna-seq_2_pcali_assembly/Aligned.sortedByCoord.out.bam \
  --etpmode \
  --softmasking \
  --gff3 \
  --cores=48 \
  --species parastichopus_californicus_using_platanus-allee-step10_with_ajap-rnaseq \
  --PROTHINT_PATH=/home/jon/Working_Files/braker2/ProtHint/bin \
  --AUGUSTUS_CONFIG_PATH=/home/jon/Working_Files/braker2/Augustus/config
```

it probably wouldn't hurt to try some of the other modes with only protein data or just rna-seq and see how that impacts the gene models.

# Wed 20 Jan 2021 10:31:04 AM PST

annnd finished. that took awhile. 

taking a quick peek
```bash
grep ">" augustus.hints.aa | wc -l
```
annd now it's 
```bash
60846
```

so lets try it with just protein data

```bash
./../../BRAKER/scripts/braker.pl \
  --genome=/home/jon/Working_Files/star/assemblies/platanus-allee_illumina_step10.renamed.masked_repeatmodeler-fam.fa \
  --species=parastichopus_californicus_plat-allee-step10_prot-only \
  --prot_seq=/home/jon/Working_Files/braker2/metazoa/proteins.fasta \
  --softmasking \
  --gff3 \
  --cores=48 \
  --PROTHINT_PATH=/home/jon/Working_Files/braker2/ProtHint/bin \
  --AUGUSTUS_CONFIG_PATH=/home/jon/Working_Files/braker2/Augustus/config
```

depending on how this goes, I think I may run it with the apostichopus japonicus genome annotations next. Although, if I remember correctly, apostichopus japonicus is already included in the metazoa orthoDB I am using? 

# Sat 23 Jan 2021 12:17:20 PM PST

it finished. Checking number of gene models
```bash
51999
```

huh. So that helped. sorta. I guess this means I need to move on to functional annotation and using that to sort out the garbage. I should also set up apollo so I can actually look at the results. Just a reminder to myself also, I need to run braker2 using the last two braker2 commands on the redundans assembly also. 