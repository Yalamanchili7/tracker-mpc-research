# Solar Tracker Optimization Research

**Can machine learning beat geometric backtracking for single-axis solar trackers?**

This research investigates whether Reinforcement Learning (RL) or Model Predictive Control (MPC) can outperform the industry-standard geometric backtracking algorithm.

## 🎯 Key Findings

| Method | vs Backtracking | Best Conditions |
|--------|-----------------|-----------------|
| RL (SAC/PPO) | **-2.0%** | - |
| RL + Forecast | **-2.9%** | - |
| MPC + Perfect Forecast | **+0.01%** | Clear days |
| MPC + Perfect Forecast | **+3.7% avg, +7.6% max** | ☁️ Cloudy days |

### Bottom Line

- **Clear days**: Geometric backtracking is already optimal - sun position fully determines the best angle
- **Cloudy days**: MPC with forecasting achieves **3-7% gains** by anticipating irradiance changes
- **RL cannot beat backtracking** without future information

This validates commercial products like **Nextracker TrueCapture** and **Array Technologies SmarTrack** which use forecasting/sensing for optimization.

## 📓 Notebooks

| # | Notebook | Description |
|---|----------|-------------|
| 01 | [Environment Setup](notebooks/01_environment_setup.ipynb) | PVLib physics, backtracking validation |
| 02 | [Gym Environment](notebooks/02_gym_environment.ipynb) | Gymnasium RL environment |
| 03 | [RL Training](notebooks/03_rl_training.ipynb) | SAC & PPO training |
| 04 | [Advanced Training](notebooks/04_advanced_training.ipynb) | Enhanced features, 500K steps |
| 05 | [Residual Learning](notebooks/05_residual_learning.ipynb) | Learn corrections to backtracking |
| 06 | [Real Weather](notebooks/06_real_weather_validation.ipynb) | NSRDB data validation |
| 07 | [Multi-Year Training](notebooks/07_multiyear_training.ipynb) | 7 years of data (2018-2024) |
| 08 | [**MPC Forecasting**](notebooks/08_mpc_forecasting.ipynb) | ⭐ Key result: MPC beats backtracking |
| 09 | [Multi-Site](notebooks/09_multisite_comparison.ipynb) | Geographic comparison |

**Start with Notebook 08** for the main results.

## 📊 Data

- **Source**: 7 years of 10-minute solar irradiance data (2018-2024)
- **Location**: Phoenix, AZ (33.45°N, -111.95°W)  
- **Records**: 368,000+ timesteps
- **Variables**: GHI, DNI, DHI, temperature, wind speed

Data not included in repo. Download from [NREL NSRDB](https://nsrdb.nrel.gov/) or use your own irradiance data.

## 🔧 Setup

```bash
# Clone repo
git clone https://github.com/yalamanchili7/tracker-mpc-research.git
cd tracker-mpc-research

# Create environment
python -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook notebooks/
```

## 📦 Requirements

- Python 3.10+
- pvlib >= 0.10.0
- stable-baselines3 >= 2.1.0
- gymnasium >= 0.29.0
- pytorch >= 2.0.0
- pandas, numpy, matplotlib

## 🔬 Methodology

### Physics Model (PVLib)
- Single-axis horizontal tracker
- Perez diffuse irradiance model
- Inter-row shading via `shaded_fraction1d`
- Temperature-dependent efficiency

### Algorithms Tested

1. **Geometric Backtracking** - Industry standard, avoids row-to-row shading
2. **RL (SAC/PPO)** - Learns from current state only
3. **Residual RL** - Learns corrections to backtracking
4. **MPC** - Optimizes over 6-step forecast horizon

### Why RL Fails
- Backtracking is mathematically optimal given current sun position
- RL only sees current state, cannot anticipate future conditions
- No exploitable patterns in clear-sky conditions

### Why MPC Succeeds on Cloudy Days
- Uses forecast to anticipate irradiance changes
- Can pre-position tracker before clouds arrive
- Reduces unnecessary movement during transients

## 📈 Results Summary

```
Weather Type    | MPC Improvement | Significance
----------------|-----------------|------------------
Clear           | +0.01%          | Backtracking optimal
Variable        | +0.02%          | Minimal opportunity  
Cloudy          | +3.66% avg      | Forecasting helps!
Best single day | +7.63%          | High variability day
```

## 🏢 Industry Relevance

This research validates the approach used by:
- **Nextracker TrueCapture** - Uses irradiance sensing + cloud detection
- **Array Technologies SmarTrack** - ML-based diffuse optimization

The finding that **forecasting is necessary to beat backtracking** explains why these products focus on real-time sensing rather than pure ML.

## 📄 License

MIT License - see [LICENSE](LICENSE)

## 👤 Author

**Sundeep Gupta**  
AI Engineer specializing in solar development technology

---

*Research conducted to understand theoretical limits of solar tracker optimization.*
