# Herding Quantum Cats · Team Quantum Thinkers

Our solution to the **Herding Quantum Cats** challenge at **Qiskit Fall Fest 2026**, University of Stuttgart (10 November 2026).

The challenge: prepare the cleanest 12-qubit GHZ ("cat") state and the biggest certifiably entangled one, first on a noisy simulator of IBM's 156-qubit Heron processor and then on the real **ibm_kingston**.

**Team:** Abhishek Vijay Potekar · Haniyeh Vakilian · Rajveer Shaw · Pakhi Rao · Ismail Paktiawal

## Results

| | Organizers' baseline | Ours |
|---|---|---|
| Cleanest cat, simulator (12 qubits) | F = 0.41 (not certified) | **F = 0.978 ± 0.017**, error-reduction factor R ≈ 15 |
| Cleanest cat, ibm_kingston (same job) | F = 0.599 ± 0.008 | **F = 0.913 ± 0.016**, R = 3.9 |
| Biggest certified cat, ibm_kingston | – | **32 qubits**, F = 0.587 ± 0.013 (submitted before the hardware deadline) |
| Biggest certified cat, ibm_kingston, late run | – | 40 qubits, F = 0.599 ± 0.018 (see note below) |
| Biggest certified cat, simulator | – | 48 qubits, F = 0.694 ± 0.029 |

F is a certified lower bound on the fidelity, F = P + (X − 1)/2. A cat counts as certified when F − 2σ > 0.5, which proves genuine N-qubit entanglement.

## How we did it

1. **Layout search.** The baseline sits on qubits 0–11, and qubit 1 has a 16.8% readout error. A depth-first search with pruning scores every 12-qubit chain on the chip by its readout and CZ errors and picks the best one. F: 0.44 → 0.72.
2. **Grow the cat from the middle.** The Hadamard goes on the middle qubit and the cat grows in both directions at once: 11 → 6 CZ layers and less idle time. F → 0.74.
3. **TREX readout mitigation.** Removes readout errors from both measured quantities. F → 0.94.
4. **Linear zero-noise extrapolation** (noise ×1, ×3). F → 0.978, stable across simulator seeds.

For big cats we grow breadth-first **trees** on good qubits only, with dynamical decoupling on hardware. The evidence table in the notebook (Mission 6) lists every step, including what did not help.

## Repository contents

| File | What it is |
|---|---|
| `team_Quantum_Thinkers.ipynb` | Our executed challenge notebook (Missions 0–8), with all outputs |
| `herding_cats.py` | The organizers' toolkit, unchanged |
| `submission_Quantum_Thinkers.json` | The submission file as we posted it (Biggest Cat = the 40-qubit late run) |
| `submission_Quantum_Thinkers_in_deadline_32.json` | The same submission with our in-deadline 32-qubit Biggest Cat |
| `final_job.json`, `big_job_*.json`, `*.qpy` | Saved hardware jobs and circuits, so the notebook can re-analyse our runs |
| `pitch/` | Our 5-minute pitch deck (PDF and PowerPoint) |

## Run it

```bash
python -m venv fallfest
fallfest\Scripts\activate            # macOS/Linux: source fallfest/bin/activate
pip install -r requirements.txt
jupyter lab team_Quantum_Thinkers.ipynb
```

The simulator missions run offline. The hardware cells only re-analyse our saved jobs (`SUBMIT_NEW_JOBS = False`), which needs an IBM Quantum account with access to those jobs. The chip-map drawings need [Graphviz](https://graphviz.org/download/).

## Note on the 40-qubit hardware cat

The challenge required final hardware jobs to be submitted by 18:45. Our 32-qubit cat (job `dbpkb85t7fqc7389ja9g`) was submitted at 17:26. Haniyeh's 40-qubit cat (job `dbpma344148s739us5c0`) ran on ibm_kingston at 19:40, after that deadline, and we told the organizers so. Both results are real hardware runs with their raw counts stored in the submission files.

## Credits and license

The challenge, the `herding_cats.py` toolkit and the notebook template are by the Qiskit Fall Fest 2026 Stuttgart organizers ([kaiuki2000/herding-cats](https://github.com/kaiuki2000/herding-cats)) under the Apache License 2.0, see `LICENSE`. The pitch deck uses the Qiskit Fall Fest 2026 slide template, MIT License, see `LICENSE-TEMPLATE`. Our solutions in the notebook are shared under the same Apache 2.0 license.
