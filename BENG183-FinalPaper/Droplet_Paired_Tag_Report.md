# Droplet Paired Tag

**Asees Aulakh, Jennifer Kwok, Peter Wang**

------------------------------------------------------------------------

## Introduction + Background

Droplet Paired Tag combines two very powerful tools: cell expression
levels and chromatin markers. Before, scientists would use scRNA to find
cell expression levels and ChIP seq to measure the chromatin state
separately. This creates a large gap of information. We know which genes
are turned on and which regulatory marks sit on the chromatin, but it's
difficult to understand how a histone mark actually drives gene
expression in a specific cell type. Droplet Paired Tag fixes this
problem, combining both tools so we can understand how cells respond to
signals, differentiate, or even how they become dysfunctional in
diseases. This allows us to ask more questions like 'Which chromatin
states directly correspond to transcriptional activation?' and 'How does
a specific histone modification change gene activity?'

The first combination of scRNA and ChIP-seq is a tool named Paired Tag.
This was a huge breakthrough in the scientific community because it
allowed researchers to connect regulatory elements and gene activity in
a specific cell. While being an extremely useful tool, there were large
limitations to the Paired Tag workthrough. The multiday workflow, low
throughput, and complex steps led many scientists to be unable to
replicate the workflow. These challenges led to the development of
Droplet Paired Tag.

Droplet Paired Tag condensed the barcoding into a singular step, smaller
workflow, and uses microfluidic droplets to make the process more
scalable. Changing the throughput from only 100s of cells to 10,000+
cells analyzed at once. This new method keeps all the benefits of its
predecessor: simultaneous measurement of gene expression and histone
markers and removes the complexity. This new method has made single cell
chromatin combined with RNA profiling more powerful and accessible.

------------------------------------------------------------------------

## Workflow

### Wet Lab

![Wet Lab Workflow](download.png)

The first step to using Droplet Paired Tag to study cells is to prepare
nuclei for barcoding. First, we permeabilize cells so the nucleus is
accessible, as we will be working within the nucleus. Then, with the
nuclei isolated, we use antibodies that recognize specific histone
modifications. For example, the specific chromatin feature H3K27ac which
indicates active enhancers and promoters is identified by antibodies
that bind to only where the H3K27ac mark is present. The antibodies are
then a guide for transposase, which cuts DNA and inserts short adapter
sequences only at the places where the antibody is bound, which is where
the marked sites are. Now the chromatin fragments are tagged. At the
same time, we lyse the nucleus to the point where mRNA spills out, so
now each nucleus contains tagged chromatin fragments and free mRNA
molecules. The chromatin fragments are ready for barcoding.

The "droplet step" involves pairing nuclei with barcoded gel beads in a
microfluidic system. In every droplet, there is one nucleus and one bead
coated with oligos that all share the same cell barcode. Each droplet
becomes a tiny reaction chamber with its own unique label. Then, inside
each droplet, mRNA is copied into cDNA by reverse transcriptase, and the
cDNA attaches to primers on the bead which carry the cell barcode. With
this, all transcripts from that nucleus share the same barcode. From
here, the chromatin fragments that already carry sequencing adapters
from the transposase step also bind to the same barcode. Eventually when
droplets are broken, we end up with barcoded cDNA and barcoded chromatin
fragments. These form separate sequencing libraries, but because they
share a barcode, we can match the RNA and chromatin to the same cell
during analysis.

------------------------------------------------------------------------

## Software/Tools

![Software Visualization 1](download-1.jpg) ![Software Visualization
2](download.jpg)

### Seurat

Seurat is an R package that was originally developed for analyzing
single-cell RNA-seq data. It helps in providing tools for normalization,
clustering, dimensionality reduction, and visualization. Droplet
Paired-Tag generates thousands of paired single-cell RNA and chromatin
profiles, producing massive datasets that require careful integration.
Seurat, in combination with Signac, provides the computational framework
to analyze these paired datasets, linking gene expression with chromatin
state for each cell. Before this integrated approach, it was challenging
to jointly interpret RNA and chromatin data at single-cell resolution.
By using Seurat, researchers can cluster cells, identify cell types, and
correlate chromatin accessibility or histone modifications with
transcriptional activity, allowing Droplet Paired-Tag data to reveal
regulatory mechanisms underlying cellular identity and function.

### Signac

