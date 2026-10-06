[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.nm000276-blue)](https://doi.org/10.82901/nemar.nm000276)

# SWEC iEEG Dataset (BIDS)

Long-term pre-surgical intracranial EEG (iEEG) from patients with
pharmacoresistant epilepsy, recorded at the Sleep-Wake-Epilepsy-Center (SWEC),
Department of Neurology, Inselspital, University of Bern, in collaboration with
the Integrated Systems Laboratory, ETH Zurich.

## Contents
- Subjects: 50 (`sub-01` ... `sub-50`; 40 since v1.0.0, 10 added in v1.1.0)
- Total recording: 6672 hours of continuous iEEG
- Annotated electrographic seizures: 460
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
v1.0.0 (40 subjects) was converted to BIDS from the public HDF5 release at
https://mb-neuro.medical-blocks.ch/public_access/databases/ieeg/swec_ieeg
(authentic medical-blocks / artorg source) and re-hosted its 40-subject public subset.

v1.1.0 adds the 10 remaining subjects of the same cohort (`sub-07`, `sub-13`,
`sub-14`, `sub-17`, `sub-21`, `sub-25`, `sub-26`, `sub-40`, `sub-44`, `sub-49`)
from the authors' Hugging Face release `NeuroTec/SWEC_iEEG_Dataset`, revision
`584e9d29313ad6d2ed675b5d5202240f4ff75970`
(https://huggingface.co/datasets/NeuroTec/SWEC_iEEG_Dataset). Every source file
was verified against its Hugging Face LFS sha256 before conversion. Conversion is
lossless: the float32 `data/ieeg` samples of the HDF5 part files are written
unchanged, in part order, into one continuous BrainVision file per subject (no
filtering, resampling, re-referencing, scaling or channel removal), and the
`data/seizures` onsets/offsets become `events.tsv` rows. The source holds no
timing information beyond this gap-free concatenation of parts.

Subject crosswalk. `sub-NN` here is upstream patient NN. In the Hugging Face
release the same patient is folder `ID{NN+18}` (e.g. `sub-07` = `ID25`,
`sub-50` = `ID68`): the virtual-dataset index of every `IDxx_total.h5` still
points to `ID{xx-18}_part_*.h5`, and the 40 v1.0.0 subjects match folders
ID19-ID68 exactly in channel count, sampling rate, sample count and (for 39 of
40) seizure table. The same converter reproduces 39 of the 40 v1.0.0 `.eeg`
files byte-for-byte (sha256 identical) from the Hugging Face files. The
exception is `sub-02` (see Corrections). The
`participants.tsv` column `source_id` records the Hugging Face folder of each
subject.

Hugging Face folders ID01-ID18 are a second copy of 18 of these patients
(ID01-ID18 = ID20, ID21, ID22, ID24, ID25, ID27, ID28, ID29, ID30, ID31, ID32,
ID34, ID35, ID36, ID37, ID38, ID39, ID40: identical channel count, sampling
rate, sample count and seizure table; compressed signal chunks byte-identical
where compared). They are not added again. The card's figures (68 subjects,
9328 hours, 704 ictal events) count these 18 copies; the 50 distinct patients
total 6672 hours and 460 annotated seizures.

## Corrections in v1.1.0
- `sub-02`: in the v1.0.0 file, 6 of the 20 source parts (parts 1, 6, 9, 14,
  15 and 16; 170,454,540 samples, about 92.5 h) were all zeros. The source
  holds real signal there (Hugging Face `ID20`); the other 14 parts are
  identical. v1.1.0 replaces the file with the complete conversion, same size,
  sampling rate and channels.
- `sub-01`: v1.0.0 shipped no `events.tsv`. The Hugging Face release annotates
  25 seizures for this patient (folder `ID19`), whose signal is byte-identical
  to the v1.0.0 file. v1.1.0 adds them.

## How to cite
Carzaniga, F., Hersche, M., Sebastian, A., Schindler, K. & Rahimi, A.
"A foundation model with multi-variate parallel attention to generate neuronal
activity." arXiv:2506.20354 (2025). https://doi.org/10.48550/arXiv.2506.20354
SWEC-ETHZ iEEG Database: http://ieeg-swez.ethz.ch/

## License
Community Data License Agreement – Permissive, Version 2.0 (CDLA-Permissive-2.0).

The Hugging Face dataset card (revision `584e9d29`) states:
"The iEEG SWEC dataset is licensed using the Community Data License Agreement –
Permissive, Version 2.0" and, under "Disclaimer": "This dataset may only be used
for research. For other applications any liability is denied. In particular, the
dataset must not be used for diagnostic purposes."
