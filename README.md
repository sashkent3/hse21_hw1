# Список всех команд, которые были выполнены на сервере
```
seqtk sample -s309 /usr/share/data-minor-bioinf/assembly/oil_R1.fastq 5000000 > oil_R1.fastq
seqtk sample -s309 /usr/share/data-minor-bioinf/assembly/oil_R2.fastq 5000000 > oil_R2.fastq
seqtk sample -s309 /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq 1500000 > oilMP_S4_L001_R1_001.fastq
seqtk sample -s309 /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq 1500000 > oilMP_S4_L001_R2_001.fastq

mkdir fastqc
fastqc -o fastqc *.fastq
mkdir multiqc
multiqc -o multiqc fastqc

platanus_trim oil_R1.fastq oil_R2.fastq 
platanus_internal_trim oilMP_S4_L001_R1_001.fastq oilMP_S4_L001_R2_001.fastq 
rm oilMP_S4_L001_R1_001.fastq oilMP_S4_L001_R2_001.fastq oil_R1.fastq oil_R2.fastq

mkdir fastqc_trimmed
fastqc -o fastqc_trimmed *trimmed
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed

platanus assemble -f *.trimmed

platanus scaffold -c out_contig.fa -IP1 oil_R1.fastq.trimmed oil_R2.fastq.trimmed -OP2 oilMP_S4_L001_R1_001.fastq.int_trimmed oilMP_S4_L001_R2_001.fastq.int_trimmed

platanus gap_close -c out_scaffold.fa -IP1 oil_R1.fastq.trimmed oil_R2.fastq.trimmed -OP2 oilMP_S4_L001_R1_001.fastq.int_trimmed oilMP_S4_L001_R2_001.fastq.int_trimmed
rm *.trimmed *.int_trimmed
```

# Скриншоты и статистика из файлов multiQC