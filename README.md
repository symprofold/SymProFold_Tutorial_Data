# SymProFold Tutorial


> [!IMPORTANT]
>  **AlphaFold-Multimer not necessarily required for this tutorial**
>  
> During this tutorial, ~100 models will be predicted with AlphaFold-Multimer. To calculate these models, you can use your AlphaFold-Multimer installation, but we also provide precalculated models.  
> To work through the tutorial, you do not require access to a AlphaFold-Multimer installation if you use the precalculated models as input.

&nbsp;

## 0. Loading of Tutorial Data


> [!IMPORTANT]
> This tutorial explains every step, furthermore we provide a folder with all intermediate results. You can check or copy the intermediate results stored in this folder.
>
> If you face issues during the tutorial, feel free to contact us. You find our email [here]( https://github.com/symprofold).  

Load the **folder with intermediate results**:
Continue with either **A)** or **B)**, depending on whether you have a Git installation on your system.

##### A) using existing Git on your system
```bash
cd /path_to_project/SymProFold/preassemblies/
git clone https://github.com/symprofold/SymProFold_Tutorial_Data.git Vaer_tutorial_data/
```
_(Vaer_ for the tutorial example _Vibrio aerogenes)_

##### B) without Git on your system
*   Download the SymProFold_Tutorial_Data repository as ZIP file (button 'Code', 'Download ZIP')
*   Move the downloaded ZIP file to `/path_to_project/SymProFold/preassemblies/`
*   Extract the ZIP file in the folder `/path_to_project/SymProFold/preassemblies/`
*   Rename the folder `SymProFold_Tutorial_Data-main` to `Vaer_tutorial_data`  
    _(Vaer_ for the tutorial example _Vibrio aerogenes)_

&nbsp;

## 1. Specifying Input Sequence, Pre-filtering

### 1.1 Creation of Folder

The folder `preassemblies/template/` is the template folder for a new SymProFold run.  
Copy this folder and name it `Vaer/` (for the tutorial example *Vibrio aerogenes*). The new folder now should have the path `preassemblies/Vaer/`.  
Change (`cd`) into the `Vaer/` folder for the next steps.
```bash
cd /path_to_project/SymProFold/preassemblies/
cp -R template/ Vaer/
cd Vaer/
```

### 1.2 Specifying the Input Sequence

> [!IMPORTANT]
> **SymProFold accepts as input either a**
> *   **FASTA file** or a
> *   **Uniprot<sup>[4]</sup> gene accession code**

*   Option **FASTA file**:  
    place a FASTA file with file extension *.fa into the `Vaer/` folder, e.g. `A0A1M5ZCF8.fa`
*   Option **Uniprot gene accession code**:  
    place an empty file with the Uniprot<sup>[4]</sup> gene accession code as filename into the `Vaer/` folder, e.g. `A0A1M5ZCF8` (without a filetype postfix)

We will now run the SymProFold pipeline for the gene A0A1M5ZCF8 of _Vibrio aerogenes_.  
To create an empty file with the Uniprot gene accession code, you can rename the placeholder file `rename_file_to_accession_code` to `A0A1M5ZCF8`.
```bash
mv rename_file_to_accession_code A0A1M5ZCF8
```

### 1.3 Creation of data structure

Run `1_create_datastructure.py` to create the data structure for the following workflow steps.  
```bash
python3 1_create_datastructure.py
```
(typical runtime: < 1s)

