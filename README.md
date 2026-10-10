# PhD Oral Defense

Materials for my PhD oral defense at the National University of Singapore (NUS), titled **“Machine Learning Augmented Simulation and Analysis of Dynamical Systems.”**

The presentation explores how machine learning can enhance scientific computing while preserving mathematical structure, through three themes:

- learning representations with parametric Koopman operators;
- learning problem-adapted numerical integrators;
- learning integrator-aware equations for molecular dynamics.

## Repository contents

- [`slides/index.html`](slides/index.html) — interactive Reveal.js presentation
- [`slides/PhD_Oral_Defense_GuoYue.pdf`](slides/PhD_Oral_Defense_GuoYue.pdf) — exported slide deck
- [`slides/speaker_script.md`](slides/speaker_script.md) — bilingual Chinese–English speaker notes
- [`slides/img/`](slides/img/) — figures, photos, and video assets

## View the slides

Open the PDF directly, or serve the interactive version locally:

```bash
cd slides
python3 -m http.server 8000
```

Then visit <http://localhost:8000>. An internet connection is required to load the Reveal.js, MathJax, and 3Dmol.js dependencies used by the interactive deck.

## License

This repository is licensed under the [MIT License](LICENSE).
