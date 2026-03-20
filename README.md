# Little help to reconstruct data

**Date:** July 2025

This document describes how to use a really basic pipeline for SuperNEMO data reconstruction from SD to PTD for simulation and from UDD to PTD for data. This method should be used only until official production is done.

---

## Basic use of the code

The code is divided in several Falaise module (lots of them from other people in the collaboration) and can be launched using the following command (the git clone should only be done the first time):

```bash
$> git clone https://github.com/Maathiiss/SN-reco
$> snswmgr_load_setup falaise@5.1.9
$> flreconstruct -p UDD_to_ptd_data/UDD_to_ptd_data.conf -i UDD.brio -o PTD.brio
```

You'll obtain a PTD.brio file for simulation and data processed the same way and that should be comparable.


## Understand each module

The pipeline is divided into 5 different modules:

- `udd2pcd`
- `pcd2cd`
- `Cimrman` → from CD to TTD bank
- `ChargedParticleTracker` → from TTD to PTD bank

Lets start with the two modules `udd2pcd` and `pcd2cd`:

Theses two modules were written by Manu. This code is used to switch from the UDD bank to the CD bank (You can find UDD files at this path : `/sps/nemo/snemo/snemo_data/reco_data/UDD/delta-tdc-1600us-v3/snemo_run-${run_number}_udd.brio`).

### `udd2pcd`

Available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/udd2pcd_module.cc#L35). Pre-calibration, such as z calculation.

### `pcd2cd`

Is using 2 files for the calibrations:

- Time calibration file → file produced by Manu for now, stored in my scratch directory. Easily replaceable.
- Energy calibration file → file produced by me using only the "a" parameter in the calibration, stored in my scratch directory. Easily replaceable.

You need to change the calibration file with the closest one present here : /sps/nemo/scratch/granjon/full_gain_analysis/Bi/root/compute_a/data_calibration_pol2/

The code is available [here](https://github.com/SuperNEMO-DBD/Falaise/blob/c0b9854a02b6703080c9680ad8822deded0b6045/source/falaise/snemo/processing/pcd2cd_module.cc)

### `Cimrman`

The Cimrman algorithm, have a lots of parameters that can be changed. They are for the moment based on 2nu and 0nu efficiency (see docDB #6186).
Alphas particle and huge kinks are possible but not optimized.

### `ChargedParticleTracker`

Particles trajectory extrapolation on the foil and calorimeter to identify particle's nature.