The folder `preassemblies/Vaer/` now should contain the following additional subfolders and data files:
```
                                [explanation of folder/file]
preassemblies/Vaer/             
├── A0A1M5ZCF8_x1/              [folder for unrelaxed full-length monomer predictions]
│   ├── A0A1M5ZCF8_x1/          [folder for relaxed full-length monomer predictions]
│   │   └── copy_relaxed_here   [info file indicating to copy the relaxed predictions in this subfolder]
│   └── copy_unrelaxed_here     [info file indicating to copy the unrelaxed predictions in this subfolder]
├── A0A1M5ZCF8_x2/              [folder for unrelaxed full-length homodimer predictions]
│   ├── A0A1M5ZCF8_x2/          [folder for relaxed full-length homodimer predictions]
│   │   └── copy_relaxed_here
│   └── copy_unrelaxed_here
├── A0A1M5ZCF8_x3/              [folder for unrelaxed full-length homotrimer predictions]
│   ├── A0A1M5ZCF8_x3/          [folder for relaxed full-length homotrimer predictions]
│   │   └── copy_relaxed_here
│   └── copy_unrelaxed_here
├── A0A1M5ZCF8_x4/              [folder for unrelaxed full-length homotetramer predictions]
│   ├── A0A1M5ZCF8_x4/          [folder for relaxed full-length homotetramer predictions]
│   │   └── copy_relaxed_here
│   └── copy_unrelaxed_here
├── A0A1M5ZCF8_x6/              [folder for unrelaxed full-length homohexamer predictions]
│   ├── A0A1M5ZCF8_x6/          [folder for relaxed full-length homohexamer predictions]
│   │   └── copy_relaxed_here
│   └── copy_unrelaxed_here
├── ...
├── A0A1M5ZCF8_x2.fa            [full-length homodimer FASTA file for prefiltering prediction]
├── A0A1M5ZCF8_x3.fa            [    ...     homotrimer                 ...                  ]
├── A0A1M5ZCF8_x4.fa            [    ...     homotetramer               ...                  ]
├── A0A1M5ZCF8_x6.fa            [    ...     homohexamer                ...                  ]
├── A0A1M5ZCF8.fa               [full-length monomer FASTA file]
└── ...

```
The FASTA files located directly in `preassemblies/Vaer/` are ready-to-use input files for AlphaFold-Multimer. This makes it easy to copy them into your AlphaFold-Multimer input folder.

Among the generated files, you will find e.g. a full-length homodimer FASTA file `A0A1M5ZCF8_x2.fa` for the pre-filtering prediction.

### 1.4 Prediction of full-length homodimer models with AlphaFold-Multimer

*   With access to a AlphaFold-Multimer installation:  
    Predict 5 models with the full-length homodimer FASTA file `A0A1M5ZCF8_x2.fa` as input using an AlphaFold-Multimer<sup>[3]</sup> installation.

*   Without AlphaFold-Multimer installation:  
    Use our precalculated models located in `Vaer_tutorial_data/section1.5_begin/A0A1M5ZCF8_x2/`.

### 1.5 Extraction of ipTM+pTM score

The script `2_rank_predictions.py` of SymProFold extracts the ipTM+pTM scores of those predicted models that are located in the same folder as the script itself.  
In AlphaFold-Multimer<sup>[3]</sup> predictions, the ipTM+pTM score is stored in the file `ranking_debug.json` (if available) and in PKL files. The SymProFold script `2_rank_predictions.py` reads the score either from `ranking_debug.json` or from PKL files, depending on what is available.

The `2_rank_predictions.py` script can be executed either directly in the results folder of the AlphaFold-Multimer installation or - as in this tutorial - in the corresponding subfolder of the preassemblies folder.

Now copy the predictions into the folder `preassemblies/Vaer/A0A1M5ZCF8_x2/` according to this scheme:
```
                                [explanation of folder/file]
preassemblies/Vaer/
…
├── A0A1M5ZCF8_x2/              [folder for unrelaxed full-length homodimer predictions]
│   ├── A0A1M5ZCF8_x2/          [folder for relaxed full-length homodimer predictions]
│   │   └── copy_relaxed_here   [info file indicating to copy the relaxed predictions in this subfolder]
│   └── copy_unrelaxed_here     [info file indicating to copy the unrelaxed predictions in this subfolder]
…
```

* Copy the unrelaxed models (ideally 5) in the folder where the info file `copy_unrelaxed_here` is located. Also copy the file `ranking_debug.json` in this folder.
* Copy the relaxed models (if available, ideally 5) in the folder where the info file `copy_relaxed_here` is located. Also copy the file `ranking_debug.json` in this folder.

* Copy the script `2_rank_predictions.py` to the folders that include the predicted models (PDB files and `ranking_debug.json`). The script duplicates the models and writes the ipTM+pTM score in the filename.

> [!NOTE]
> The script `2_rank_predictions.py` requires the following Python modules to be available on the system/environment on which it is executed:  
> glob, json, matplotlib, numpy, os, pathlib, pickle, shutil

* Run the script `2_rank_predictions.py`
  ```bash
  python3 2_rank_predictions.py
  ```
  (typical runtime: < 10s)
  
The folder now should contain additional files with this file naming scheme including the ipTM+pTM score:
```
A0A1M5ZCF8_x2/
├── unrelaxed_rank00_0.497.pdb
├── unrelaxed_rank01_0.359.pdb
├── unrelaxed_rank02_0.306.pdb
├── unrelaxed_rank03_0.291.pdb
└── unrelaxed_rank04_0.246.pdb
```

### 1.6 Pre-filtering

