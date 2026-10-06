# Task-SEEG recordings from epilepsy patients performing a visual working memory task

This is a BIDS (iEEG) copy of the Figshare dataset

> Tian, Ziwei (2026). *task-SEEG data from epilepsy patients.* figshare. Dataset.
> https://doi.org/10.6084/m9.figshare.32189838.v1 (article 32189838, version 1, published 2026-05-07)

Source description (Figshare, verbatim): "This dataset contains stereo-electroencephalography (SEEG) recordings
acquired from epilepsy patients while they performed a visual working memory task. The data were collected to
investigate the neural mechanisms underlying working memory processes in the human brain." Participants: "patients
with drug-resistant epilepsy who underwent SEEG implantation for clinical evaluation. Electrode placement was
determined solely by clinical requirements, with no additional electrodes implanted for research purposes."
Electrodes: "Intracranial multi-contact depth electrodes (8–16 contacts; length, 2 mm; diameter, 0.8 mm; spacing,
1.5 mm; Huake-Hengsheng Medical Technology, Beijing, China) were stereotactically implanted with robotic
assistance." Recording: "Amplifier: Nicolet EEG; Sampling rate: 2048 Hz; Online reference: A white matter site
selected by the clinical team." Task: "Participants performed a visual working memory task. Further details of the
experimental paradigm can be found in the accompanying research article."

## Contents
- 7 participants (sub-01 to sub-07), 8 continuous EDF recordings (sub-03 has two runs), task label `vwm`.
- 2048 Hz, 148 signals per recording (the EDF+ annotation signal of sub-06 and sub-07 is not counted).
- Total duration 22,168 s (6.2 h): 1,080.6 to 4,010.9 s per recording (exact values in each `_ieeg.json`).
- Units as in the EDF headers (µV for SEEG contacts). No filtering, resampling, re-referencing or channel removal
  was done for this copy. The EDF prefilter fields are empty, so `low_cutoff`/`high_cutoff` are n/a.
- `participants.tsv`: age, sex, years of education, seizure-onset zone, and the contacts the authors list as frontal
  eye field and hippocampus. Values are verbatim from the source (columns renamed to snake_case; `male`/`female`
  coded M/F).

## What the source does not contain
- **No events, trial or behavioural files.** The Figshare record has none, and the EDFs have no task annotations
  (sub-07 has one EDF+ annotation "renwu", pinyin for "task", at 0.75 s; sub-06 and sub-07 also have machine log
  entries). The `TRIG`
  channel carries a few analog-like transitions per recording (about 80 to 170 sample-to-sample changes), not a clean
  digital code. It was kept but not decoded into events. Users need the paradigm from the authors' article.
- **No electrode coordinates and no imaging.** `electrodes.tsv` lists the SEEG contact names with x/y/z = n/a.
  `coordsystem.json` says "Other" with units n/a.
- No ethics statement and no funding statement are given in the record.

## Channel types (changed from the source)
The source `channels.tsv` (MNE-BIDS output) typed all 148 signals as SEEG. Here the types follow the EDF labels:
SEEG for electrode contacts (letter, optional prime marks, number); ADC for `DC1`–`DC16` (Nicolet DC auxiliary
inputs); TRIG for `TRIG`; MISC for `OSAT`, `PR`, `Pleth` and for amplifier inputs with the Nicolet default names
`C123`–`C128`, which the source does not assign to an electrode. Channels whose digital value is constant over the
whole recording are marked `bad` with `status_description` "flat". The source channel tables are kept in
`sourcedata/`.

## Power-line frequency
The source sidecars say "n/a". `PowerLineFrequency` is set to 50 because the recordings show a clear 50 Hz peak
(spectral peak-to-neighbour ratio 18 to 6,400 at 50 Hz, about 1 at 60 Hz, in every recording).

## Licence
The Figshare record gives two different licence statements:
1. The record's licence field: **"CC BY 4.0"** (https://creativecommons.org/licenses/by/4.0/).
2. The record's description, "Conditions of use": **"This dataset is made available under [CC0 / CC BY-NC 4.0]
   license. Users are requested to cite the associated research paper when using these data."**

The depositor (Bruno Aristimunha, 2026-10-06) decided to apply the most restrictive of the stated licences. This
copy is therefore released under **CC BY-NC 4.0 (`CC-BY-NC-4.0`)**. Commercial use is not permitted under this
copy. If the author clarifies the licence, this copy will be updated.

## Associated publications
The Figshare record refers to "the accompanying research article" but does not name it. These articles by the
same first author describe SEEG recordings during a visual working memory task with eye tracking in epilepsy
patients. The link is inferred from author, paradigm and dates; the record does not state it:
- Tian Z, Huang S, Liu D, Yang Z, Li S, Hu B, Feng L, Wang Q. Hippocampal interictal spikes disrupt theta-band
  hippocampal-frontal eye field connectivity: fixation skewness as an indicator of transient network instability.
  *Epilepsy & Behavior* 181:111071 (2026). https://doi.org/10.1016/j.yebeh.2026.111071 (seven epilepsy patients
  with hippocampal and frontal-eye-field iEEG; published two days after the Figshare record).
- Tian Z, Huang S, Liu D, Yang Z, Li S, Hu B, Wang Q, Feng L. Distinct and coordinated contributions of hippocampus
  and frontal eye field to novelty exploration and revisitation. *NeuroImage* 329:121838 (2026).
  https://doi.org/10.1016/j.neuroimage.2026.121838
Both list affiliations at the Xi'an Institute of Optics and Precision Mechanics (CAS) and Xiangya Hospital, Central
South University. The Figshare record names neither institution.

## De-identification done for this copy
The source EDF headers contain a hospital record number, the date of birth and the real recording date. The
source `scans.tsv` files contain the real recording dates. The EDF+ annotation signals of sub-06 and sub-07 hold a
`Montage:` entry that contains a personal name (the files do not say whose; it may be the name of a montage or of a
staff member, or a patient). For this copy:

Dates truncated to month (day set to 01).

- EDF patient field (hospital record number, sex, date of birth) set to `X X X X`. Recording field set to
  `Startdate 01-MMM-YYYY X X X` and start date to `01.MM.YY`, keeping the source year and month. The time of day is
  kept.
- In sub-06 and sub-07, the text after `Montage:` in the EDF+ annotation signal is replaced by `X` characters of
  the same byte length. Other annotations ("Clip Note", "Gain/Filter Change", "renwu") are kept.
- `scans.tsv` acq_time keeps year, month and time of day, with the day set to 01. The two sub-03 runs were
  recorded on the same day, so the interval between them is kept.
- A byte comparison against the source confirmed that no other byte changed: all signal samples are identical
  to the Figshare files.
- The source EDF and `scans.tsv` files are therefore **not** included in `sourcedata/`. The other source files
  (README, dataset_description, participants, channels and sidecar JSON) are included unchanged in
  `sourcedata/figshare-32189838-v1/`. `PROVENANCE.tsv` there lists every source file with its Figshare MD5 and
  whether it is included.
