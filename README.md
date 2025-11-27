# CXL-MemSim-Compute-Express-Link-CXL-Memory-Simulation-and-Visualization-Toolkit

Overview
**
CXL-MemSim is a lightweight simulation and visualization toolkit designed to model, compare, and analyze DDR5 vs CXL-Type3 memory performance.
It generates synthetic telemetry, computes latency and bandwidth metrics, and produces publication-quality plots for architectural exploration.
This project is part of my ongoing study of emerging memory technologies, cloud-scale system architecture, and hardware/software co-design.
Features
Synthetic telemetry dataset generation for DDR5 and CXL-Type3
Latency curve visualization
Bandwidth curve computation and visualization
Organized project structure with reproducible outputs
Built using Python, pandas, numpy, seaborn, and matplotlib
Easily extendable to real hardware telemetry or PMU counter logs
Memory Model
The simulator models:
DDR5 latency behavior vs access size
CXL-Type3 latency overhead
Bandwidth calculation:
Bandwidth (MB/s)
=**
Access Size (KB)
Latency (ns)
×
10
6
Bandwidth (MB/s)= 
Latency (ns)
Access Size (KB)
​	
 ×10 
 6
 
This framework can incorporate:
Real DRAM/CXL timing
Cloud computing telemetry
Vendor-specific latency tables

Project Structure
CXL-MemSim/
│
├── src/
│   ├── simulator.py
│   └── plotter.py
│
├── data/
│   └── telemetry_samples.csv
│
├── results/
│   ├── latency_curve.png
│   └── bandwidth_curve.png
│
├── venv/  (optional)
│
└── README.md

#Installation
1. Create a virtual environment
python3 -m venv venv
source venv/bin/activate
2. Install dependencies
pip install pandas numpy seaborn matplotlib
Usage
1. Generate telemetry
python src/simulator.py
2. Generate plots
python src/plotter.py

#Output
Latency Curve
Compares DDR5 vs CXL-Type3 latency across access sizes.
Saved as: results/latency_curve.png
Bandwidth Curve
Bandwidth derived from telemetry latency values.
Saved as: results/bandwidth_curve.png

#Example Code (plotter.py)
import os
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

BASE_DIR = "/Users/bossx918spyder/Documents/CXL Research/CXL-MemSim"
DATA_FILE = os.path.join(BASE_DIR, "data", "telemetry_samples.csv")
RESULTS_DIR = os.path.join(BASE_DIR, "results")
os.makedirs(RESULTS_DIR, exist_ok=True)

def plot_latency_and_bandwidth():
    df = pd.read_csv(DATA_FILE)
    print("Columns in CSV:", df.columns)

    # LATENCY PLOT
    plt.figure(figsize=(8, 6))
    sns.lineplot(data=df, x="size_kb", y="DDR5_latency_ns", label="DDR5")
    sns.lineplot(data=df, x="size_kb", y="CXL_latency_ns", label="CXL-Type3")
    plt.xlabel("Access Size (KB)")
    plt.ylabel("Latency (ns)")
    plt.title("DDR5 vs CXL Memory Latency Curve")
    plt.grid(True)
    plt.savefig(os.path.join(RESULTS_DIR, "latency_curve.png"), dpi=600)
    plt.show()

    # BANDWIDTH PLOT
    df["DDR5_bandwidth_MBps"] = df["size_kb"] / df["DDR5_latency_ns"] * 1e6
    df["CXL_bandwidth_MBps"] = df["size_kb"] / df["CXL_latency_ns"] * 1e6

    plt.figure(figsize=(8, 6))
    sns.lineplot(data=df, x="size_kb", y="DDR5_bandwidth_MBps", label="DDR5")
    sns.lineplot(data=df, x="size_kb", y="CXL_bandwidth_MBps", label="CXL-Type3")
    plt.xlabel("Access Size (KB)")
    plt.ylabel("Bandwidth (MB/s)")
    plt.title("DDR5 vs CXL Memory Bandwidth Curve")
    plt.grid(True)
    plt.savefig(os.path.join(RESULTS_DIR, "bandwidth_curve.png"), dpi=600)
    plt.show()

if __name__ == "__main__":
    plot_latency_and_bandwidth()

#Future Work
Incorporate real PMU-based telemetry data
Add CXL memory pooling and switching models
Simulate multi-socket NUMA topologies
Integrate with HPC and AI workload traces
Add support for hardware performance monitoring counters