In the case of A0A1M5ZCF8, the pre-filtering is passed, because the full-length homodimer has an ipTM+pTM score >= 0.3 (filename of the top-ranking model is `unrelaxed_rank00_0.497.pdb`, ipTM+pTM score is 0.497).  
In this case, we continue with section 2 "Subchain Identification". Proceeding is not recommended for full-length homodimer ipTM+pTM scores < 0.3.

&nbsp;

## 2. Subchain Identification

### 2.1 Prediction of full-length monomer models with AlphaFold-Multimer

* Predict 1 model (or 5, if this is your standard configuration) with the full-length monomer FASTA file `A0A1M5ZCF8.fa` as input using an AlphaFold-Multimer installation.
* Analog to section 1.5, copy the predictions into the folder `preassemblies/Vaer/A0A1M5ZCF8_x1/A0A1M5ZCF8_x1/` or alternatively for unrelaxed models `preassemblies/Vaer/A0A1M5ZCF8_x1/`.
* Analog to section 1.5, copy the script `2_rank_predictions.py` to the folder that includes the predicted model(s) (PDB files and `ranking_debug.json`) and run this script.

### 2.2 Domain Identification with Domain_Separator

*   Start ChimeraX
*   Execute this command in ChimeraX:  
    ```bash
    run "/path_to_project/SymProFold/preassemblies/Vaer/3_run_domain_separator.py"
    ```
    (typical runtime: ~ 10s)

This command automatically runs Domain_Separator on the top-ranked model in `preassemblies/Vaer/A0A1M5ZCF8_x1/A0A1M5ZCF8_x1/` or alternatively for unrelaxed models `preassemblies/Vaer/A0A1M5ZCF8_x1/` and writes a FASTA file named `A0A1M5ZCF8_d.fa` into the folder `preassemblies/Vaer/`. This FASTA file contains the identified domain sequences separated by line breaks.

> [!IMPORTANT]
> The domain boundaries in the generated file `A0A1M5ZCF8_d.fa` may differ by ~1 residue from the corresponding file provided in the tutorial data (`/Vaer_tutorial_data/section3_begin/A0A1M5ZCF8_d.fa`).  
> If you plan to use the AlphaFold predictions provided by the tutorial data in the next sections, replace now (!) the `A0A1M5ZCF8_d.fa` by the corresponding file from the tutorial data.  
> This is important to ensure an exact correspondence between the domain boundaries in `A0A1M5ZCF8_d.fa` and the subchains in the predictions.

> [!NOTE]
> If a file other than the top-ranked model should be used for domain separation, the 'Domain_Separator' installation in `/path_to_project/SymProFold/tools/domain_separator/` with the input folder `/path_to_project/SymProFold/tools/domain_separator/input/` can be used. A tutorial to run Domain_Separator independetly is available here: https://github.com/symprofold/Domain_Separator .

### 2.3 Generation of Subchains

*   Run `4_generate_subchains.py` to create the standard set of subchains.
    ```bash
    python3 4_generate_subchains.py
    ```
    (typical runtime: < 1s)

This script generates FASTA files for each subchain.
Again, the FASTA files are located directly in `preassemblies/Vaer/` and are ready-to-use input files for AlphaFold-Multimer. This makes it easy to copy them into your AlphaFold-Multimer input folder.

Furthermore the folder `preassemblies/Vaer/` now should contain a subfolder with the following subchain FASTA files:
You only need these files if you carry out the MSA step of AlphaFold-Multimer separately. Then you can use these files for MSA generation.
```
                                [explanation of folder/file]
msa_gen/
├── A0A1M5ZCF8_1x2.fa           [FASTA file of subchain containing domain 1 as homodimer]
├── A0A1M5ZCF8_3x2.fa           [FASTA file of subchain containing domain 3 as homodimer]
├── A0A1M5ZCF8_12x2.fa          [FASTA file of subchain containing domains 1 and 2 as homodimer]
├── A0A1M5ZCF8_23x2.fa          [FASTA file of subchain containing domains 2 and 3 as homodimer]
└── A0A1M5ZCF8_x2.fa            [FASTA file of full-length homodimer]
```

&nbsp;

## 3. Prediction of Multimer Sets

### 3.1 Prediction of Subchains as Multimers with AlphaFold-Multimer

*   With access to a AlphaFold-Multimer installation:  
    Predict 5 models for each combination of subchain and oligomeric state (dimer, trimer, tetramer, hexamer) using an AlphaFold-Multimer<sup>[3]</sup> installation. In total, these are  (5x4)-1=19 runs.
    Use the FASTA files located directly in `preassemblies/Vaer/` as input.

