# PULSEGUARD-Reliable-Contactless-Physiological-Monitoring-from-Facial-Video
An independent research extension of contactless physiological monitoring — investigating whether a system that estimates heart rate from facial video can become more trustworthy by explicitly modeling signal quality, spectral uncertainty, failure causes, and the possibility of abstaining when evidence is insufficient for reliable measurement.

*This project is inspired by and designed as an explicit research extension of Prof. Prasanta Kumar Ghosh's work on robust real-time pulse-rate estimation from facial video using sparse spectral peak tracking at IISc Bangalore.
Prof. Ghosh's approach addresses the question of how to accurately estimate pulse rate from noisy rPPG signals using spectral analysis. PULSEGUARD extends this to ask a different and complementary question: given such an estimation system, when should the system trust its own estimate, what caused an unreliable measurement, and should the system withhold a prediction rather than produce an unreliable one?
PULSEGUARD does not reproduce Prof. Ghosh's system. It explicitly credits the original sparse spectral peak tracking contribution and builds the reliability layer independently on top of that direction.*

## **Central Research Question**
Can a contactless physiological monitoring system become more trustworthy by explicitly modeling signal quality, spectral uncertainty, failure causes, and the possibility of abstaining when the available evidence is insufficient for reliable measurement?

## **The Five Independent Contributions**
**Contribution 1** — Physiological Signal Quality Index (PSQI). A seven-component composite index quantifying the reliability of each HR measurement. Components include spectral quality, peak coherence across methods, motion quality, illumination quality, ROI stability, temporal consistency, and harmonic coherence. The PSQI is empirically validated to correlate with HR estimation error and is used as the primary reliability signal throughout the system.

**Contribution 2** — Uncertainty-Aware HR Estimation. Rather than reporting a single HR number, PULSEGUARD reports HR ± CI_half where the confidence interval width is derived from spectral peak width, cross-method disagreement, and PSQI calibration. The empirical coverage of these intervals is measured and reported.

**Contribution 3** — Failure-Cause Diagnosis Engine. Nine specific failure codes (F1 through F9) are detected at each measurement, covering excessive motion, illumination inadequacy, ROI failure, low SNR, spectral ambiguity, insufficient duration, cross-method disagreement, no dominant peak, and signal saturation. When a measurement is unreliable, the system explains specifically why.

**Contribution 4** — Selective Prediction and Abstention. A reliability score combining PSQI, uncertainty interval, and failure count determines whether to REPORT, CAUTION, or ABSTAIN for each measurement. When abstaining, the system provides specific guidance on which conditions to improve before re-recording.

**Contribution 5** — Always-Predict vs Abstain Experiment. The core scientific contribution: comparing three strategies — always-predict, confidence-weighted, and selective abstain — on the same dataset. The risk-coverage curve proves that rejecting the lowest-reliability measurements reduces MAE on the accepted subset, validating the abstention framework.

## **Architecture**
```text
Facial Video / Webcam Input
          │
          ▼
ROI Detection and Tracking
  Forehead + Cheeks + Motion Assessment
  ROI Quality Estimation
          │
          ▼
Multi-Method rPPG Extraction (6 methods)
  Green │ CHROM │ POS │ PCA │ ICA │ LGI
          │
          ▼
Adaptive Quality-Weighted Fusion
  Per-method spectral quality scoring
  Weighted ensemble PSD
          │
          ▼
Sparse Spectral Peak Tracking (Inspired by Prof. Ghosh)
  Multi-candidate peak evaluation
  Harmonic coherence validation
  Temporal continuity constraint
          │
          ▼
RELIABILITY LAYER (Independent Contributions)
  ├── Physiological Signal Quality Index (PSQI)
  ├── Uncertainty-Aware HR Estimation (CI)
  ├── Failure-Cause Diagnosis (F1-F9)
  └── Selective Prediction Engine
          │
          ▼
  REPORT / CAUTION / ABSTAIN
  HR_est ± CI_half
  Failure explanation
```

## **Cell-by-Cell Summary**
The notebook contains twenty-six cells. Cell 3 generates a synthetic physiological video dataset across nine challenge conditions — normal, head movement, talking, illumination change, low light, partial occlusion, video compression, reduced resolution, and frame rate variation — using a physically motivated BVP signal model including harmonics, HRV, motion artefacts, and illumination variation.

Cell 5 implements all six rPPG extraction methods. Cell 6 implements sparse spectral peak tracking with multi-candidate evaluation, harmonic coherence validation, prominence scoring, spectral concentration metrics, and temporal continuity. Cell 7 implements adaptive quality-weighted fusion across all six methods.

Cell 8 implements the PSQI. Cell 9 computes uncertainty-aware HR estimates with calibrated confidence intervals. Cell 10 implements the failure-cause diagnosis engine. Cell 11 implements the selective prediction framework.

Cell 12 evaluates HR errors with Bland-Altman analysis. Cell 13 analyses uncertainty calibration — whether stated intervals actually contain the ground truth at the stated rate. Cell 14 is the key experiment comparing always-predict versus abstain strategies with a risk-coverage curve.

Cells 15 through 23 provide PSQI component analysis, robustness testing, failure detection ROC analysis, statistical testing, multi-method comparison, spectral visualisation, the master summary figure, a single-patient reliability dashboard, and ablation study.

## **Key Results**
PULSEGUARD demonstrates that the reliability layer provides meaningful improvements in trustworthiness. The PSQI correlates negatively with HR estimation error — samples with higher PSQI are estimated more accurately. Failure diagnosis correctly identifies unreliable measurements with AUC significantly above random chance. The confidence intervals achieve empirical coverage close to the target rate. The risk-coverage curve confirms that selectively abstaining on low-reliability measurements reduces MAE on the accepted subset — validating the scientific hypothesis.

## **Honest Limitations
PULSEGUARD is a resea**rch prototype validated on simulated data. Real deployment requires validation on real facial video from actual cameras. The BVP signal model, while physically motivated, does not capture all real-world rPPG complexities. Face detection and landmark tracking are simulated — real deployment requires Mediapipe Face Mesh or equivalent. The PSQI thresholds require tuning on real clinical datasets. The failure detection AUC would need to be validated against ground truth failure labels from human annotators.

## **About the Author**
Nifla Nalakath |
BTech in Computer Science and Engineering |
APJ Abdul Kalam Technological University, Kerala, India |
niflanalakath@gmail.com
