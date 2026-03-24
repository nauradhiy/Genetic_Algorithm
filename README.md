# Lung Cancer Detection via miRNA: Genetic Algorithm-Based DNA Probe Design for ITB iGEM Team

## Project Context
This research was developed as a direct computational contribution to the ITB iGEM (International Genetically Engineered Machine) competition team, representing Institut Teknologi Bandung in one of the world's most prestigious synthetic biology competitions. The DNA probe sequences produced in this project were designed in close collaboration with ITB Microbiology Department team, who defined the biological parameters and requirements the sequences must satisfy. The mathematically optimized sequences are intended for wet laboratory research in Singapore, where experimental costs are exceptionally high, making computational pre-validation not just beneficial but financially critical before any physical synthesis is ordered.

## Problem Statement
The ITB iGEM team requires a specific DNA probe sequence capable of hybridizing with miRNA-20a, a biological marker overexpressed in lung cancer patients, as part of a **Rolling Circle Amplification (RCA) based detection system**. The core challenge is that designing DNA sequences through **random trial-and-error carries a significant risk of experimental failure**, and each failed attempt translates directly into **wasted research budget in Singapore** where laboratory procedures are extremely costly. The team needed a rigorous computational method to generate DNA probe candidates that already satisfy all defined biological and thermodynamic parameters before any physical experiment is conducted, **eliminating unnecessary cost from poorly designed sequences.**

## Why This Research Matters
**miRNA-20a** has been identified as a biological marker that is significantly more abundant in lung cancer patients compared to healthy individuals, making it a promising early detection target. However, designing a DNA probe that can hybridize specifically and stably with this miRNA is a **highly complex optimization problem** that manual or simple software-based approaches cannot reliably solve. Without a rigorous computational framework, probe design remains trial-and-error, limiting the development of portable, affordable, and accurate diagnostic devices.

Every DNA sequence that **fails to meet the biological criteria upon synthesis** means the entire Singapore laboratory procedure must be repeated at substantial cost. Mathematical modeling through **genetic algorithm optimization** bridges this gap by simulating thousands of candidate sequences computationally, filtering them against strict biological constraints, and delivering only the sequences most likely to perform correctly in the actual laboratory setting. This transforms an expensive, uncertain experimental process into a **cost-controlled, evidence-based workflow.**

## Key Research Objectives
| Objective | Research Question |
| --------- | ----------------- |
| Deliver Lab-Ready Sequences | How can DNA probe sequences be computationally designed to meet all parameters set by the Microbiology team before physical synthesis? |
| Minimize Experimental Failure Risk | How can optimization reduce the probability of sequence-related failure in Singapore laboratory procedures? |
| Thermodynamic Stability | How can GC content and free energy (ΔG) be optimized for stable and efficient DNA-miRNA hybridization? |
| Selectivity Assurance | How can specificity toward miRNA-20a be guaranteed while preventing binding with non-target miRNA sequences? |
| Secondary Structure Prevention | How can unwanted secondary structures in the dumbbell DNA probe be avoided prior to physical synthesis? |

## Key Analyses
1. **Biological Parameter Modeling**\
Encoded four critical biological constraints from the Department of Microbiology at ITB: GC content (40%–60%), free energy (ΔG) target of -15 kcal/mol, absence of homopolymer runs of 4 or more nucleotides, and no reverse-complementary subsequences of 2–7 nucleotides in the loop.
2. **Free Energy Computation**\
Implemented the nearest-neighbor thermodynamic model (SantaLucia and Hicks, 2004) for ΔG stacking computation in the stem region, with loop free energy estimated at 6.3 kcal/mol and a 0.5 kcal/mol penalty for terminal A-T base pairs.
3. **Genetic Algorithm Optimization for Loop (17 flexible nucleotides)**\
Evolved 200 candidates over 400 generations with a 30% mutation rate. The fitness function penalized GC content violations, homopolymer presence, palindromic subsequences, and terminal adenine nucleotides.
4. **Genetic Algorithm Optimization for Stem (8 flexible nucleotides)**\
Minimized the deviation of computed ΔG from the target of -15 kcal/mol with penalties for GC content violations and homopolymers. The best candidate was reverse-complemented to form the full 36-nucleotide stem.
5. **Full Probe Assembly and Ranking**\
Assembled 66-nucleotide full probes as stem forward + loop + stem reverse, scored using Total Score = loop fitness minus total ΔG, and the top 5 candidates were selected as final deliverables.
6. **Secondary Structure Validation**\
Verified all 5 candidates using OligoAnalyzer to confirm the intended stem-loop (dumbbell) structure as the final computational quality check before sequence submission.

## Optimization Framework
Implemented a two-phase **Genetic Algorithm optimization pipeline** in Python with the following configuration:
| Parameter | Value |
| --------- | ----- |
| Population Size | 200 candidates |
| Number of Generations | 400 cycles |
| Mutation Rate | 30% |
| Elitism | Top 10 candidates retained per generation |
| Crossover Method | One-point crossover from top 25 candidates 
| Solution Space (Loop) | {A, T, G, C}^17 |
| Solution Space (Stem) | {A, T, G, C}^8 |
| Free Energy Target (Stem) | Approximately -15 kcal/mol (range -20 to -10 kcal/mol) |
| GC Content Constraint | 40% to 60% |
| Thermodynamic Reference | SantaLucia and Hicks (2004), nearest-neighbor parameters |