*   Without AlphaFold-Multimer installation:  
    Use our precalculated models located in the subfolders of `Vaer_tutorial_data/section3.2_begin/`.

* Analog to section 1.5 and 2.1, copy the predictions into the folders `preassemblies/Vaer/A0A1M5ZCF8_[...]x[...]/`.


### 3.2 Extraction of ipTM+pTM scores

* Analog to section 1.5 and 2.1, copy the script `2_rank_predictions.py` to all folders that include the predicted model(s) (PDB files and `ranking_debug.json`) and run this script.

&nbsp;

## 4. Scoring of Symmetry Axis Complexes

### 4.1 Analyzation and Model Processing Tasks

*   Restart ChimeraX (do not reuse the previous instance/window to avoid caching side effects)
*   Execute this command in ChimeraX:  
    ```bash
    run "/path_to_project/SymProFold/preassemblies/Vaer/5_analyze_predictions.py"
    ```
    (typical runtime: ~ 10 min)

The script `5_analyze_predictions.py` performs several analyzation and model processing tasks on all predictions.  
These tasks include e.g.
* generation of interface matrices
* orientation of models in xy place

### 4.2 Generation of Symmetry Plot with Scoring

*   Run `6_symplot.py` to generate the symmetry plot.  
    ```bash
    python3 6_symplot.py
    ```
    (typical runtime: ~ 10s)

The folder `preassemblies/Vaer/` now should contain the symmetry plot (`A0A1M5ZCF8_subchain_set1_0.png`) and other PNG files with symmetry plots, e.g. highlighting the main binding sites.

&nbsp;

## 5. Assembly of Unit Cell

### 5.1 Superposition, Optimization

#### 5.1.1 Combinatorial Search through Symmetry Complexes

In this step, symmetry complexes from the _rotational symmetry axis clusters_ identified in section 4.2 (see symmetry plot) are tested for layer assembly and scored by a quality score.  
The script `template_combinatorial_search.py` is the template script for this combinatorial search.

*   Change (`cd`) into the assemblies folder.
    ```bash
    cd /path_to_project/SymProFold/assemblies/
    ```

*   Copy this script and name it `Vaer_combinatorial_search.py`.
    ```bash
    mv template_combinatorial_search.py Vaer_combinatorial_search.py
    ```

In this script, the prediction scenarios to be included in the search and score run are defined:
```bash
symplex0_predscenarios = ['23x2', '23x4', '23x6', '3x2', '3x4']
symplex1_predscenarios = ['12x4', '12x6', '1x3', '1x4', '1x6', 'FLx4']
```
Further configuration parameters: species abbreviation, species name, gene name

*   Run `Vaer_combinatorial_search.py` to test combinations of symmetry complexes to form a assembly.  
    ```bash
    python3 Vaer_combinatorial_search.py
    ```
The runtime is ~ 10h if only 1 instance of the script is running.  
You can start several instances and run them parallel. If 20 instances are started, the runtime is reduced to ~ 30min (if enough cpu cores and memory available).

#### 5.1.2 Results of Combinatorial Search

The results of the combinatorial search are collected in the folder `/path_to_project/SymProFold/assemblies/Vaer_Vibrio aerogenes_A0A1M5ZCF8_[version]/`.  
Each subfolder represents one symmetry complex combination, the quality score can be read from the subfolder name.
```
assemblies/Vaer_Vibrio aerogenes_A0A1M5ZCF8_[version]/             
├── ...
├── 44_23-12_0.229_0.19_0.039_d2_0_24/
└── ...

naming scheme:
[  4   ][  4   ]_[   23    ]-[   12    ]_[  0.229   ]_[  0.19  ]_[  0.039   ]_d[    2    ]_[...]_[  2   ][  4   ]
[order0][order1]_[subchain0]-[subchain1]_[score_qual]_[score_cl]_[score_bend]_d[align_dom]_[...]_[model0][model1]

[order0]:       order of rot. symmmetry axis 0 (symmetry complex 0)
[order1]:       order of rot. symmmetry axis 1 (symmetry complex 1)
[subchain0]:    subchain of symmetry complex 0
[subchain1]:    subchain of symmetry complex 1
[score_qual]:   score_quality = score_clash+score_bend
[score_cl]:     score_clash
[score_bend]:   score_bend
[align_dom]:    alignment domain
[model0]:       model number (starting with 0) for symmetry complex 0
[model1]:       model number (starting with 1) for symmetry complex 1
```
*   Identify the symmetry complex combination with the best (smallest) quality score (score_quality). In this tutorial, the score is minimal for `44_23-12_0.229_0.19_0.039_d2_0_24/`.

