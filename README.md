# Little help to reconstruct data

**Date:** July 2025

This document describes how to use a really basic pipeline for SuperNEMO data reconstruction from SD to UDD to PCD for simulation and from UDD to PCD for data for the teo first phases of data taking. This method should be used only until official production is done.

---

## Basic use of the code

The code is divided in several Falaise module (lots of them from other people in the collaboration) and can be launched using the following command (the git clone should only be done the first time):

```bash
$> git clone https://github.com/Maathiiss/SN-reco_phase_-0-1-
$> source /sps/nemo/scratch/chauveau/software/falaise/develop/this_falaise.sh
$> flreconstruct -p UDD_to_ptd_data/simu_phase_0/1.conf -i UDD.brio -o PTD.brio
```

You'll obtain a PTD.brio file for simulation and data processed the same way and that should be comparable.

---

## Understand each module

The pipeline is divided into 5 different modules:

- `udd2pcd`
- `pcd2cd`
- `TKReconstruct` → from CD to TTD bank
- `ChargedParticleTracker` → from TTD to PTD bank

Lets start with the two modules `udd2pcd` and `pcd2cd`:

Theses two modules were written by Manu. This code is used to switch from the UDD bank to the CD bank (files are here: `/sps/nemo/snemo/snemo_data/reco_data/UDD/delta-tdc-10us-v3/snemo_run-XXXX_udd.brio`).

### `udd2pcd`

Available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/udd2pcd_module.cc#L35). Pre-calibration, such as z calculation.

### `pcd2cd`

Is using 2 files for the calibrations:

- Time calibration file → file produced by Manu for now, stored in my scratch directory. Easily replaceable.
- Energy calibration file → file produced by me using only the "a" parameter in the calibration, stored in my scratch directory. Easily replaceable.

The code is available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/pcd2cd_module.cc)

### `TKReconstruct`

The TKReconstruct algorithm, and not the Cimmerman algorithm, is used for sake of simplicity. This is a short term solution that will be changed in the future.

### `ChargedParticleTracker`

Particles trajectory extrapolation on the foil and calorimeter to identify particle's nature.