![image alt](https://github.com/nauradhiy/Genetic_Algorithm/blob/c17b59a6968ae2f7bb5babe319b522c07e2d7416/Genetic%20Algorithm%20Flowchart.png)

## Data Architecture 
Sourced thermodynamic reference data from two peer-reviewed tables in SantaLucia and Hicks (2004): loop free energy contributions by loop size for hairpin, bulge, and internal loops under 1 M NaCl conditions, and nearest-neighbor stacking parameters (ΔH°, ΔS°, ΔG°37) for all 10 Watson-Crick DNA base pair propagation sequences. These datasets were encoded directly into the Python optimization engine to power real-time fitness evaluation across all candidate sequences during evolutionary cycles.

## Results
All 5 final DNA probe candidates successfully met every design criterion defined by the team from the Department of Microbiology at ITB:
**Probe Stem (Best Candidate) 5'-GTGCAGGTAGTTCTTTAGCTAAAGAACTACCTGCAC-3'** with **ΔG = -20.59 kcal/mol** and GC content within the 40% to 60% range, free of homopolymers.

**Top 5 Full Probe Candidates Delivered to the iGEM Team**
| No. | Full DNA Sequence (66 nt) | ΔG (kcal/mol) | GC Content |
| ---- | ------------------------ | ------------- | ---------- |
| 1 | 5'-GTGCAGGTAGTTCTTTAGTATAAGCACTTTATTCACTACGTAGGCCCTCTAAAGAACTACCTGCAC-3' | -14.29 | 42.42% |
| 2 | 5'-GTGCAGGTAGTTCTTTAGTATAAGCACTTTATTCAATACGGGAGGACGCTAAAGAACTACCTGCAC-3' | -14.29 | 42.42% |
| 3 | 5'-GTGCAGGTAGTTCTTTAGTATAAGCACTTTATGCGTCCTGTGGGCCGCCTAAAGAACTACCTGCAC-3' | -14.29 | 48.48% |
| 4 | 5'-GTGCAGGTAGTTCTTTAGTATAAGCACTTTATCTGTCCTTGTTGCCAGCTAAAGAACTACCTGCAC-3' | -14.29 | 42.42% |
| 5 | 5'-GTGCAGGTAGTTCTTTAGTATAAGCACTTTATCTGTCCTTGCCGGGATCTAAAGAACTACCTGCAC-3' | -14.29 | 43.94% |

All candidates achieved **ΔG of -14.29 kcal/mol** within the target range of -10 to -20 kcal/mol, GC content between **42.42% and 48.48%**, no homopolymers, and confirmed **stem-loop structure formation** via OligoAnalyzer. Probes 1, 2, 4, and 5 produced the **cleanest structures** with no secondary pairings in the loop region.

## Research Impact
By delivering **5 computationally validated DNA probe candidates** that fully satisfy all biological and thermodynamic parameters defined by the team from the Department of Microbiology at ITB, this research **directly eliminates the risk and cost of sequence-related experimental failure** in Singapore laboratory procedures. The genetic algorithm screened thousands of candidate sequences across 400 generations to arrive at sequences that are not just theoretically sound but confirmed structurally clean through OligoAnalyzer simulation. This means the ITB iGEM team can proceed to physical synthesis and wet laboratory testing with high confidence that the sequences will perform as intended, protecting the team's research budget and timeline.
   
## Recommendations
1. **Submit Probes 1, 2, 4, and 5 as Priority Candidates for Singapore Synthesis**\
OligoAnalyzer simulation confirmed these four candidates form clean stem-loop structures without secondary pairings in the loop region. These should be the primary sequences submitted for physical synthesis to maximize the probability of experimental success on the first attempt.
2. **Use Probe 3 as a Backup Candidate with Caution**\
Probe 3 shows a minor 2-nucleotide pairing in the loop region. It remains within all thermodynamic and GC content parameters but carries a slightly higher risk of structural interference during hybridization. It is recommended as a secondary option only if the priority probes encounter unforeseen issues.
3. **Extend the Computational Framework for Future iGEM Research Cycles**\
The two-phase genetic algorithm pipeline built in Python is fully reusable. For future ITB iGEM research targeting different miRNA biomarkers or other nucleic acid targets, the same framework can be redeployed by updating the fixed nucleotide constraints, enabling the team to consistently reduce experimental failure risk through computational pre-validation.
4. **Conduct Specificity Testing Against Structurally Similar miRNAs**\
Before committing to the Singapore laboratory procedure, it is advisable to verify computationally that the designed probes do not cross-hybridize with miRNA sequences structurally similar to miRNA-20a, adding a final layer of confidence to the sequence selection.
5. **Document and Archive the Optimization Pipeline for ITB iGEM Knowledge Transfer**\
The methodology and Python codebase developed in this research represent reusable institutional knowledge for the ITB iGEM team. Formal documentation ensures future team members can leverage and build upon this computational approach without starting from scratch.

> Thermodynamic Reference: SantaLucia, J., Jr., and Hicks, D. (2004). The Thermodynamics of DNA Structural Motifs. Annual Review of Biophysics and Biomolecular Structure, 33, 415-440.
*This research was developed as a computational contribution to the ITB iGEM competition team. All sequences are intended for research purposes only.*
