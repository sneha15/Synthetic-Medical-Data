# 🏥 Hospital Patient Flow Simulation at IMH

This project simulates the flow of patients through various hospital stations at the Institute of Mental Health (IMH) over the course of a single working day. The simulation generates realistic synthetic data representing patient demographics, arrival patterns, queue modeling, patient assignments, diagnostic testing, and the sequence of interactions with hospital services.

## 📊 Interactive Notebook

Click the button below to open and run the full analysis in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](
https://colab.research.google.com/drive/1oYvh9tKfmZeA77SffkFiDPMnGTsivl5k?usp=sharing)

## 📁 Repository Structure

- `clinic_data_simulation.ipynb`: Main analysis notebook (to view plots, click on Colab link above)
- `clinic_data_simulation.html`: Converted html file
- `patient_data_flow.csv`: Synthetic patient flow and demographic data

---

## 📅 Simulation Overview

- **Duration:** 1 working day (8:00 AM – 5:00 PM)
- **Patients Simulated:** 100
- **Time Granularity:** Minute-level tracking
- **Purpose:** Synthetic generation of hospital flow data for analysis and visualization

---

## 🧍 Patient Demographics & Characteristics

- **Age Range:** 12 to 90 years
- **Gender Distribution:** 50% Male / 50% Female
- **Insurance Providers (Random Assignment):**
  - AIA HealthShield
  - Prudential
  - Great Eastern
  - NTUC Income
  - HSBC

### 🧠 Health Conditions (Based on Estimated Prevalence)

| Condition                        | Probability |
|----------------------------------|-------------|
| Depression                       | 25%         |
| Anxiety Disorders (GAD)          | 20%         |
| Obsessive-Compulsive Disorder    | 12%         |
| Schizophrenia                    | 10%         |
| Bipolar Disorder                 | 8%          |
| Alcohol Use Disorders            | 7%          |
| Dementia                         | 6%          |
| PTSD                             | 5%          |
| Personality Disorders            | 4%          |
| ADHD                             | 3%          |

---

## 🔄 Patient Flow Through the Clinic

Patients proceed through the following stages:

1. **Arrival** (via non-homogeneous Poisson process)
2. **Check-In Desk**
3. **Triage Station A or B**
4. **Doctor Consultation (Doctors A–E, Psychiatry)**
5. **Diagnostic Test (Blood Test, Neuroimaging, EEG, or None)**
6. **Pharmacy**
7. **Billing Counter**

---

## 🖥️ Simulation Logic by Station

### 🧾 Check-In Desk
- **1 Desk Only**
- **Check-In Duration:** 1–3 mins (Exponential distribution, mean = 2)
- **Processing:** FIFO if queue forms

### 🩺 Triage Station
- **Stations:** A & B
- **Triage Classification:** 20% Emergency, 80% Routine
- **Duration:** 3–5 mins (Uniform distribution)
- **Queue Logic:** Emergency prioritized, otherwise FIFO

### 👨‍⚕️ Doctor Consultation
- **Doctors Available:** A–E (Psychiatrists)
- **Duration:** 15–20 mins (Normal distribution, mean = 17.5, SD = 1.5)
- **Logic:**
  - Emergency queue prioritized
  - Within each category, FIFO by triage end time

### 🧪 Diagnostic Test Assignment
- **Assignment Probabilities:**
  - Blood Test: 20%
  - Neuroimaging: 10%
  - EEG: 10%
  - None: 60%

### 🧪 Diagnostic Test Simulation
- **Stations Per Test:**
  - Blood: B1, B2
  - Neuroimaging: N1, N2
  - EEG: E1, E2
- **Durations & Result Delays:**

| Test Type     | Duration (mins) | Result Days (Uniform) |
|---------------|------------------|------------------------|
| Blood Test    | 10–15 (mean=12.5) | 1–3                    |
| Neuroimaging  | 30–60 (mean=45)   | 2–4                    |
| EEG           | 45–60 (mean=52.5) | 7–14                   |

- **Queueing:** Emergency prioritized, FIFO within category

### 💊 Pharmacy
- **Pharmacists:** P1, P2, P3
- **Steps:**
  - Medication Finding: 5–7 mins (Normal, mean=6, SD=0.5)
  - Explanation Time: 3–5 mins (Normal, mean=4, SD=0.5)
- **Logic:** FIFO, based on drop-off time (latest of consultation or test end)

### 💳 Billing
- **Billing Stations:** B1, B2
- **Billing Duration:** 4–7 mins (Normal, mean=5.5, SD=0.5)
- **Queue:** FIFO by prescription collection time

---

## 📤 Output CSV: Column Descriptions

| Column Name                   | Description |
|------------------------------|-------------|
| `PatientID`                  | Unique patient identifier |
| `Age`, `Gender`              | Demographics |
| `HealthCondition`            | Assigned mental health condition |
| `Insurance`                  | Assigned insurance provider |
| `ArrivalTime`                | Time of arrival at the clinic |
| `CheckInStart`, `CheckInEnd`| Check-in timestamps |
| `CheckInDurationMin`        | Time spent at check-in desk |
| `CheckInWaitMin`            | Time waited before check-in |
| `PatientsWaitingCheckIn`    | Queue size upon arrival |
| `TriageIn`, `TriageOut`     | Triage timestamps |
| `TriageDurationMin`         | Duration of triage |
| `TriageWaitMin`             | Wait before triage |
| `TriageCategory`            | Emergency / Routine classification |
| `TriageStation`             | Station A or B |
| `PatientsWaitingTriage`     | Queue size at triage |
| `ConsultationIn`, `ConsultationOut` | Doctor consult timestamps |
| `TotalConsultationTimeMin`  | Duration of consultation |
| `ConsultationWaitMin`       | Wait before seeing doctor |
| `Doctor`, `Specialty`       | Assigned doctor and their specialty |
| `RoutineQueueSize`, `EmergencyQueueSize` | Queue size before consultation |
| `DiagnosticTests`           | Assigned test (Blood, Neuroimaging, EEG, or None) |
| `TestIn`, `TestOut`         | Diagnostic test timestamps |
| `TestDurationMin`           | Duration of test |
| `TestWaitMin`               | Wait before test |
| `TestStation`               | Station performing test |
| `TestResultDays`            | Days until results are ready |
| `PrescriptionDropoff`       | Time prescription sent to pharmacy |
| `PharmacistIn`, `PharmacistOut` | Pharmacist start and end timestamps |
| `PrescriptionFindTime`      | Time to find medication |
| `PrescriptionCollectionTime`| Time patient receives medication |
| `ExplanationTimeMin`        | Time pharmacist explains usage |
| `PharmacyWaitMin`           | Time spent waiting in pharmacy |
| `PharmacistAssigned`        | Pharmacist ID (P1, P2, P3) |
| `MedicationAvailability`    | 90% Available, 10% Unavailable |
| `PharmacyQueueSize`         | Pharmacy queue size on arrival |
| `BillingIn`, `BillingOut`   | Billing timestamps |
| `BillingDurationMin`        | Duration of billing process |
| `BillingWaitMin`            | Wait time at billing |
| `BillingStation`            | Billing counter assigned (B1, B2) |
| `BillingQueueSize`          | Number of patients ahead in billing queue |

---

## 📈 Key Visualizations

- **Pie Charts**: Gender, insurance, triage categories, medication access
- **Histograms**: Age distribution, total waiting time, total clinic time
- **Bar Charts**: Patients per disorder, patient arrivals, emergency vs routine wait times, patients seen per doctor, diagnostic test orders, average queue length
- **Sankey Diagram**: Flow of patients through clinic stages
- **Gantt Chart**: Patient timelines through the clinic
- **Time Series**: Consultation queue sizes over time, pharmacy queue sizes over time, wait time progression
- **Box Plots**: Processing time per station, variation in wait durations by station, result days by diagnostic test

20 Plotly-powered charts offer interactive views of key metrics.

---

## 📦 Example Use Cases

- Visualizing and analyzing bottlenecks in patient flow
- Testing queueing models in clinical environments
- Understanding time-based load across departments
- Generating synthetic datasets for ML training and dashboards

---

## 🛠️ Tech Stack

- **Language:** Python
- **Distributions:** NumPy
- **Data Handling:** Pandas
- **Visualization:** Plotly

---

## 🔗 Additional Links

- 🔍 **Notebook (GitHub)**: [clinic_data_simulation.ipynb](https://github.com/sneha15/Synthetic-Medical-Data/blob/main/clinic_data_simulation.ipynb)
- 🌐 **CSV File (Github)**: [View on nbviewer](https://github.com/sneha15/Synthetic-Medical-Data/blob/main/patient_flow_data.csv)
- 📄 **HTML Export (GitHub Pages)**: [View on GitHub Pages](https://sneha15.github.io/Synthetic-Medical-Data/clinic_data_simulation.html)

---

## 📬 Contact

For questions or collaboration inquiries, please open an [issue on GitHub](https://github.com/sneha15/Synthetic-Medical-Data/issues).

---

## ✅ License

This project is provided for research and educational purposes.

---