Signac is a R package that was built for analyzing single cell chromatin
data analysis such as scATAC-seq, CUT&Tag, Paired-Tag, and Droplet
Paired-Tag. Before Signac, there was no pipeline to process single cell
chromatin data, making it difficult to analyse. The developed pipeline
to fix this problem was Signac. Signac is an end-to-end framework that
was developed in 2021 by Tim Stuart and coworkers. It is paired with
Seurate to enable integrated single-cell chromatin and gene expression
analysis within one unified model. Droplet Paired Tag generates
thousands of single cell chromatin and RNA profiles, Dignac provides
researchers the computational structure needed to analyze the chromatin
side of the data.

------------------------------------------------------------------------

## Workflow for Seurat and Signac

**Input Files:** Droplet Paired-Tag generates paired single-cell RNA-seq
and chromatin fragment datasets. The chromatin fragment dataset includes
every DNA fragment accompanied by a barcode indicating its originating
cell. The RNA matrix holds gene expression counts corresponding to each
cell.

**Data Organization:** The chromatin fragment files are loaded into
Signac. Saved in a designated ChromatinAssay object, inside a Seurat
object. This configuration allows for analysis of chromatin and RNA
data.

**Peak Identification and Matrix Formation:** Scientists detect regions,
with enrichment termed as peaks. They create a cell x peak count matrix,
with each entry representing the number of chromatin fragments from a
cell located within each peak.

**Quality/ Filtering:** This stage eliminates cells of low quality by
employing metrics such as TSS enrichment, nucleosome signal and fragment
length distributions. This ensures that only nuclei of quality proceed
to subsequent analysis.

**Normalization and Dimensionality Reduction:** The peak count matrix
undergoes normalization through TF-IDF, which scales features according
to their significance, across cells. Subsequently Latent Semantic
Indexing (LSI) is applied to reduce dimensionality and highlight the
factors driving variation in the chromatin data.

**Cell Type Identification:** Cells group together according to
comparable chromatin accessibility profiles. Scientists label these
groups using established marker genes, gene activity metrics or the
linked RNA expression information, from Droplet Paired-Tag.

**Motif and Regulatory Analysis:** Signac performs motif enrichment,
transcription factor footprinting and peak-to-gene association to infer
which regulatory components govern gene expression.

**Multimodal Integration:** Signac works within Seurat, so chromatin
data can directly connect with paired scRNA-seq data. This lets
researchers link regulatory elements, chromatin state, and gene
expression at the single-cell level, uncovering specific regulatory
mechanisms for different cell types.

Droplet Paired Tag generates intricate chromatin and RNA data, to make
sense of all the information at a single cell level requires tools built
for multi-omnic analysis. Signac helps researchers link histone marks
directly to gene expression patterns. By integrating with Seurat,
scientists can cluster cells, identify regulatory elements, and connect
them to gene regulation. Overall, Seurat and Signac trunks complex
paired chromatin RNA datasets into interpretable maps of cell states.
This helps us better understand how cells change, respond to different
signals, and even become dysregulated from disease.

------------------------------------------------------------------------

## Importance/Conclusion

![Biological Application](download-2.jpg)

Droplet Paired-Tag is an important component of bioinformatics and RNA
sequencing because it allows researchers to see both gene expression and
chromatin changes in the same single cell, instead of studying them
separately. This gives a much clearer picture of how genes are actually
regulated inside real biological systems. By connecting histone
modifications directly to gene activity, scientists can better
understand how cells develop, specialize, and respond to different
signals.

This technology is especially useful for studying diseases, because many
of them are caused by changes in gene regulation rather than changes in
the DNA sequence itself. With Droplet Paired-Tag, researchers can
identify which cell types are affected and how abnormal chromatin states
lead to faulty gene expression. This can help guide the development of
more precise, targeted treatments in the future. Tools like Seurat and
Signac make it possible to handle and interpret the massive amount of
data that Droplet Paired-Tag produces. This software is crucial in
allowing researchers to organize the data, group similar cells together,
and connect chromatin changes to gene expression in a clear and
impactful way.

Overall, Droplet Paired-Tag brings together powerful lab techniques and
advanced computational tools, giving scientists and bioinformaticians a
much stronger method to explore how cells work, diseases develop, and
how we might find better treatment methods in the future.

------------------------------------------------------------------------

## Sources

Stuart, Tim, et al. "Single-Cell Chromatin State Analysis with Signac."
Nature Methods, U.S. National Library of Medicine, Nov. 2021,
pmc.ncbi.nlm.nih.gov/articles/PMC9255697/.

Xie, Yang, et al. "Droplet-Based Single-Cell Joint Profiling of Histone
Modifications and Transcriptomes." Nature Structural & Molecular
Biology, U.S. National Library of Medicine, Oct. 2023,
pmc.ncbi.nlm.nih.gov/articles/PMC10584685/#Sec3.

