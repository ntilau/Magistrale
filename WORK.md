## Model Order Reduction in Full-Wave Analysis of Phased Array Antennas

### Papers

* + Antenna Arrays : DFT analysis of large antenna arrays
  + Approximation : Reduced Basis, POD, SVD
  + Finite Elements Method (FEM)for Electromagnetics
  + Model Order Reduction (MOR) in Electromagnetics
  + Near Field to Far Field Transformation (N2F)

### Simulations

* + FEKO simulations
  + FEM MOR projects
    - 1 Vivaldi (based on *TaperedSlotAntennaArrayNoPec2.hfss*) for N2F test : fields extracted from HFSS and from LTE simulations. Brief explanation of the LTE solver call procedure
    - 3 x 5 patch antennas – 1st order basis (notice HFSS did not converge for ∆S = 0.05, but this actually does not influence the MOR results)
    - 3 x 5 patch antennas – 2nd order basis
    - 40 Vivaldi antennas from *TaperedSlotAntennaArray\*.hfss* – *lte\_fileset* cannot be extracted from the available HFSS model with the current *ANST\_MeshReader* version. Old *lte\_fileset* have been used
  + Matlab N2F
    - Interpolation tests :
      * Empirical interpolation of a generic parameter dependent function and of the scalar N2F operator
      * Polynomial basis construction and DFT truncation test
    - matlabLib : Library for both scalar and vector N2F
    - scalar field approximation tests for box and sphere
    - vector fields for box and sphere with FEKO results comparisons
  + Plots made during the internship @ LTE

### Thesis LaTeX files

* + Defense (includes timings results that I couldn’t add to the thesis for deadline reasons)

For any further information: **ntilau@gmail.com**