Example structure of a assembly subfolder (that did not stop early):
```
                                            [explanation of folder/file]
44_23-12_0.229_0.19_0.039_d2_0_24/
├── assembly/                               [folder with 3x3 assembly model]
├── snapshot1_ax_predictions/               [folder with symmetry complexes used for superposition]
├── spacegroup_predicted_p4.txt             [file with filename showing the symmetry group]
└── Vaer_A0A1M5ZCF8_biochem_properties.txt  [TXT file with biochemical properties of the assembly, e.g. lattice constant]
```
The lattice constant can be found in the file `Vaer_A0A1M5ZCF8_biochem_properties.txt`: `a = 12.7nm`.  
A lookup in folder `snapshot1_ax_predictions/` shows an ipTM+pTM score of 0.674 for symmetry complex 0 (model 2) and an score of 0.828 for symmetry complex 1 (model 4).

#### 5.1.3 Consideration of quality score and ipTM+pTM

When we look at the models within the subchain combination "23x4 + 12x4", we find that there are also models with higher ipTM+pTM score that result in the same assembly with the same lattice constant (`a = 12.7nm`):  
symmetry complex 0: subchain 23, model 1 (instead model 2): 0.713 [ipTM+pTM]  
symmetry complex 1: subchain 12, model 0 (instead model 4): 0.873 [ipTM+pTM]

(For subchain 23, model 0 would have a higher ipTM+pTM score (than model 1), but a different domain conformation, resulting in very high clash score/not the same assembly.)

### 5.2 Setup Assembly Script

*   Change (`cd`) into the assemblies folder.  
    ```bash
    cd /path_to_project/SymProFold/assemblies/
    ```
    This folder contains the assembly runfiles (`*_run.py`) for the S-Layer assemblies published in [1]. Each assembly runfile contains the input configuration for an assembly.  
    The configuration parameters include
    * species name, species abbreviation, gene name  
    * the symmetry complexes (symmetry complex 0 and symmetry complex 1)  
    * relaxation status of the symmetry complex models (relaxed, unrelaxed)  
    * alignment domain

*   Adapt an existing assembly runfile or use the existing runfile `Vaer_run.py` for this tutorial. The symmetry complex with the higher order should be set as symmetry complex 0.   

An example configuration for this tutorial can be found in `Vaer_run.py`, using the parameter from section 5.1.2. In this case, both rotational symmetry axis have an order of 4, so here we can e.g. set the subchain 12 as symmetry complex 0 and the subchain 23 as symmetry complex 1.
```bash
conf.set_species('Vaer', 'Vibrio aerogenes')
        # 'Vaer': storage abbreviation
        # 'Vibrio aerogenes': species name

conf.set_gene('A0A1M5ZCF8')
        # 'A0A1M5ZCF8': gene name

symplex0_folder = 'A0A1M5ZCF8_12x4/'
        # 'A0A1M5ZCF8_12x4/': symmetry complex 0 (count starts at 0)

model_status0 = 1
        # relaxation status of model 0:
        # 0: unrelaxed model, 1: relaxed model

alignment_domain = 2
        # alignment domain used for superposition
```

### 5.3 Calculation of Assembly Models

*   Start ChimeraX
*   Execute this command in ChimeraX to assemble the symmetry complexes to the resulting unit cell.  
    ```bash
    run "/path_to_project/SymProFold/assemblies/Vaer_run.py"
    ```
    (typical runtime: ~ 10min)

> [!NOTE]
> You also find the assembled models of this tutorial in the repository https://github.com/symprofold/Structures under `Vaer_Vibrio aerogenes`.

&nbsp;

&nbsp;

## References to the Software mentioned and used

[1] C. Buhlheller*, T. Sagmeister*, C. Grininger, N. Gubensäk, U. Sleytr, I. Usón, T. Pavkov-Keller, SymProFold - Structural prediction of symmetrical biological assemblies, 03 January 2024, Preprint, https://doi.org/10.21203/rs.3.rs-3830312/v1  
[2] Pettersen, E. F. et al. UCSF ChimeraX: Structure visualization for researchers, educators, and developers. Protein Science 30, 70–82 (2021).  
[3] Evans, R. et al. Protein complex prediction with AlphaFold-Multimer. bioRxiv 2021.10.04.463034 (2022).  
[4] The UniProt Consortium, UniProt: the Universal Protein Knowledgebase in 2023, Nucleic Acids Research, Volume 51, Issue D1, 6 January 2023, Pages D523–D531, https://doi.org/10.1093/nar/gkac1052
