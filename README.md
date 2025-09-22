[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/aKWLU3-A)

# Week 2: Technology brainstorm group 1R
**Group member GitHub usernames**: CeliaPolanco, chiarakehl

## Exercise A
**Technology**: flow cytometry

**Description**: Flow cytometry is a technique used to rapidly measure and analyze physical and chemical characteristics of individual cells or particles suspended in a fluid. The sample is pushed through a narrow stream so that cells pass single file through a laser beam. As each cell is hit by the laser, detectors measure scattered light (which gives information about size and internal complexity) and fluorescence (from dyes or antibodies bound to specific molecules). The signals are collected electronically, allowing simultaneous measurement of multiple parameters for thousands of cells per second. This enables detailed population analysis, such as identifying cell types, measuring protein expression, or detecting rare cell subsets.

**Source**: ChatGPT, "write a short summary of how flow cytometry works"



## Exercise B
**Technology**: Pool-seq

**Description**: Pool-seq (pooled sequencing) is a genomic approach where DNA from many individuals within a population is combined into a single sample and sequenced together, rather than sequencing each individual separately. This method provides estimates of allele frequencies across the population at a much lower cost and effort compared to individual sequencing. While it does not preserve information about individual genotypes, Pool-seq is powerful for studying population genetics, such as detecting selection, estimating genetic diversity, or identifying population structure.

**Source**: ChatGPT, "write a short paragraph of how pool-seq works"

**Application**: Pool-seq´s diverse applications include estimating allele frequencies across populations, tracking evolutionary changes over time, detecting signatures of natural selection, assessing genetic diversity and population structure, monitoring low-frequency or rare variants, and studying structural variants such as transposable element insertions or copy-number changes. In Kofler et al. (2012), Pool-seq was used to study transposable element dynamics in a natural population of Drosophila melanogaster, revealing a large number of both known and novel insertions and showing how their frequencies are influenced by selection, recombination, and TE family characteristics.

Examples of Population Genetic Analyses (to estimate population-level metrics) using pool-seq Data:
- Allele frequency spectrum (AFS): Estimate the distribution of allele frequencies across loci.
- Nucleotide diversity (π) and heterozygosity: Compute from read counts (PoPoolation provides scripts for this)
- Tajima’s D, F_ST, and other statistics: PoPoolation2 allows sliding-window calculations of F_ST, π, and Tajima’s D from pooled data.
- Detecting selection:
- Temporal analysis: Compare allele frequencies across time points.
    - Extreme frequency shifts: Identify loci with unusually large frequency changes.
    - F_ST (Fixation Index) outliers: Detect loci under divergent selection.

**Sources:** ChatGPT, "Write a short paragraph explaining the different applications of pool-seq and include as an example the application of pool-seq of the paper: Robert Kofler, Andrea J. Betancourt, and Christian Schlötterer, “Sequencing of Pooled DNA Samples (Pool-Seq) Uncovers Complex Dynamics of Transposable Element Insertions in Drosophila Melanogaster,” PLoS Genet 8, no. 1 (January 26, 2012): e1002487, doi:10.1371/journal.pgen.1002487."

Robert Kofler, Andrea J. Betancourt, and Christian Schlötterer, “Sequencing of Pooled DNA Samples (Pool-Seq) Uncovers Complex Dynamics of Transposable Element Insertions in Drosophila Melanogaster,” PLoS Genet 8, no. 1 (January 26, 2012): e1002487, doi:10.1371/journal.pgen.1002487.
https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1002487

**Statistical analysis:** Assuming the application: Temporal analysis of allele frequencies as a measure of divergence (to detect loci under selection). The goal is to identify loci that show unusually large allele frequency changes (Δ𝑝) over time, which may indicate divergent or directional selection. Since Pool-seq provides allele counts from pooled DNA, statistical methods must account for sampling noise, sequencing error, and the stochasticity of allele frequency changes due to drift.

1) Modelling Neutral Drift
    To test for selection, we need a null expectation of allele frequency changes under genetic drift:
    - Binomial model: allele counts are modeled as a binomial sampling of the population allele frequency.
    - Beta-binomial model: used when there is overdispersion due to Pool-seq sampling or PCR biases.
    - Wright-Fisher simulations: simulate allele frequency trajectories under drift given effective population size (𝑁𝑒) and number of generations (𝑡).

2) Statistical Testing for Divergent Selection
    - Approach 1: Outlier detection
        - Compare the observed Δ𝑝 at each locus to the null distribution under drift.
        - Loci with extreme Δ𝑝 values (e.g., top 1% genome-wide) are candidate targets of selection.
    - Approach 2: Likelihood / Bayesian methods
        - Compute the likelihood of observed allele counts at multiple time points under models of drift vs selection:
          - 𝐿(data∣𝑠)=∏𝑡Binomial(reads supporting allele∣coverage,𝑝𝑡(𝑠))
              - 𝑠 = selection coefficient;
              - 𝑝𝑡(𝑠) = expected allele frequency at time 𝑡 under selection.
        - Use likelihood ratio tests or Bayesian posterior probabilities to test whether 𝑠 ≠ 0.
    - Approach 3: Regression / Generalized Linear Models
        - Treat allele frequency changes as a function of time and fit a GLM with a binomial (or beta-binomial) error structure.
        - Loci with significant temporal trends can be considered candidates for directional selection.

3) Multiple Testing Correction
   - Since thousands to millions of loci are tested, control the false discovery rate (FDR) using methods like Benjamini-Hochberg.
   - Only loci exceeding the significance threshold after correction are considered robust candidates for divergent selection.
