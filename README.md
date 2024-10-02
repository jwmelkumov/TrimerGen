# TrimerGen

TrimerGen is a molecular utility program that generates trimers in an 11D space. The trimers are
described by the radial distance between the centers of mass (COMs) of their constituent monomers and 8 Euler angles (in the ZYZ convention). 
The user must provide the geometry of each monomer (in XYZ file format), along 
with 11 coordinates: $( R_{\rm COM}^{\rm AB}, R_{\rm COM}^{\rm BC}, R_{\rm COM}^{\rm AC}, \beta_A, \gamma_A, \alpha_B, \beta_B, \gamma_B, \alpha_C, \beta_C, \gamma_C )$. Note: $\alpha_A$ is hardcoded and set to 0.

## Table of Contents  
- [Euler Angles and Rigid Body Rotations](#euler-angles-and-rigid-body-rotations) 
- [Notes](#notes) 
- [Dependencies](#dependencies) 
- [Usage](#usage) 

## Euler Angles and Rigid Body Rotations

<p align="center">
<img src="https://raw.githubusercontent.com/jwmelkumov/DimerGen/main/Documentation/EulerRotations.png" width="400">
</p>

Given a set of atomic coordinates and masses (automatically extracted via lookup table from the substrings of
atomic labels in the input XYZ file) for each monomer, the COM is computed and the coordinates are shifted such that the COM is at the origin. The inertia tensor is then computed for each monomer and the principal axes are obtained by solving for the eigenvectors of the inertia tensor. After the principal axes are obtained, the coordinates of each monomer are transformed into the principal axes frame. Using the user-specified Euler angles, the rotation matrix is constructed and applied to the coordinates in the principal axes frame. The rotated coordinates are then transformed back to the original frame and the separation vector between the two monomers is used to translate the coordinates by the user-specified separation (in angstroms).

The convention used here is ZYZ, so the rotation matrix is constructed as:
<p align="center">
<img src="https://raw.githubusercontent.com/jwmelkumov/DimerGen/main/Documentation/R.png" width="400">
</p>

where
<p align="center">
<img src="https://raw.githubusercontent.com/jwmelkumov/DimerGen/main/Documentation/Rz.png" width="400">
</p>

and
<p align="center">
<img src="https://raw.githubusercontent.com/jwmelkumov/DimerGen/main/Documentation/Ry.png" width="400">
</p>

## Notes
- TrimerGen supports monomers with off-atomic sites (such as M in TIP4P and LP "lone pairs" in force fields).
- Monomers containing all elements are supported.
- This is not a simulation package. As such, there is no potential energy function or scoring function implemented. This is a utility that is meant to be paired with external automation and logic.

## Dependencies
- NumPy
- Python3

## Usage 
```bash
./pyTrimerGen.py xyzfileA xyzfileB xyzfileC RCOMAB RCOMBC RCOMAC betaA gammaA alphaB betaB gammaB alphaC betaC gammaC
```
