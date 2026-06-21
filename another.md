# Presidio Hardened IDS Experiment

## 1. Synthetic Traffic Generation
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python generate_traffic.py --normal 10000 --attacks 500 --attack-types portscan synflood exfiltration --seed 42 --output data/traffic.csv
Generating 10000 normal + 500 attack flows (seed=42)...
  Total flows:   10498
  Normal:        10000
  Attack:        498  (4.7%)
  Features:      10
  Saved to:      data/traffic.csv
## 2. Exploratory Data Analysis
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python eda.py --input data/traffic.csv

=== EDA: Network Traffic Dataset ===

  Rows:        10498
  Normal:      10000
  Attack:      498  (4.7%)
  Features:    duration_s, packet_count, byte_count, avg_packet_size, inter_arrival_ms, syn_flag_ratio, rst_flag_ratio, unique_dst_ports, dst_port, protocol

  Numeric summary (all features):
                           mean           std      min           max
duration_s            34.258334  5.079833e+01    0.100  5.985870e+02
packet_count         564.893408  3.774305e+03    2.000  4.880800e+04
byte_count        254786.025529  1.335712e+06  100.000  4.467570e+07
avg_packet_size      782.420147  2.478104e+02   37.500  1.510700e+03
inter_arrival_ms      65.555440  1.644447e+02    0.014  1.993570e+03
syn_flag_ratio         0.076333  1.631788e-01    0.000  9.996000e-01
rst_flag_ratio         0.027146  6.893278e-02    0.000  7.931000e-01
unique_dst_ports      10.513241  7.628347e+01    1.000  1.022000e+03
dst_port            2725.268146  5.650383e+03   22.000  6.538900e+04
protocol               7.595828  3.874128e+00    6.000  1.700000e+01

  Label distribution:
label
0    10000
1      498
## 3. Original IDS Model Training
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python train.py --input data/traffic.csv --contamination 0.05 --model-out models/ids_model.pkl
Training on 8398 flows (contamination=0.05)...
2026-06-21 21:01:28,215 [INFO] presidio_ids: SECURITY_EVENT event=detector_fit n_samples=8398 contamination=0.05
2026-06-21 21:01:28,235 [INFO] presidio_ids: SECURITY_EVENT event=detector_evaluate precision=0.9524 recall=1.0 f1=0.9756

  Test metrics (n=2100):
  Precision:    0.9524
  Recall:       1.0000
  F1:           0.9756
  Threshold:    -0.550894
2026-06-21 21:01:28,271 [INFO] presidio_ids: SECURITY_EVENT event=detector_saved path=models/ids_model.pkl

  Model saved to: models/ids_model.pkl
## 4. Real-Time Traffic Streaming
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python stream.py --rate 50 --attack-prob 0.03
Streamed 184 flows to data\stream_live.csv
## 5. Adversarial Evasion Attack on the Original Model
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python attack.py --mode evasion --model models/ids_model.pkl --attack-type portscan --n-attempts 200 --output reports/evasion_report.json
Running evasion attack (portscan, 200 attempts)...
2026-06-21 21:01:59,082 [INFO] presidio_ids: SECURITY_EVENT event=detector_loaded path=models/ids_model.pkl
2026-06-21 21:02:16,865 [INFO] presidio_ids: SECURITY_EVENT event=evasion_complete model=models/ids_model.pkl attack_type=portscan n_evaded=200 evasion_rate=1.0

  Model:          models/ids_model.pkl
  Attack type:    portscan
  Attempts:       200
  Evaded:         200
  Evasion rate:   100.0%

  Report saved to: reports/evasion_report.json
## 6. Adversarial Model Hardening
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python train.py --input data/traffic.csv --adversarial reports/evasion_report.json --contamination 0.05 --model-out models/ids_model_hardened.pkl
  Added 200 adversarial flows for hardening.
Training on 8558 flows (contamination=0.05)...
2026-06-21 21:02:22,146 [INFO] presidio_ids: SECURITY_EVENT event=detector_fit n_samples=8558 contamination=0.05
2026-06-21 21:02:22,167 [INFO] presidio_ids: SECURITY_EVENT event=detector_evaluate precision=0.987 recall=0.7855 f1=0.8748

  Test metrics (n=2140):
  Precision:    0.9870
  Recall:       0.7855
  F1:           0.8748
  Threshold:    -0.548647
2026-06-21 21:02:22,202 [INFO] presidio_ids: SECURITY_EVENT event=detector_saved path=models/ids_model_hardened.pkl

  Model saved to: models/ids_model_hardened.pkl
## 7. Adversarial Evasion Attack on the Hardened Model
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python attack.py --mode evasion --model models/ids_model_hardened.pkl --attack-type portscan --n-attempts 200 --output reports/evasion_hardened.json
Running evasion attack (portscan, 200 attempts)...
2026-06-21 21:02:50,903 [INFO] presidio_ids: SECURITY_EVENT event=detector_loaded path=models/ids_model_hardened.pkl
2026-06-21 21:03:10,695 [INFO] presidio_ids: SECURITY_EVENT event=evasion_complete model=models/ids_model_hardened.pkl attack_type=portscan n_evaded=200 evasion_rate=1.0

  Model:          models/ids_model_hardened.pkl
  Attack type:    portscan
  Attempts:       200
  Evaded:         200
  Evasion rate:   100.0%

  Report saved to: reports/evasion_hardened.json

  [!] Some attack flows evaded detection. Retrain with --adversarial to harden.
## 8. Original and Hardened Model Comparison
(.venv) PS C:\Users\ROG ZEYPHRUS\Desktop\presidio-hardened-fastapi\test\presidio-hardened-ids> python report.py --compare ids_model ids_model_hardened

========================================
  Comparison: ids_model  vs  ids_model_hardened
========================================

  Metric                       ids_model ids_model_hardened
  -----------------------------------------------------
  Evasion rate                    100.0%         100.0%
  Delta                            +0.0%

  Result: No improvement — consider more adversarial examples or retraining.
## 9. Results Summary
different to the expected outcome, the hardened model’s evasion rate did not decrease. Both the original and hardened models recorded a 100% evasion rate, producing a difference of 0%. Therefore, adversarial retraining did not improve robustness in this execution.
## 10. Conclusion
Adversarial testing is not optional when evaluating machine-learning intrusion detection systems. Although the original detector achieved strong precision, recall and F1 results on the standard test dataset, it remained vulnerable to adversarial inputs. Detectors must therefore be evaluated using deliberately manipulated attack traffic rather than relying only on conventional performance metrics.
