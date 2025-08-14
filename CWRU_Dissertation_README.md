# Mathematica Notebooks for Force-Field & Mixture Analysis

This repository hosts a set of Wolfram *Mathematica* notebooks used to generate doctoral proposal figures and to analyze molecular systems with a focus on force-field–specific dihedral functions and mixture composition scaling. The notebooks are intended to be reproducible, readable, and adaptable to new systems.

> **Scope.** The workflows cover (i) figure generation for a doctoral proposal, (ii) validation of dihedral energy forms used in classical force fields, and (iii) relationships between total mass, molecular counts, and composition for mixtures—along with presentation-ready plot variants.

---

## Repository Contents

- `GuyFMongelli_DoctoralProposalPlots.nb`  
  End-to-end figure generation for the doctoral proposal (core plots, helper utilities, export cells).

- `GuyFMongelli_DoctoralProposalPlots_ForceFieldSpecific.nb`  
  Proposal plots parameterized for specific force fields (e.g., dihedral phase/periodicity variants and parameter sweeps).

- `GuyFMongelli_testingDihedralFunctions.nb`  
  Validation and exploration of dihedral energy terms of the form  
  \(V(\varphi) = \sum_n k_n \big(1 + \cos(n\varphi - \delta_n)\big)\), including periodicity, phase conventions, and parameter sensitivity checks.

- `MassPerVsNumMolecules.nb`  
  Baseline analysis connecting total mass and number of molecules for single- and multi-component systems.

- `MassPerVsNumMoleculesExtended.nb`  
  Extended analysis with additional cases and plotting utilities (e.g., per-component scaling and log–log diagnostics).

- `MassPerVsNumMoleculesExtended2.nb`  
  Further extended scenarios (e.g., alternate units, density assumptions, and uncertainty bands).

- `MassPerVsNumMoleculesExtended_ForPresentation.nb`  
  Presentation-oriented variant (streamlined styles, larger fonts, and export-ready figures).

> **Note.** If you reorganize the repository, consider moving `.nb` files into a `notebooks/` directory and adding `src/`, `data/`, and `output/` folders as described below.

---

## Quick Start

1. **Requirements**
   - Wolfram *Mathematica* **14.x** recommended (13.0+ likely compatible).  
   - Optional paclets used in some environments: `MaTeX`, `IGraphM`, or units/plotting helpers. If a notebook reports a missing paclet, install via:
     ```wolfram
     PacletInstall["MaTeX"]
     ```

2. **Clone**
   ```bash
   git clone <this-repo-url>.git
   cd <repo-name>
   ```

3. **Open a notebook**
   - Double-click any `.nb` or open from within *Mathematica*.
   - At the top of each notebook, evaluate the “Environment” cell first (if present). Many notebooks use:
     ```wolfram
     SetDirectory@NotebookDirectory[];
     $HistoryLength = 0;
     ```

4. **Evaluate all**
   - From the menu: **Evaluation → Evaluate Notebook**  
   - Or evaluate the individual sections in order (Setup → Data/Parameters → Computation → Plots/Export).

---

## Recommended Folder Structure (Optional)

If you plan to add data, scripts, or exported figures, the following structure keeps things organized:

```
.
├── notebooks/
│   ├── DoctoralProposalPlots.nb
│   ├── DoctoralProposalPlots_ForceFieldSpecific.nb
│   ├── testingDihedralFunctions.nb
│   ├── MassPerVsNumMolecules*.nb
├── data/
│   └── (inputs: CSV, JSON, MOL/SDF, parameter tables, etc.)
├── src/
│   └── (shared .wl/.m utility files imported by notebooks)
└── output/
    ├── figures/
    └── tables/
```

To create the output paths within a notebook:
```wolfram
SetDirectory@NotebookDirectory[];
outputDir  = FileNameJoin[{NotebookDirectory[], "output"}];
figDir     = FileNameJoin[{outputDir, "figures"}];
tableDir   = FileNameJoin[{outputDir, "tables"}];
Scan[If[!DirectoryQ[#], CreateDirectory[#, CreateIntermediateDirectories -> True]] &, {outputDir, figDir, tableDir}];
```

---

## Reproducing Figures (Example Pattern)

