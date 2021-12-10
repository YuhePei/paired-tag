##pre-processing

In this section, the shell scripts can't work well for datasets downloaded from SRA, so we need to change it slightly.

Take SRR12955974 file as example

Follow the pipeline until meeting the scripts `shellscrips/01.pre_process_paired_tag_fastq.sh`. Replace it as following codes:

```shell
reachtools combine2 SRR12955974
zcat SRR12955974_combined.fq.gz | bowtie /home/adminpku/paired_tag_data/Paired-Tag-master/refereces/cell_id - --norc -m 1 -v 1 -S SRR12955974_BC.sam
perl /home/adminpku/paired_tag_data/Paired-Tag-master/perlscripts/sam2covFastq.pl SRR12955974_BC.sam
```

when mapping, we need to download the mm10 reference genome

```shell
#下载并解压mm10 genome
cd reference
wget http://hgdownload.cse.ucsc.edu/goldenPath/mm10/bigZips/chromFa.tar.gz
tar zvfx chromFa.tar.gz
cat *.fa > mm10.fa
rm chr*.fa
# 构建bowtie索引   *bowtie2 所花时间较长，建议用丢后台，同时注意给输出文件文件名前缀*
time bowtie2-build mm10.fa ~/paired_tag_data/Paired-Tag-master/refereces/mm10.fa 1>mm10.bowtie_index.log 2>&1 &
```

the scripts 'shellscrips/02.proc_DNA.sh' also can't work.

so try the following scripts:
```
trim_galore SRR12955974_BC_cov.fq.gz



