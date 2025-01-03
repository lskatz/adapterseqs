# Adapter sequences

This is a repo to hold adapter sequences for genome sequencing.
Please see [LICENSE.md](/LICENSE.md) if you use this repo.

## Adapters

| File | provenance |
| ---- | ---------- |
| NexteraPE-PE.fa | [Trimmomatic](https://github.com/usadellab/Trimmomatic) [^1] |
| ONT.fa | [Porechop fork](https://github.com/BioWilko/Porechop) |
| TruSeq2-PE.fa | [Trimmomatic](https://github.com/usadellab/Trimmomatic) [^1] |
| TruSeq2-SE.fa | [Trimmomatic](https://github.com/usadellab/Trimmomatic) [^1] |
| TruSeq3-PE-2.fa | [Trimmomatic](https://github.com/usadellab/Trimmomatic) [^1] |
| TruSeq3-PE.fa | [Trimmomatic](https://github.com/usadellab/Trimmomatic) [^1] |
| TruSeq3-SE.fa | [Trimmomatic](https://github.com/usadellab/Trimmomatic) [^1] |

[^1] Oligonucleotide sequences © 2023 Illumina, Inc. All rights reserved.

## Usage

This repo is meant to be used with external analysis tools.
The following are suggestions.
Please see the license in this repo if you use these or any tools in conjunction with the sequences.

_NOTE_ I have not tried these specific examples, but there are working examples in [the workflows](/.github/workflows).

### Trimmomatic

This repo uses the sequences found from the [Trimmomatic](https://github.com/usadellab/Trimmomatic) repo itself which in turn contains the same Illumina license as found here.

Example using `TruSeq2-PE.fa` found in this repo (and in the Trimmomatic repo).

```bash
trimmomatic PE -phred33 \
  R1.fastq.gz R2.fastq.gz \
  R1_paired.fastq.gz R1_unpaired.fastq.gz \
  R2_paired.fastq.gz R2_unpaired.fastq.gz \
  ILLUMINACLIP:TruSeq2-PE.fa:2:30:10 \
  LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
```

### Cutadapt

```bash
cutadapt -a file:TruSeq2-PE.fa \
-A file:TruSeq2-PE.fa \
-o R1_trimmed.fastq.gz -p R2_trimmed.fastq.gz \
R1.fastq.gz R2.fastq.gz
```

### Fastp

```bash
fastp -i R1.fastq.gz -I R2.fastq.gz \
-o R1_trimmed.fastq.gz -O R2_trimmed.fastq.gz \
--adapter_sequence file:TruSeq2-PE.fa \
--adapter_sequence_r2 file:TruSeq2-PE.fa \
--cut_tail
```

### BBDuk

```bash
bbduk.sh in1=R1.fastq.gz in2=R2.fastq.gz \
out1=R1_trimmed.fastq.gz out2=R2_trimmed.fastq.gz \
ref=TruSeq2-PE.fa ktrim=r k=23 mink=11 hdist=1 qtrim=rl trimq=10 minlength=36
```