```wolfram
(* Ensure we are relative to the notebook location *)
SetDirectory@NotebookDirectory[];

(* Example parameters *)
params = <|
  "Units" -> "g/mol",
  "Components" -> {"A", "B"},
  "MolarMass" -> <|"A" -> 78.11, "B" -> 46.07|>,
  "Fractions" -> <|"A" -> 0.6, "B" -> 0.4|>
|>;

(* Example computation producing a plot object *)
plot = ListLinePlot[Table[{n, n^(2/3)}, {n, 10, 1000, 10}],
  Frame -> True, FrameLabel -> {"N molecules", "f(N)"},
  ImageSize -> 500
];

(* Export *)
figPath = FileNameJoin[{NotebookDirectory[], "output", "figures", "mass_vs_n.png"}];
If[!DirectoryQ@DirectoryName[figPath], CreateDirectory[DirectoryName[figPath], CreateIntermediateDirectories -> True]];
Export[figPath, plot, ImageResolution -> 300]
```

> Replace the toy computation with the plot variables produced in each notebook (e.g., `massPlot`, `compositionPlot`, `dihedralSweepPlot`, etc.).

---

## Headless / Batch Execution (Optional)

For automated builds or CI, consider exporting computational kernels from notebooks into `.wl` scripts (e.g., `src/build_figures.wl`) and running them via `wolframscript`:

```bash
wolframscript -file src/build_figures.wl
```

Inside `build_figures.wl`, call shared functions to load parameters, compute results, and export figures to `output/figures`. You can also parameterize runs using `$ScriptCommandLine` arguments.

---

## Data & Parameters

- **Data sources.** If external parameter tables or molecular structures are required, place them in `data/` and reference with `FileNameJoin` relative to `NotebookDirectory[]`.
- **Units and assumptions.** Document density, temperature, and unit conventions in the “Parameters” section of each notebook.
- **Randomness.** If any sampling is used, set a fixed seed (`SeedRandom[1234]`) for reproducibility.

---

## Force-Field Notes (Corrected)

This repository touches several common *proper* dihedral/torsion conventions. To avoid ambiguity and plotting mismatches, the definitions and conversions below are explicit and GitHub‑Markdown friendly.

### 1) Common functional forms

**AMBER/CHARMM (Fourier with phase)**  
$$
V(\phi)=\sum_{n} k_n\left[1+\cos\big(n\phi-\delta_n\big)\right]
$$
- Typical choices: $\delta_n\in\{0^\circ,180^\circ\}$; $n=1,2,3,4$.
- Parameter files usually tabulate angles in **degrees**. Mathematical libraries expect **radians**; convert as needed.

**OPLS-AA (truncated cosine series)**  
$$
\begin{aligned}
V(\phi)=&\tfrac{1}{2}V_1\big(1+\cos\phi\big)
+\tfrac{1}{2}V_2\big(1-\cos 2\phi\big)\\
&+\tfrac{1}{2}V_3\big(1+\cos 3\phi\big)
+\tfrac{1}{2}V_4\big(1-\cos 4\phi\big)\,.
\end{aligned}
$$

**Ryckaert–Bellemans (RB; GROMOS/GROMACS for alkanes)**  
$$
V(\psi)=\sum_{m=0}^{5} C_m \cos^m\!\psi,\qquad \psi=\phi-180^\circ
$$

**Improper torsions (out‑of‑plane)**  
Most force fields use a harmonic form
$$
V_{\text{imp}}(\xi)=k(\xi-\xi_0)^2
$$
with $\xi$ an out‑of‑plane angle (degrees in parameter files).

### 2) Safe conversions

**AMBER/CHARMM → OPLS** (when $\delta_n\in\{0^\circ,180^\circ\}$ and $n\in\{1,2,3,4\}$):
$$
V_n = 2\,k_n\cos\delta_n\,.
$$
Thus if $\delta_n=0^\circ$: $V_n=2k_n$; if $\delta_n=180^\circ$: $V_n=-2k_n$.  

**OPLS → AMBER/CHARMM** (choose phases to match signs):
- If $V_n\ge 0$: set $\delta_n=0^\circ$ and $k_n=V_n/2$.
- If $V_n<0$: set $\delta_n=180^\circ$ and $k_n=|V_n|/2$.

