# ![nf-core/slamseq](docs/images/nf-core-slamseq_logo.png)

[![Nextflow](https://img.shields.io/badge/nextflow-%E2%89%A519.10.0-brightgreen.svg)](https://www.nextflow.io/)


# Custom Yeast SLAMseq Setup (Mattfeng414 Fork)

> **Note**: For full pipeline documentation and usage, please see the [original nf-core/slamseq README](https://github.com/nf-core/slamseq#readme).

This fork introduces the following customizations optimized for _Saccharomyces cerevisiae_ SLAMseq data:

- **Reference header renaming**: Updated `reference/yeast.fa` headers from NCBI accessions to `>chrI…chrXVI` to match custom BED intervals. Reference genome obtained from the Saccharomyces Genome Database (SGD).
- **Custom 3′-UTR BED**: `bed/S10-3UTR_reading_windows.bed` covering all nuclear chromosomes for UTR rate calculations. Original intervals defined in Alalam H, Zepeda-Martínez JA, Sunnerhagen P. _Global SLAM-seq for accurate mRNA decay determination and identification of NMD targets._ RNA. 2022 Jun;28(6):905-915. doi:10.1261/rna.079077.121. PMCID: PMC9074897.
- **Resource caps** via `local.config`:
  ```groovy
  process {
    cpus   = 4
    memory = '16 GB'
  }
  ```
- **Read-length override**: Use `--read_length 109` to match trimmed read length and avoid array bounds errors in `tcperreadpos`.
- **Skip DESeq2**: Default `--skip_deseq2` for analyses focusing on raw conversion rates without differential expression.

---

## Quick Launch

```bash
cd /path/to/slamseq
conda deactivate   # Ensure Java 11–18 is on PATH

nextflow run . \
  --input        samples.tsv \
  --fasta        reference/yeast.fa \
  --gtf          reference/yeast.gtf \
  --bed          bed/S10-3UTR_reading_windows.bed \
  --skip_deseq2 \
  --max_cpus     4 \
  --max_memory   16.GB \
  --read_length  109 \
  -profile       docker \
  --outdir       results
```

These settings have been validated on macOS with Docker and Java 17.


