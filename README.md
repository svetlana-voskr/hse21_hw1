# Домашнее задание 1
## Часть 1
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq 

seqtk sample -s503 oil_R1.fastq 5000000 > sub1.fq
seqtk sample -s503 oil_R2.fastq 5000000 > sub2.fq
seqtk sample -s503 oilMP_S4_L001_R1_001.fastq 1500000 > sub1_MP.fq
seqtk sample -s503 oilMP_S4_L001_R2_001.fastq 1500000 > sub2_MP.fq

mkdir fastqc
ls sub* | xargs -tI{} fastqc -o fastqc {}

mkdir multiqc
multiqc -o multiqc fastqc

platanus_trim sub1.fq sub2.fq
platanus_internal_trim sub1_MP.fq sub2_MP.fq

rm sub1.fq
rm sub2.fq
rm sub1_MP.fq
rm sub2_MP.fq

mkdir trimmed_fastqc
ls sub* | xargs -tI{} fastqc -o trimmed_fastqc {}

mkdir trimmed_multiqc
multiqc -o trimmed_multiqc trimmed_fastqc

platanus assemble -f sub1.fq.trimmed sub2.fq.trimmed 2> assemble.log
grep '^>' out_contig.fa | wc -
```
Количество контигов 610
```
platanus scaffold -c out_contig.fa -IP1 sub1.fq.trimmed sub2.fq.trimmed -OP2 sub1_MP.fq.int_trimmed sub2_MP.fq.int_trimmed 2> scaffold.log
```
Количество скаффолдов 75
```
platanus gap_close -c out_scaffold.fa -IP1 sub1.fq.trimmed sub2.fq.trimmed -OP2 sub1_MP.fq.int_trimmed sub2_MP.fq.int_trimmed 2> gapclose.log

echo scaffold1_cov231 > seq_names.lst
seqtk subseq out_gapClosed.fa seq_names.lst > longest.fa
```
## Анализ multiqc 

## Вторая часть
