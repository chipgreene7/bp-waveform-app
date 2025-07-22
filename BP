import streamlit as st
import numpy as np
import matplotlib.pyplot as plt

# Set page configuration
st.set_page_config(page_title="Arterial Blood Pressure Waveform Simulator", layout="wide")

# Title
st.title("🩺 Arterial Blood Pressure Waveform Simulator")

# Sidebar inputs
st.sidebar.header("Input Blood Pressure")
systolic = st.sidebar.slider("Systolic Pressure (mmHg)", min_value=80, max_value=200, value=120)
diastolic = st.sidebar.slider("Diastolic Pressure (mmHg)", min_value=40, max_value=120, value=80)
heart_rate = st.sidebar.slider("Heart Rate (bpm)", min_value=40, max_value=180, value=75)

# Derived values
mean_pressure = diastolic + (systolic - diastolic) / 3
cycle_duration = 60 / heart_rate  # seconds per beat

# Generate waveform
def generate_waveform(duration=5, fs=1000):
    t = np.linspace(0, duration, int(fs * duration))
    waveform = np.zeros_like(t)
    for i in range(int(duration / cycle_duration)):
        start = int(i * cycle_duration * fs)
        end = int((i + 1) * cycle_duration * fs)
        beat_t = np.linspace(0, cycle_duration, end - start)
        beat_wave = (
            0.3 * np.sin(2 * np.pi * beat_t / cycle_duration) +
            0.2 * np.sin(4 * np.pi * beat_t / cycle_duration) +
            0.1 * np.sin(6 * np.pi * beat_t / cycle_duration)
        )
        beat_wave = (beat_wave - beat_wave.min()) / (beat_wave.max() - beat_wave.min())
        beat_wave = diastolic + beat_wave * (systolic - diastolic)
        waveform[start:end] = beat_wave
    return t, waveform

# Generate waveform
t, waveform = generate_waveform()

# Plot waveform
fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(t, waveform, color='red')
ax.set_title("Simulated Arterial Blood Pressure Waveform")
ax.set_xlabel("Time (s)")
ax.set_ylabel("Pressure (mmHg)")
ax.set_ylim(30, 220)
ax.grid(True)

# Display plot
st.pyplot(fig)

# Display values
st.markdown(f"**Systolic:** {systolic} mmHg &nbsp;&nbsp;&nbsp; **Diastolic:** {diastolic} mmHg &nbsp;&nbsp;&nbsp; **Mean Arterial Pressure (MAP):** {mean_pressure:.1f} mmHg &nbsp;&nbsp;&nbsp; **Heart Rate:** {heart_rate} bpm")
