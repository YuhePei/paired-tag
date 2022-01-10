#### pre-processing

In this section, the shell scripts can't work well for datasets downloaded from SRA, so we need to change it slightly.

Take SRR12955974 file as example

Follow the pipeline until meeting the scripts `shellscrips/01.pre_process_paired_tag_fastq.sh`. Replace it as following codes:

```shell
reachtools combine2 SRR12955974
zcat SRR12955974_combined.fq.gz | bowtie /home/adminpku/paired_tag_data/Paired-Tag-master/refereces/cell_id - --norc -m 1 -v 1 -S SRR12955974_BC.sam
perl /home/adminpku/paired_tag_data/Paired-Tag-master/perlscripts/sam2covFastq.pl SRR12955974_BC.sam
```

#### mapping

Firstly, we need to download the mm10 reference genome.

```shell
#下载并解压mm10 genome
cd reference
wget http://hgdownload.cse.ucsc.edu/goldenPath/mm10/bigZips/chromFa.tar.gz
tar zvfx chromFa.tar.gz
cat *.fa > mm10.fa
rm chr*.fa
# 构建bowtie索引   *bowtie2 所花时间较长，建议用丢后台，同时注意给输出文件文件名前缀*
time bowtie2-build mm10.fa mm10 &
```

The scripts 'shellscrips/02.proc_DNA.sh' also can't work.

So try the following scripts:
```
trim_galore SRR12955974_BC_cov.fq.gz
#bowtie2 对比也很花时间，不用问怎么知道的，能丢后台就丢吧，摊手
bowtie2 -x ~/paired_tag_data/Paired-Tag-master/refereces/mm10 -U SRR12955974_BC_cov_trimmed.fq.gz --no-unal -p 8 -S SRR12955974_mm10.sam
#bowtie2 这里试了几次没有成功，文件总是非常小，现在已经排除了索引的问题，怀疑可能是上一步的文件出错了，准备重新那一组数据来试一试




