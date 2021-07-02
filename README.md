# SWAT soil moisture sensitivity modified code and executable

## This repository contains:
1. modified SWAT (v. 664) source code (/modified_source_code)
2. example modified inputs needed to run modified code (/example_inputs)
3. compiled modified code Windows executable (/windows_executable)

## Background
The theory and application of this code is document in the manuscript, [TBD](link)

## Code changes
- Edited modparm, readbsn, and percmain
  - Created new variable: sa_val
  - modparm.f: create new multiplicative variable to perturb soil moisture -- real :: sa_val
  - readbsn.f: added sa_val to be read at the bottom of the file
  - percmain.f: multiply whole soil moisture layer by sa_val each day before any of the other processes, including percolation and lateral flow, since want to know its effect upon them in the model


