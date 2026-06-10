# PlanWise 
### Intelligent Software Project Planning via Hybrid AI Pipeline

> **Co-First Author Publication:** This system is the implementation behind *"Intelligent Project Management: Hybrid AI-Driven Requirement Definition and Resource Allocation"*, accepted for publication in **Results in Engineering, Elsevier (2026)**.

---

## Overview

PlanWise is an end-to-end AI pipeline that converts raw business requirements into fully assigned, dependency-aware project execution plans - automatically, in under 14 seconds.

Given a set of business requirements, the system produces:
- Structured system requirements (via fine-tuned Flan-T5)
- Subtask dependency graph (via DNN classifier)
- Criticality scores for each subtask (via GAT2)
- Optimal employee-task assignments (via GAT1 + Priority-Based Sequential Assignment)

**96% end-to-end planning accuracy · 200 tasks per project · <14s execution time**

---

## Pipeline Architecture

```
Business Requirements (BR)
        │
        ▼
┌─────────────────────┐
│  Model 1: Flan-T5   │  BR → Structured System Requirements (SR)
│  (Transformer)      │  + Sentence embeddings via all-MiniLM-L6-v2
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Model 2: DNN       │  Subtask pair classification
│  Classifier         │  (FS / SS / FF / SF / NO_RELATION)
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Model 3: DNN       │  Employee experience-level classification
│  (Experience)       │  PyTorch · 16-dim embeddings
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Model 4: GAT2      │  Criticality score prediction per subtask
│  (TensorFlow GAT)   │  MAE=0.065 · RMSE=0.085 · R²=0.877
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Model 5: GAT1      │  Optimal employee-task assignment
│  (PyTorch GAT +     │  AHP weighting + Priority-Based Sequential
│   PBSA)             │  Assignment · 200 complete assignments
└─────────────────────┘
        │
        ▼
  Django Dashboard
  (Real-time project visualization & modification)
```

---

## Repository Structure

```
PlanWise/
├── README.md
├── requirements.txt
├── models/
│   ├── model1_flan_t5/          # Flan-T5 fine-tuning & SR generation
│   ├── model2_classifier/       # Subtask relationship DNN classifier
│   ├── model3_experience_dnn/   # Employee experience-level DNN
│   ├── model4_gat2_criticality/ # GAT2 criticality score prediction
│   └── model5_gat1_assignment/  # GAT1 + PBSA employee assignment
└── django_app/                  # Production web interface
```

---

## Models

| # | Module | Architecture | Framework | Key Metric |
|---|--------|-------------|-----------|------------|
| 1 | Requirement Generation | Flan-T5 (fine-tuned) | HuggingFace Transformers | BR → SR structured JSON |
| 2 | Subtask Classifier | DNN + TF-IDF (512→256→128) | TensorFlow/Keras | Multi-class classification |
| 3 | Experience DNN | Feedforward DNN (→32→16) | PyTorch | Employee skill embedding |
| 4 | GAT2 Criticality | 2-layer GAT, 6 node features | TensorFlow | R²=0.877, MAE=0.065 |
| 5 | GAT1 Assignment | GAT + AHP + PBSA | PyTorch Geometric | 200 optimal assignments |

---

## Key Technical Decisions

- **GAT2 ground truth:** Normalized longest DAG path via Simple DFS cycle-breaking
- **GAT1 cost matrix:** Min-shift applied post-infinity-replacement (scientifically correct for z-scored values)
- **GAT1 assignment:** Priority-Based Sequential Assignment — subtasks sorted by criticality score, each assigned to the best available employee
- **AHP weights:** β₁≈0.637 (duration), β₂≈0.258 (defects), β₃≈0.105 (experience), CR=0.039

---

## Requirements

```
torch
torch_geometric
tensorflow
transformers==4.56.2
accelerate==0.34.2
datasets==2.21.0
sentence-transformers
scikit-learn
pandas
numpy
matplotlib
networkx
django
joblib
openpyxl
scipy
```

---

## Publication

**"Intelligent Project Management: Hybrid AI-Driven Requirement Definition and Resource Allocation"**  
Nour ElSamra*, Nour Ahmed* · Dr. Yehia Kotb · Nourah AlSehali · Njood AlMutairi · Muneerah Alotaibi . Mohamed Kotb · Moutaz Haddara  
*Co-first authors*  
Accepted for publication — *Results in Engineering*, Elsevier (2026)

---

## Author

**Nour ElSamra**  
B.Sc. Computer Engineering, American University of the Middle East  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-nourelsamra-blue)](https://linkedin.com/in/nourelsamra-3689382a9)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0005--8026--2805-green)](https://orcid.org/0009-0005-8026-2805)
