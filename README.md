# SWAT soil moisture sensitivity modified code and executable

## This repository contains:
1. modified SWAT (v. 664) source code (/modified_source_code/)
2. example modified inputs needed to run modified code (/example_inputs/)
3. compiled modified code Windows executable (/windows_executable/)

## Background
The theory and application of this code is documented in the manuscript, [Pignotti et al. (2021)](https://www.sciencedirect.com/science/article/abs/pii/S136481522100205X)

## Code changes
- Edited modparm.f, readbsn.f, and percmain.f
  - Created new variable: sa_val
  - modparm.f: add new variale into SWAT global variables (real :: sa_val)
  - readbsn.f: added sa_val to be read at the bottom of the basins.bsn file
  - percmain.f: multiply second surface soil moisture layer by sa_val each day before any of the other processes, including percolation and lateral flow (to determine its effect on them in the model)

## Input file changes needed to run code
- basins.bsn (last line 132 – SA_VAL; see example file in ./example_inputs/)
- The value provided will be a multiplicative factor applied to the upper soil moisture layer (as a percentage change)
- When specifying the SA_VAL perturbation parameter keep in mind:
  - A percentage change here means how much to decrease or increase soil moisture by
  - To decrease, sa_val must be < 1; factor will be applied as (SA_VAL) * soil moisture
  - To increase, sa_val must be > 1; factor will be applied as (SA_VAL) * soil moisture
  - No change to soil moisture is a sa_val = 1.0 (multiplied by 1.0)
  - **Example 1, decrease soil moisture by 10%: sa_val = 1 - 0.10 = 0.90**
  - **Example 2, increase soil moisture by 20%: sa_val = 1 + 0.20 = 1.20**
- The upper soil moisture layer is the 2nd actual soil layer as SWAT adds a 10 mm layer by default

## Other changes
The code has also been modified to print out total soil water content, rather than available soil water content, in the output.swr file. Available soil water content is soil water content less the wilting point soil water content. The model will also print out a new file, output.dg, which provides the depths of all soil layers. This can be used along with output.swr to get a fractional soil water content (i.e. output.swr value / output.dg value).
