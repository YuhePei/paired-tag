#### download sra data from NCBI


```shell
mkdir histone
cd histone
nohup prefetch --option-file left -O .
```
#### convert sra file to fq.gz file

```shell
sh sra_convert.sh
```
#### get matrix and fragment file for each sra data


```shell
mkdir fragement
mkdir matrix
sh run.sh

```
#### merge all bed file and make tbi file

```shell
cd fragement
sh bed_merge.sh histone #for single histone marker
sh bed2mod_merge.sh histone1 histone2 #for 2 histone marker
```




