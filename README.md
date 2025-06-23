# 🧬 `my_minion`🦠 

## Basecalling with Oxford Nanopore MinION

This repository provides scripts and setup instructions for **basecalling raw FAST5 files** from MinION sequencing using **Dorado**, Oxford Nanopore’s high-accuracy basecaller. These instructions are optimized for **Intel-based MacBooks** via **Docker**.

## 📖 What is Basecalling?

Nanopore sequencing works by passing a single DNA or RNA molecule through a tiny nanopore with an ionic current flowing across it. As the molecule transits the nanopore, it disrupts the ionic current, producing characteristic changes in the electrical signal known as a **“squiggle.”**

The MinKNOW™ software captures this squiggle in real time. Each squiggle corresponds to a single strand of DNA or RNA, known as a **read**, which may include canonical bases (A, T, C, G) and modified bases such as methylation.

**Basecalling** is the process of decoding these raw electrical signals to determine the nucleotide sequence of the DNA or RNA strand. Basecalling algorithms analyze the squiggle to produce readable DNA/RNA sequences in standard formats like FASTQ or BAM.

Modern nanopores like the R10 model have multiple sensing regions inside the pore, improving signal accuracy, especially in regions of repeated bases.

Oxford Nanopore basecalling software (e.g., **Dorado**) employs neural networks—machine learning models inspired by biological neural networks—that learn to interpret complex signal patterns and improve over time, delivering increasingly accurate basecalling.

## 📂 Data Formats from MinKNOW™

When running a sequencing experiment with Oxford Nanopore's MinKNOW™, you will encounter different data formats for raw and processed reads:

### Raw Data Format

- **FAST5 files**  
  These files contain the raw electrical signal data (the "squiggle") recorded as DNA or RNA molecules pass through the nanopore.  
  FAST5 files are based on the hierarchical HDF5 file format and store complex signal data.

### Basecalled Reads Format

- **FASTQ files**  
  After basecalling, the raw signal data is decoded into nucleotide sequences with associated quality scores. These are stored in FASTQ format, which is standard for sequence reads.

- **BAM files** (optional)  
  Sometimes basecalled reads are aligned to a reference genome and saved in BAM format, a compressed binary version of the SAM alignment format, used for downstream analyses.

### Summary Table

| Data Type           | File Format | Description                               |
|---------------------|-------------|-------------------------------------------|
| Raw signal data     | FAST5       | Raw electrical signals from the nanopore  |
| Basecalled reads    | FASTQ       | Nucleotide sequences + quality scores     |
| Aligned reads (optional) | BAM       | Aligned sequences for further analysis    |

---

## 🍏 Dorado Setup on macOS

### ✅ Requirements

- A Mac with **Apple Silicon** (M1, M2, or M3 series)
- **macOS 13 or later** (Ventura or Sonoma)
- At least **16 GB of unified memory** (recommended for viral or large datasets)
- Terminal access (e.g., iTerm or Terminal.app)

> 💡 If you're using Apple Silicon, visit the [Dorado GitHub Releases](https://github.com/nanoporetech/dorado/releases), download the latest macOS release, and follow the official setup instructions.

If you're on an **Intel-based Mac** running **macOS 13+**, you’ll need to install and run **Dorado inside a Docker container** (see below).

---

## 🐳 Dorado Installation via Docker (Intel Mac)

Dorado is not natively optimized for Intel-based Macs, but you can run it in a **Linux-based Docker container**. This method supports **CPU-only basecalling** for systems without a GPU.

### ⚙️ Step-by-Step Installation

#### 1. Install Docker (if not already installed)

Download and install Docker Desktop:  
👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

#### 2. Run Dorado in Docker

You can launch Dorado from the terminal using:

```bash
docker run nanoporetech/dorado
```
Examples:

To **list available basecalling models**, run:

```bash
docker run nanoporetech/dorado list-models
```

To **perform basecalling**, use:

```bash
docker run \
    --gpus 1 \
    -v $PWD:$PWD \
    nanoporetech/dorado \
    dorado basecaller "/models/dna_r10.4.1_e8.2_400bps_hac@v3.5.2" $PWD/fast5/ \
> output.sam
```
> 🔍 Adjust the model path and input/output directories as needed for your dataset.

## Perform basecalling

### 📖 Basecalling with RAMSES – University of Cologne

At the University of Cologne (Uni Köln), **RAMSES** environment is used to facilitate this step in a standardized and reproducible way.



---
### 📚 References

1. **Dorado – Oxford Nanopore Basecaller**  
   GitHub repository: [https://github.com/nanoporetech/dorado](https://github.com/nanoporetech/dorado)  
   Official basecaller for Oxford Nanopore Technologies, supporting both CPU and GPU basecalling with high accuracy.

2. **Oxford Nanopore Technologies Documentation**  
   Main documentation hub: [https://community.nanoporetech.com/](https://community.nanoporetech.com/)  
   Includes setup guides, protocols, software updates, and best practices.
   
3. **Docker**  
  🔗 [Getting Started Guide](https://www.docker.com/get-started)

4. **Docker Hub - nanoporetech/dorado**  
  🔗 [Docker Image](https://hub.docker.com/r/nanoporetech/dorado)

5. **Minimap2: Pairwise alignment for nucleotide sequences**  
   Li, H. (2018). *Bioinformatics*, 34(18), 3094–3100.  
   DOI: [10.1093/bioinformatics/bty191](https://doi.org/10.1093/bioinformatics/bty191)  
   Widely used aligner for long noisy reads from Nanopore and PacBio.

6. **edgeR: Differential expression analysis of digital gene expression data**  
   Robinson, M. D., McCarthy, D. J., & Smyth, G. K. (2010). *Bioinformatics*, 26(1), 139–140.  
   DOI: [10.1093/bioinformatics/btp616](https://doi.org/10.1093/bioinformatics/btp616)

7. **DESeq2: Moderated estimation of fold change and dispersion**  
   Love, M. I., Huber, W., & Anders, S. (2014). *Genome Biology*, 15(12), 550.  
   DOI: [10.1186/s13059-014-0550-8](https://doi.org/10.1186/s13059-014-0550-8)

8. Nick Laurenz Kaiser, Martin H Groschup, Balal Sadeghi, VirDetector: a bioinformatic pipeline for virus surveillance using nanopore sequencing, Bioinformatics, Volume 41, Issue 2, February 2025, btaf029, https://doi.org/10.1093/bioinformatics/btaf029

9. https://gitlab.git.nrw/uzk-itcc-hpc/itcc-hpc-ramses/-/wikis/Documentation#22-ssh-access-keys-and-things
