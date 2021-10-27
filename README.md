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

echo scaffold6_cov232 > longest_id.txt
seqtk subseq out_gapClosed.fa longest_id.txt > longest.fasta
```

# Скриншоты и статистика из файлов multiQC
## До подрезания [(полный отчет)](https://github.com/sashkent3/hse21_hw1/blob/main/multiqc/multiqc_report.html)
![GenStat](https://user-images.githubusercontent.com/33320473/139153418-7ba71d98-7478-427a-82a7-2c754ef2d304.png)
![SeqCnt](https://user-images.githubusercontent.com/33320473/139153427-24e42738-3995-42b3-95b6-f5c1cc2b09eb.png)
![SeqQual](https://user-images.githubusercontent.com/33320473/139153434-3211c2b1-de69-402c-ba0f-147b64e0bcc8.png)
## После подрезания [(полный отчет)](https://github.com/sashkent3/hse21_hw1/blob/main/multiqc_trimmed/multiqc_report.html)
![GenStat](https://user-images.githubusercontent.com/33320473/139153501-4673ce41-13e8-4a9d-94a4-976a9f8de473.png)
![SeqCnt](https://user-images.githubusercontent.com/33320473/139153519-80a777a4-8a30-4668-9c4e-75310688b49f.png)
![SeqQual](https://user-images.githubusercontent.com/33320473/139153531-80cc6217-e602-442a-8aa2-46292517354c.png)
