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
```
