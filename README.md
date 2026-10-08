# Polymer force-field and mixture-composition research notebooks

Mathematica research notebooks for molecular-interaction plots, torsional-function comparisons, and converting water/alcohol molecule counts to mass fractions. These support preparation and interpretation of molecular-dynamics studies rather than running trajectories themselves.

## Functions and engineering applications

| Source | Observed function | Inferred application |
|---|---|---|
| `MassPerVsNumMolecules.nb` | Defines `MassPerEt[alpha]`, `MassPerMe[alpha]`, and `MassPerP[alpha]` using alcohol count and water count `beta` | Choosing discrete mixture compositions for MD inputs [1,2] |
| `MassPerVsNumMoleculesExtended*.nb` | Extended composition/presentation variants | Comparing sampling choices and preparing composition plots |
| `GuyFMongelli_testingDihedralFunctions.nb` | Plots cosine-based torsion expressions | Checking periodicity, conventions, and energy-profile shapes [3] |
| `GuyFMongelli_DoctoralProposalPlots.nb`, `GuyFMongelli_DoctoralProposalPlots_ForceFieldSpecific.nb` | Bond, angle, dihedral, and Lennard-Jones potential plots | Examining force-field parameter sensitivity for polymer/cosolvent modeling [1,3] |

## Example function calls

In Mathematica, with the current directory set to this repository:

```wolfram
nb = NotebookOpen[FileNameJoin[{Directory[], "MassPerVsNumMolecules.nb"}]];
```

In a fresh kernel, evaluate the cells defining molecular weights and `MassPerEt`, `MassPerMe`, `MassPerP`, keeping `beta` symbolic during their definitions. Then:

```wolfram
N[MassPerEt[100] /. \[Beta] -> 1000]
N[100 MassPerEt[100] /. \[Beta] -> 1000]
N[{MassPerEt[100], MassPerMe[100], MassPerP[100]} /. \[Beta] -> 1000]
```

The first call returns an ethanol **mass fraction**, the second converts it to mass percent, and the third compares the three alcohol choices at equal molecule counts. For the notebook’s rounded ethanol/water molecular weights (46 and 18), the first value is approximately **0.20354**, or **20.354%**. This expected value was checked by arithmetic; the notebook was not executed here. Count ratios cancel Avogadro’s constant. Fixed total molecule count requires making water count decrease as alcohol count increases; a fixed `beta` describes a different sweep.

## Reproducibility and limits

Input cells include explanatory text as well as executable code; evaluate selected definitions instead of blindly evaluating the entire notebook. Use consistent energy, length, and angular units when comparing force fields. A plotted torsional potential does not establish that its coefficients reproduce a particular polymer or liquid. Validate minima/periodicity, mixing rules, and parameter provenance before producing simulation inputs. The related `CWRU_Dissertation_README.md` offers additional historical guidance, but actual symbol definitions and this source-based summary govern the examples. Archive names do not certify a completed dissertation, degree, or reproduced publication.

## Review scope and software citation

Documentation reviewed on 2026-10-08 against source commit [`66e8d6badd23`](https://github.com/gmongell/Mathematica_PhD_Dissertation_CWRU_EChE/tree/66e8d6badd239c1b5a8b1d3aa6bc724d9bed52b2). “Observed” means supported by source inspection; engineering applications are reasoned possibilities unless explicitly demonstrated. Scholarly references provide methodological context and do not certify these implementations. Runtime validation is stated separately above.

For software attribution, cite Guy Francis Mongelli, *Mathematica_PhD_Dissertation_CWRU_EChE*, the [repository](https://github.com/gmongell/Mathematica_PhD_Dissertation_CWRU_EChE), the exact commit used, and your access date. Also cite the relevant method publications and any original third-party contributors. No unverified software DOI or release version is assigned by this documentation.

## Scholarly references

1. “The surface activity of polymers in cosolvated systems determined from computational simulation.” *Polymer* (2016). [DOI: 10.1016/j.polymer.2015.11.003](https://doi.org/10.1016/j.polymer.2015.11.003). Related polymer/cosolvent surface-activity research. The publisher indexes the journal publication as 2016; the DOI contains 2015. Exact reproduction requires the original inputs and analysis conventions.

2. L. Martínez, R. Andrade, E. G. Birgin, and J. M. Martínez (2009). “PACKMOL: A package for building initial configurations for molecular dynamics simulations.” *Journal of Computational Chemistry* 30, 2157–2164. [DOI: 10.1002/jcc.21224](https://doi.org/10.1002/jcc.21224). Supports molecular packing and initialization workflows.

3. W. L. Jorgensen, D. S. Maxwell, and J. Tirado-Rives (1996). “Development and Testing of the OPLS All-Atom Force Field on Conformational Energetics and Properties of Organic Liquids.” *JACS* 118, 11225–11236. [DOI: 10.1021/ja9621760](https://doi.org/10.1021/ja9621760). Methodological context for organic-liquid force fields and torsional/nonbonded parameters.

## Ownership and existing license notices

Copyright (c) 2025 Guy Francis Mongelli

The existing project notice declares Apache License 2.0 for project code. Documentation, prose, and figures are declared CC BY 4.0; notebook code cells are Apache-2.0 and narrative/figures CC BY 4.0. Preserve all file-level and third-party notices. This README update does not change ownership or licensing terms.
