[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.nm000276-blue)](https://doi.org/10.82901/nemar.nm000276)

# SWEC iEEG Dataset (BIDS)

Long-term pre-surgical intracranial EEG (iEEG) from patients with
pharmacoresistant epilepsy, recorded at the Sleep-Wake-Epilepsy-Center (SWEC),
Department of Neurology, Inselspital, University of Bern, in collaboration with
the Integrated Systems Laboratory, ETH Zurich.

## Contents
- Subjects: 40
- Total recording: 5376 hours of continuous iEEG
- Annotated electrographic seizures: 381
- Task label: `ltm` (long-term monitoring; passive, no task)
- Format: BrainVision (IEEE float32), data in microvolts (µV)

## Recording & preprocessing (at source)
- Intracranial strip, grid, and depth electrodes (mixed).
- 16-bit analog-to-digital conversion.
- Sampling rate 512 Hz or 1024 Hz (per subject; see participants.tsv).
- Digitally band-pass filtered 0.5-150 Hz (4th-order Butterworth, forward-backward / zero-phase).
- Channels with artifacts were removed at the source; remaining channels marked `good`.
- Median reference across channels (per the SWEC-ETHZ long-term protocol).
- Seizure onsets/offsets annotated by board-certified epileptologist Prof. Kaspar Schindler.

## Anonymization — channels & electrodes
The public SWEC release is FULLY ANONYMIZED. Anatomical channel labels and
electrode locations are NOT disclosed (electrode coordinates + the implied
imaging would be re-identifying for epilepsy-surgery patients). Channels are
therefore named `iEEG01..iEEGNN`, preserving the original recording order
(1:1 with the source channel index 0..N-1). `electrodes.tsv` lists every
contact with coordinates set to `n/a`, and channel type is recorded as the
generic intracranial `SEEG` (the per-contact strip/grid/depth type is not
disclosed). No locations were fabricated.

## Provenance
Converted to BIDS from the public HDF5 release at
https://mb-neuro.medical-blocks.ch/public_access/databases/ieeg/swec_ieeg
(authentic medical-blocks / artorg source). This BIDS dataset re-hosts the
40-subject / 378-file public subset.

## How to cite
Carzaniga, F., Hersche, M., Sebastian, A., Schindler, K. & Rahimi, A.
"A foundation model with multi-variate parallel attention to generate neuronal
activity." arXiv:2506.20354 (2025). https://doi.org/10.48550/arXiv.2506.20354
SWEC-ETHZ iEEG Database: http://ieeg-swez.ethz.ch/

## License
Community Data License Agreement – Permissive, Version 2.0 (CDLA-Permissive-2.0).
