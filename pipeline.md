##pre-processing

In this section, the shell scripts can't work well for datasets downloaded from SRA, so we need to change it slightly.

Take SRR12955974 as example

```
reachtools combine2 SRR12955974
zcat SRR12955974_combined.fq.gz | bowtie /home/adminpku/paired_tag_data/Paired-Tag-master/refereces/cell_id - --norc -m 1 -v 1 -S SRR12955974_BC.sam
perl /home/adminpku/paired_tag_data/Paired-Tag-master/perlscripts/sam2covFastq.pl SRR12955974_BC.sam
```