**Notes on RB ↔ OPLS/AMBER.**  
RB uses powers of $\cos(\phi-180^\circ)$; mapping is linear but involves a fixed matrix between $\{C_0,\dots,C_5\}$ and Fourier coefficients $\{\cos n\phi\}$. In practice, fit $C_m$ to a dense grid of $V(\phi)$ generated from your OPLS/AMBER parameters (see the `testingDihedralFunctions.nb` notebook).

### 3) Units, angles, and scaling

- **Energy units:** keep all $k_n$, $V_n$, and $C_m$ in a single system (e.g., kcal/mol or kJ/mol).  
- **Degrees vs radians:** parameter files usually specify degrees; *Mathematica* uses radians in `Cos`. Use `Cos[angle Degree]` to stay consistent.
- **1–4 scaling:** many force fields scale 1–4 electrostatics/LJ (e.g., 0.5/0.5 or 1.0/1.0). Torsion fits are often coupled to those choices; use consistent scaling when reproducing barriers.
- **Periodicities:** ensure the intended $n$ values are present (missing terms implicitly have zero amplitude).

### 4) Mathematica‑safe evaluation snippets

Inline utilities to eliminate unit mistakes and enable quick comparisons:

```wolfram
(* Degrees-safe cosine *)
cosd[θ_] := Cos[θ Degree];

(* AMBER/CHARMM-style torsion: params = {{n,k,δ}, ...} with degrees for δ *)
Vamber[phi_, params_List] := Total[params /. {n_, k_, δ_} :> k (1 + cosd[n phi - δ])];

(* OPLS torsion with {V1,V2,V3,V4} *)
Vopls[phi_, {V1_,V2_,V3_,V4_}] :=
  0.5 V1 (1 + cosd[phi]) + 0.5 V2 (1 - cosd[2 phi]) +
  0.5 V3 (1 + cosd[3 phi]) + 0.5 V4 (1 - cosd[4 phi]);

(* AMBER/CHARMM -> OPLS conversion when δ∈{0°,180°} *)
toOPLS[params_List] := Module[{byN, V = ConstantArray[0., 4]},
  byN = Association[Rule @@@ (params /. {n_, k_, δ_} :> {n, {k, δ}})];
  Do[ If[KeyExistsQ[byN, n], With[{k = byN[n][[1]], δ = byN[n][[2]]},
        V[[n]] = 2 k Cos[δ Degree]]], {n, 1, 4}];
  V
];
```

These snippets assume angles are supplied in **degrees** (`phi` in degrees). If your plotting variable is in radians, drop the `Degree` wrapper and supply radians consistently.


## Style & Export

- **Plot styling.** Presentation-ready notebooks use larger fonts, aspect ratios suitable for slides, and color-safe palettes.
- **Export formats.** Prefer vector formats (`.pdf`, `.eps`, `.svg`) for publication and high-resolution `.png` for slides. Typical usage:
  ```wolfram
  Export["output/figures/<name>.pdf", plot];
  Export["output/figures/<name>.png", plot, ImageResolution -> 300];
  ```

---

## Version Control Tips

- **Large files.** Consider enabling [Git LFS](https://git-lfs.com/) for `.nb` files:
  ```bash
  git lfs install
  echo "*.nb filter=lfs diff=lfs merge=lfs -text" >> .gitattributes
  ```
- **Determinism.** Keep outputs out of version control; commit only notebooks and source. Use `output/` in `.gitignore`.

Example `.gitignore` additions:
```
/output/
/data/raw/
*.DS_Store
```

---

## Contributing

Pull requests are welcome for:
- Converting shared utilities into `.wl` files under `src/`
- Adding tests for dihedral/angle/bond energy conventions
- Extending mixture models and unit tests

Please open an issue to discuss substantial changes.

---

## License

Add your chosen license (e.g., MIT) as `LICENSE`. If omitted, all rights are reserved by default.

---

## Citation

If you use these notebooks in academic work, please cite appropriately. You may add a `CITATION.cff` file like:

```yaml
cff-version: 1.2.0
title: "Mathematica Notebooks for Force-Field & Mixture Analysis"
authors:
  - family-names: Mongelli
    given-names: Guy Francis
version: 0.1.0
date-released: 2025-08-14
```

---

## Contact

For issues and feature requests, please open a GitHub issue in this repository.
