# Summer Student Project: Tagging Polarized Boson Eigenstates with the Particle Transformer

**Institution:** University of Bologna (Master's student)  
**Host:** CERN — EP-CMG or equivalent jet-physics group  
**Duration:** 4 weeks (one calendar month)  
**Prerequisites:** Basic Python, basic ML (classification concepts), introductory particle physics (jets, boosted objects)

---

## Physics motivation

Boosted, hadronically decaying W and Z bosons are produced in two distinct
polarization eigenstates: **longitudinal (V_L)** and **transverse (V_T)**.
The longitudinal component is the degree of freedom introduced by the Higgs
mechanism, so any anomalous production of V_L in scattering processes would
signal new physics beyond the Standard Model — it is the central observable
in Vector Boson Scattering (VBS) and in searches for heavy resonances
decaying to WW/WZ/ZZ pairs.

The two polarization states leave distinct imprints on the jet substructure:
V_L decays isotropically in the boson rest frame (sin²θ* distribution),
while V_T decays are forward-backward peaked (1 + cos²θ*), leading to
different energy sharing between the two subjets. The goal of this project
is to teach the **Particle Transformer (ParT)** — a state-of-the-art
Transformer-based jet tagger — to distinguish V_L from V_T jets, using a
fresh simulation sample produced with the same toolchain as JetClass-II.

**Primary deliverable (must-have):**
A validated W_L / W_T / QCD dataset (~500k jets per class), a fine-tuned
ParT tagger, and ROC curves comparing it to a classical substructure baseline.

**Stretch goals (only if ahead of schedule):**
Z_L / Z_T classes; pT-dependence study; mass-decorrelation check; BDT
baseline on a set of hand-crafted substructure variables.

---

## Key references and repositories

| Resource | Link |
|---|---|
| Gentle introduction to neural networks and transformers | https://www.3blue1brown.com/?topic=neural-networks |
| Broad and quick introduction to Machine Learning in HEP | https://inspirehep.net/files/bb7fe2e5330d81b1ef71e6605e3e5d40 |
| Particle Transformer (ParT) code + JetClass | https://github.com/jet-universe/particle_transformer |
| JetClass-II generation scripts | https://github.com/jet-universe/jetclass2_generation |
| ParT paper | https://arxiv.org/abs/2202.03772 |
| JetClass dataset (Zenodo) | https://zenodo.org/record/6619768 |
| JetClass-II dataset (HuggingFace) | https://huggingface.co/datasets/jet-universe/jetclass2 |
| weaver-core (training framework) | https://github.com/hqucms/weaver-core |
| sophon ntuple branch definitions | https://github.com/jet-universe/sophon |
| Jet substructure review | https://arxiv.org/abs/1709.04464 |
| W polarization via subjet energy proxy | https://link.springer.com/article/10.1007/JHEP05(2025)028 |
| ML tagging of longitudinal bosons (amplitude method) | https://arxiv.org/abs/2306.07726 |
| Polarized matrix elements in MG5_aMC | https://arxiv.org/abs/1912.01725 |

---

## Project phases

### Phase 0 — Before arrival / Days 1–2: Accounts and environment setup

**Goal:** Working login, disk space, and GPU access on day one.

**Tasks:**
- Request CERN account, Kerberos, lxplus SSH access, EOS quota
  (start these *before* arrival — HR takes 3–5 days)
- Learn to source an LCG view:
  `source /cvmfs/sft.cern.ch/lcg/views/LCG_104/x86_64-el9-gcc13-opt/setup.sh`
- Confirm GPU access with supervisor: lxplus-gpu nodes or HTCondor GPU queue
- Clone the two main repos, install `weaver-core`
- Set up a personal git repo for the project (all code tracked from day one)

**Feasibility:** ✅ Low risk — purely administrative, but account delays are
real so start early.

**Training resources:**
- lxplus documentation: https://lxplusdoc.web.cern.ch
- CERN Batch (HTCondor): https://batchdocs.web.cern.ch
- EOS/CERNBox: https://cernbox.docs.cern.ch
- HSF software training (general HEP computing): https://hepsoftwarefoundation.org/training/curriculum.html
- Git basics (if rusty): https://swcarpentry.github.io/git-novice/

---

### Phase 1 — Week 1: Understand the tools; reproduce a known result

**Goal:** By Friday of week 1, the student has run ParT fine-tuning
end-to-end on a *public* dataset and has read the core physics of
boson polarization.

**Tasks:**

1. **Physics reading (days 1–2).** Read the ParT paper
   (arXiv:2202.03772). Understand the JetClass dataset structure:
   10 jet classes, 100M jets, MadGraph+Pythia+Delphes simulation chain.
   Read the jet substructure review (arXiv:1709.04464) sections 1–3.
   Study the W polarization proxy paper (JHEP 05 2025) to understand
   why energy sharing between subjets encodes polarization.

2. **Hands-on with ParT (days 2–4).** Clone `particle_transformer`,
   download the small **TopLandscape** dataset (~200MB, not the 100M-jet
   JetClass), and run:
   ```bash
   ./train_TopLandscape.sh ParT-FineTune kin
   ```
   on a GPU node. Understand the weaver YAML config files
   (`data/TopLandscape/`). Plot the ROC curve from the saved model output.

3. **Understand the generation chain (day 5).** Read the
   `jetclass2_generation` README end-to-end. Understand each step:
   MadGraph process card (`mg5_step1.dat`) → Pythia8 shower (`py8.dat`) →
   Delphes detector simulation → `makeNtuples.C` ROOT macro → flat ntuple
   readable by weaver.

**Feasibility:** ✅ Low risk. The repo is turnkey; the main risk is
GPU-node queue times, which is why the dataset used here is deliberately small.

**Training resources:**
- ParT paper: https://arxiv.org/abs/2202.03772
- ParticleNet paper (conceptual architecture background): https://arxiv.org/abs/1902.08570
- Jet substructure review (Larkoski, Moult, Nachman): https://arxiv.org/abs/1709.04464
- W polarization substructure study: https://link.springer.com/article/10.1007/JHEP05(2025)028
- uproot + awkward-array for validation plots: https://uproot.readthedocs.io / https://awkward-array.org
- PyTorch 60-min blitz (if needed): https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html

---

### Phase 2 — Week 2: Generate small polarized samples and validate them

**Goal:** A working private generation chain producing polarized W jets,
validated on ~10k events per class.

**Tasks:**

1. **Set up the generation environment (day 1).** Install MadGraph5_aMC@NLO
   with Pythia8 and Delphes following the `jetclass2_generation` README
   (the LCG_104 stack on lxplus EL9 nodes covers all dependencies).
   Download the `2HDM` model inside MadGraph.

2. **Write the polarized process cards (days 2–3).**  
   Create new subdirectories in `gen_configs/`:
   - `jetclass2_polarized/train_W_L/` for longitudinal W
   - `jetclass2_polarized/train_W_T/` for transverse W  
   
   Use MadGraph's polarized process syntax (available since MG5_aMC v2.7):
   ```
   # mg5_step1.dat for W_L
   generate p p > w+{L} j, w+{L} > j j
   ```
   Reuse the existing `train_qcd` config for background.
   Have the supervisor review the process cards before running — this is the
   single highest-value checkpoint of the whole project.

3. **Run small local jobs (day 4).** Generate ~10k events per class locally
   (no condor yet). Pass through Delphes with
   `delphes_card_CMS_JetClassII_onlyFatJet.tcl` (cheapest card, fatjets only)
   and ntuplize with `makeNtuples.C`.

4. **Validate the physics (day 5).** This is the key physics step before
   trusting anything downstream. Plot:
   - Jet pT, softdrop mass, N-subjettiness τ21
   - Subjet energy asymmetry (E1 − E2)/(E1 + E2)
   - Reconstructed cos θ* proxy for W_L vs W_T  
   
   Check that W_L shows the expected sin²θ* distribution and W_T shows
   1 + cos²θ* at generator level. If the distributions look wrong, stop
   and debug the process cards.

**Feasibility:** ⚠️ Medium risk — this is the riskiest phase of the project.
MadGraph environment problems and polarization syntax are where a
beginner can lose several days. Two mitigations are essential:
(a) supervisor review of the process cards on day 2;
(b) agree in advance that if generation fails, the fallback is to use
existing JetClass-II X→WW events relabeled by generator-level polarization
information — this preserves the ML part of the project entirely.

**Training resources:**
- MadGraph tutorial (CERN school material): https://indico.cern.ch/category/6285/
- Polarized matrix elements in MG5_aMC: https://arxiv.org/abs/1912.01725
- Pythia8 documentation: https://pythia.org/documentation/
- Delphes quick tour: https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/QuickTour
- ROOT/uproot for validation: https://uproot.readthedocs.io

---

### Phase 3 — Week 3: Scale up production and fine-tune ParT

**Goal:** Full training dataset on EOS and first fine-tuned polarization tagger.

**Tasks:**

1. **Port generation to HTCondor (days 1–2).** Use the `[job_num]` argument
   already built into `run.sh` to split jobs. Target ~500k jets per class
   (W_L, W_T, QCD). At 100 events per generation step this is ~1500 short
   jobs — manageable on lxbatch over 2–3 days of wall time. Write the
   jobs to EOS.

   Example condor submit file:
   ```
   executable = run.sh
   arguments  = jetclass2_polarized/train_W_L 100000 100 $(ProcId)
   queue 50
   ```

2. **Write the weaver data config (days 2–3).** Create a new YAML file for
   the polarized classes, starting from `data/JetClass/JetClass_kin.yaml`.
   Use the `kin` feature set (kinematics only) for the first run — it is
   the most portable and avoids issues with displacement variables that
   the lite Delphes card may not populate.

3. **Fine-tune ParT (days 3–5).** Run fine-tuning from the pre-trained
   JetClass checkpoint (analogous to `ParT-FineTune` in
   `train_QuarkGluon.sh`):
   - **Binary task:** W_L vs W_T
   - **3-class task:** W_L vs W_T vs QCD (if sample sizes allow)
   
   Also run a from-scratch ParT training on the same data as a baseline
   to quantify how much the pre-training helps. Track loss and validation
   AUC with TensorBoard. Keep all configs and checkpoints in git.

**Feasibility:** ✅ Medium-high risk. Condor babysitting is tedious but
standard; fine-tuning on ~1.5M jets takes a few hours on a single modern
GPU. The dataset is ~100× smaller than JetClass — precisely the regime
where fine-tuning a pre-trained model should demonstrate a clear advantage,
which is itself an interesting result.

**Training resources:**
- HTCondor at CERN (quickstart + GPU jobs): https://batchdocs.web.cern.ch/local/quick.html
- weaver-core usage and YAML configs: https://github.com/hqucms/weaver-core
- Pre-trained ParT checkpoints: https://github.com/jet-universe/particle_transformer/tree/main/models
- ParT fine-tuning section: arXiv:2202.03772 Section 4
- TensorBoard quickstart: https://pytorch.org/tutorials/recipes/recipes/tensorboard_with_pytorch.html

---

### Phase 4 — Week 4: Evaluation, physics interpretation, write-up

**Goal:** Quantified tagger performance, sanity checks, and a written
report + group presentation.

**Tasks:**

1. **Performance metrics (days 1–2).**
   - ROC curves and AUC for fine-tuned ParT vs from-scratch ParT vs
     classical cos θ* baseline
   - Background rejection at 50% and 70% signal efficiency
   - Performance as a function of jet pT (check for pT dependence)

2. **Physics sanity checks (day 3).**
   - Does the network discriminant correlate with the generator-level
     decay angle? (If yes, it is learning the right thing.)
   - Does the tagger sculpt the softdrop jet mass? (It should not.)
   - Compare the learned attention weights of ParT to the subjet energy
     asymmetry — is the model focusing on the expected features?

3. **Write-up (days 3–5).**  
   Produce a 10–15 page report covering: physics motivation, generation
   setup and validation, ML architecture and training, results, and
   outlook. This becomes directly usable as a thesis chapter.  
   Prepare a 15-minute presentation for the group meeting.

4. **Buffer.** Realistically 2–3 days of this week absorb overruns from
   weeks 2–3. Do not plan stretch goals unless you are ahead by day 3.

**Feasibility:** ✅ Low risk, provided weeks 2–3 delivered samples.
The scientific bar for a one-month project is met even with the binary
W_L / W_T result alone — it is a complete, publishable-quality study unit.

**Training resources:**
- scikit-learn ROC/AUC: https://scikit-learn.org/stable/modules/model_evaluation.html
- mplhep for publication-style HEP plots: https://mplhep.readthedocs.io
- Mass decorrelation context (reading only): https://arxiv.org/abs/1603.00027
- ML approaches to polarization tagging: https://arxiv.org/abs/2306.07726

---

## Summary timeline

| Week | Phase | Main risk | Supervisor checkpoint |
|---|---|---|---|
| Days 1–2 | Phase 0: Accounts, environment | Admin delay — start early | Confirm GPU access |
| Week 1 | Phase 1: Reproduce ParT on TopLandscape + reading | Low | Review ROC curve |
| Week 2 | Phase 2: Polarized generation + validation plots | **High** — process cards | **Review process cards day 2** |
| Week 3 | Phase 3: Condor scale-up + fine-tuning | Medium | Check validation AUC mid-week |
| Week 4 | Phase 4: Evaluation + report | Low | Draft report review day 3 |

---

## The single most important de-risking action

Have the supervisor (or another MadGraph-experienced person) review the
polarized process cards on **day 2 of week 2** — before any generation
runs. Everything downstream is mechanical by comparison, and catching a
wrong process definition early saves 3–4 days of debugging.

The agreed fallback, to be established on day one of the project, is:
use existing JetClass-II X→WW events and relabel them by generator-level
polarization. This preserves the entire ML training and evaluation pipeline
even if generation fails.

---

## Suggested reading order for the first week

1. arXiv:2202.03772 — ParT paper (architecture + JetClass dataset)
2. arXiv:1709.04464 §1–3 — Jet substructure primer
3. JHEP 05 (2025) 028 — W polarization proxy from subjet energies
4. JetClass-II generation README — understand the MG+PY8+Delphes chain
5. arXiv:2306.07726 — ML-based longitudinal boson tagger (for context)
