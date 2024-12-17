# ConvmatPLL
An algorithm that calculates harmonic transfer functions of linear periodically time-variant (LPTV) systems, for example, digital phase-locked loops (DPLLs) using conversion matrices.

## Introduction
Digital PLLs play an important role in contemporary RF transceivers, as they offer technology-scalable area and an excellent noise-power figure-of-merit to support carrier aggregation in multiple bands with high-order quadrature amplitude modulation (QAM). In prior works that analyze the noise transfer functions of such systems, typically the loop is assumed to be linear time-invariant (LTI), where noise folding introduced by divider downsampling is ignored. This LTI assumption starts to generate large prediction errors when designers try to increase the loop bandwidth to more aggressively suppress the phase noise (PN) generated by ring oscillators. LPTV analysis, on the other hand, accurately models noise folding. However, long mathematical derivations and careful assumptions have to be made to get closed-form expressions, which heightens the barrier for designers to adapt their own designs. Moreover, the cross-correlated frequency-shifted spectrum of cyclostationary noise sources is ignored, leading to inaccurate results in most cases.

This work provides a unifying method for calculating the harmonic transfer functions using conversion matrices. It offers the following benefits:
  - All time-varying components are represented as matrix multiplications, therefore the final system equations become LTI-like loop gain and path gain. The user only need to know how to draw block diagrams and find loop gain and path gain using Mason's gain formula. 
  - The cross-correlated frequency-shifted spectrum of cyclostationary noise sources is taken care by the proposed uncorrelated upsampling. It also improves computational efficiency by reducing problem dimension, enabling users to quickly explore the design space.

## File Explanation
- `convmat_fpec.m`: The main script of the proposed algorithm. It calls a Simulink model `FPEC_sim.slx` that simulates the output
    noise of a PLL with [fast phase error correction (FPEC)](https://ieeexplore.ieee.org/document/8737704). The results are then compared with the proposed algorithm. You can change the model parameters, turn on or off noise sources, and inspect fundamental/aliased noise, etc. 

- `FPEC_sim.slx`: The Simulink model that implements the DPLL with FPEC.

- `syllaios2011.m`: The script that implements the LPTV analysis of DSM noise transfer function from [Ioannis L. Syllaios, ISCAS 2011](https://ieeexplore.ieee.org/document/5937524). Before using this script, run `convmat_fpec.m` to set up variables in the workspace. Remember to turn off the noise from the reference and DCO by setting `noise_ref_en` and `noise_dco_en` to 0 in `convmat_fpec.m`. 

- `MDLL`: Folder that contains the implementation of a [multiplying delay-locked loop (MDLL)](https://ieeexplore.ieee.org/document/8730473).

  - `convmat_mdll.m`: The main script. Comparing to `convmat_fpec.m`, the only change is the expression of loop gain and path gain in the section `Transfer Functions`; some other unused variables are deleted as well. Adapting to different designs is easy, no need to derive the equations all over again.
  - `MDLL_sim.slx`: The Simulink model of the MDLL. 

## Requirement
MATLAB R2023b and above
- Simulink
- Signal Processing Toolbox

## Citation
If you used this code and find it helpful to your research, please consider citing our paper:

```
@ARTICLE{lu2024convmatpll,
  author={Lu, Hongyu and Mercier, Patrick P.},
  journal={IEEE Transactions on Circuits and Systems I: Regular Papers}, 
  title={Linear Periodically Time-Variant Digital PLL Phase Noise Modeling Using Conversion Matrices and Uncorrelated Upsampling}, 
  year={2024},
  volume={71},
  number={12},
  pages={6021-6033},
  keywords={Phase locked loops;Linear systems;Noise;Computational modeling;Time-frequency analysis;Phase noise;Harmonic analysis;Conversion matrix;harmonic transfer matrix (HTM);phase-locked loop (PLL);phase noise;linear periodically time-variant (LPTV);noise folding},
  doi={10.1109/TCSI.2024.3415001}}
```

```
H. Lu and P. P. Mercier, "Linear Periodically Time-Variant Digital PLL Phase Noise Modeling Using Conversion Matrices and Uncorrelated Upsampling," in IEEE Transactions on Circuits and Systems I: Regular Papers, vol. 71, no. 12, pp. 6021-6033, Dec. 2024, doi: 10.1109/TCSI.2024.3415001.
```
