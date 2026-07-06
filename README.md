# COMETA Workshop Report — Polarization Tagging for Hadronic Boosted Bosons

LaTeX report summarizing the **COMETA workshop on polarization tagging for
hadronic boosted bosons**, Perugia (IT), 1–2 July 2026.
Indico: https://indico.cern.ch/event/1601346/

## Structure

```
main.tex               # main document (compile this with pdfLaTeX)
sections/
  01_introduction.tex        # Welcome / physics case
  02_semileptonic.tex        # Day 1 — Semi-leptonic decay channels
  03_jet_substructure.tex    # Day 1 — Jet substructure and tagging
  04_quantum_ml.tex          # Day 1 — Quantum machine learning
  05_open_data.tex           # Day 1 — Open data discussion
  06_mc_modeling.tex         # Day 2 — MC modeling incl. polarisation
  07_experimental_status.tex # Day 2 — Experimental status on polarisation
  08_ml_approaches.tex       # Day 2 — Machine-learning approaches
  09_closing_discussion.tex  # Day 2 — Closing discussion / COMETA deliverable
```

Each section mirrors one block of the workshop agenda; each contribution is
summarized in roughly half a page.

## Build locally

```bash
latexmk -pdf main.tex
```

## Use with Overleaf

Two options:

1. **GitHub sync (recommended).** Push this repository to GitHub, then in
   Overleaf: *New Project → Import from GitHub* and select the repo. Overleaf
   auto-detects `main.tex` as the root document (pdfLaTeX). Any further pull
   requests merged into the GitHub repo can be pulled into Overleaf via
   *Menu → GitHub → Pull*.
2. **Zip upload.** Zip the repository contents and use
   *New Project → Upload Project* in Overleaf.

## Notes on sources

Summaries are based on the slide decks uploaded to Indico. Two decks
(`LM_UJ_2026.pdf`, traditional jet substructure; `Perugia_Presentation.pdf`,
boosted VBS semi-leptonic prospects) exceeded the retrievable size and are
summarized from the contribution abstracts and session context, as flagged in
footnotes in the document.
