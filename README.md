# FLARE: A Resource-Aware Distributed Framework for Detecting and Mitigating Multi-Vector TCP Flag and Fragmentation Attacks in Software-Defined IoT Networks

**FLARE** is a modular, high-performance framework for detecting, classifying, and mitigating TCP flag and IP fragmentation attacks in SDN-enabled IoMT/IoT networks. It integrates programmable data planes (P4) with multi-controller SDN architectures and an adaptive ML-based ensemble classifier for real-time response and mitigation.

---

## 📌 **Key Modules**

### 1️⃣ P4-SFFP (Stateful Flag & Fragment Processing)
- Written in P4_16 for BMv2 and hardware targets.
- Parses and inspects TCP flags (RST, FIN, SYN) and IP fragmentation fields.
- Tracks stateful counters to detect suspicious patterns.

### 2️⃣ LSMA (Logically Synchronized Multi-Controller Architecture)
- Synchronizes security alerts across multiple SDN controllers.
- Supports both Java-based controllers (Beacon, Floodlight, OpenDaylight) and Python-based controllers (Ryu, POX).
- Provides urgent and routine pathways for policy consistency.

### 3️⃣ AFAC (Adaptive Flag Attack Classifier)
- Ensemble ML classifier combining KNN, Decision Tree, Random Forest, SVM, XGBoost with a Neural Network meta-learner.
- Classifies attack likelihood based on SFFP stateful features.
- Generates actionable alerts with confidence scores.

### 4️⃣ MLDFM (Multi-Layer Distributed Flag Mitigation)
- Evaluates incoming alerts against dynamic policy rules.
- Triggers mitigation actions: rate-limit flags, drop malicious fragments, or flush switch state.
- Uses P4Runtime to update P4 switches in real time.

---

## 📂 **Repository Structure**

```bash
flare-framework/
├── README.md
├── LICENSE
├── p4src/
│   └── flare_sffp.p4
├── configs/
│   ├── p4info.txt
│   ├── bmv2.json
│   ├── controller_config.yaml
│   ├── afac_config.yaml
│   ├── mldfm_policy.yaml
├── runtime/
│   ├── p4runtime_controller.py
│   ├── flow_rules.json
│   ├── flare_alert.json
│   ├── lsma_sync.py
├── controllers/
│   ├── lsma_controller_java/
│   │   ├── LSMAController.java
│   │   └── FlowPusher.java
│   └── lsma_controller_python/
│       └── lsma_controller.py
├── afac/
│   ├── ensemble_trainer.py
│   ├── online_classifier.py
│   ├── models/
│   └── notebooks/
│       └── train_afac.ipynb
├── mldfm/
│   ├── mitigation_policy_engine.py
│   ├── actions/
│   │   ├── rate_limit.py
│   │   ├── drop_frag.py
│   │   └── flush_state.py
└── Makefile
```

---

## ⚙️ **Deployment Instructions**

### ✅ **1. Build P4 Pipeline**
```bash
make build
```

### ✅ **2. Run BMv2 Switch**
```bash
simple_switch_grpc --device-id 0 -i 0@veth0 -i 1@veth1 configs/bmv2.json
```

### ✅ **3. Start P4Runtime Controller**
```bash
python3 runtime/p4runtime_controller.py
```

### ✅ **4. Train AFAC Classifier**
```bash
python3 afac/ensemble_trainer.py
```

Or use `notebooks/train_afac.ipynb` for interactive analysis.

### ✅ **5. Run Online Classifier**
```bash
python3 afac/online_classifier.py
```

### ✅ **6. Run LSMA Sync Module**
```bash
python3 runtime/lsma_sync.py
```

### ✅ **7. Start Controllers**
- **Java-based controllers:** Deploy `LSMAController.java` + `FlowPusher.java` inside Beacon, Floodlight, or OpenDaylight.
- **Python-based controllers:** Launch Ryu or POX with `lsma_controller.py`.

### ✅ **8. Activate Mitigation**
```bash
python3 mldfm/mitigation_policy_engine.py
```

---

## ✅ **Emulated vs Real-World**

- **Emulated:** Mininet + BMv2 + Ryu or Floodlight.
- **Physical:** P4 hardware switch (Barefoot Tofino, NetFPGA) + real SDN controllers cluster.

---

## 📎 **Key Config Files**

- `controller_config.yaml`: defines multi-controller topology and TLS.
- `afac_config.yaml`: ML ensemble, training, and thresholds.
- `mldfm_policy.yaml`: default flag thresholds and mitigation actions.
- `flare_alert.json`: JSON Schema for all alerts.

---

## ⚡ **Dependencies**

- Python 3.x  
- `p4c` compiler (`p4lang/p4c`)  
- BMv2 (`simple_switch_grpc`)  
- Python libs: `grpcio`, `p4runtime_lib`, `scikit-learn`, `xgboost`, `joblib`, `jsonschema`, `requests`  
- Ryu or your Java SDN controller.

---

## 📬 **Contact**

Please open an issue or email the authors via your institutional address for more help.

---

**🔐 Ready to secure your IoT world with P4 + SDN + ML — run FLARE today!**

---
