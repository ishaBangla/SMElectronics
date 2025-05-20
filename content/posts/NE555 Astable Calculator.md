---
title: "NE555 Astable Calculator"
date: 2025-05-14T11:30:00+05:30
draft: false
---
<img src="https://ishaBangla.github.io/SMElectronics/images/s.jpg" alt="555 Circuit" />
This advanced NE555 Astable Calculator to determine frequency, duty cycle, and timing for your oscillator circuit.
<style>
  .calculator {
    max-width: 500px;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 10px;
    font-family: sans-serif;
  }

  .calculator .input-group {
    display: flex;
    gap: 0.5rem;
    margin: 0.5rem 0;
    align-items: center;
  }

  .calculator .input-group input,
  .calculator .input-group select {
    width: 170px;
    padding: 0.4rem;
    border: 2px solid #007bff;
    border-radius: 5px;
    outline: none;
    box-sizing: border-box;
  }

  .calculator .input-group input:focus,
  .calculator .input-group select:focus {
    border-color: #0056b3;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.3);
  }

  .calculator button {
    margin-top: 1rem;
    padding: 0.5rem;
    width: 100%;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .result {
    margin-top: 1rem;
    font-weight: bold;
  }

  .error-message {
    color: red;
    margin-top: 1rem;
  }
</style>


<div class="calculator">
  <label>Capacitance (C):</label>
  <div class="input-group">
    <input type="number" id="capacitanceValue" placeholder="e.g., 10">
    <select id="capacitanceUnit">
      <option value="1e-6">μF</option>
      <option value="1e-9">nF</option>
    </select>
  </div>

  <label>Resistance 1 (R1):</label>
  <div class="input-group">
    <input type="number" id="resistance1Value" placeholder="e.g., 1000">
    <select id="resistance1Unit">
      <option value="1">Ω</option>
      <option value="1000">KΩ</option>
      <option value="1000000">MΩ</option>
    </select>
  </div>

  <label>Resistance 2 (R2):</label>
  <div class="input-group">
    <input type="number" id="resistance2Value" placeholder="e.g., 1000">
    <select id="resistance2Unit">
      <option value="1">Ω</option>
      <option value="1000">KΩ</option>
      <option value="1000000">MΩ</option>
    </select>
  </div>

  <button onclick="calculate()">Calculate</button>

  <div class="error-message" id="errorMessage"></div>
  <div class="result" id="resultFields"></div>
</div>

<script>
  function calculate() {
    const C = parseFloat(document.getElementById("capacitanceValue").value);
    const Cunit = parseFloat(document.getElementById("capacitanceUnit").value);
    const R1 = parseFloat(document.getElementById("resistance1Value").value);
    const R1unit = parseFloat(document.getElementById("resistance1Unit").value);
    const R2 = parseFloat(document.getElementById("resistance2Value").value);
    const R2unit = parseFloat(document.getElementById("resistance2Unit").value);

    const errorMsg = document.getElementById("errorMessage");
    const resultDiv = document.getElementById("resultFields");

    if (isNaN(C) || isNaN(R1) || isNaN(R2)) {
      errorMsg.textContent = "Please enter valid numeric values for C, R1, and R2.";
      resultDiv.innerHTML = "";
      return;
    }

    errorMsg.textContent = "";

    const Cfarads = C * Cunit;
    const R1ohms = R1 * R1unit;
    const R2ohms = R2 * R2unit;

    const T1 = 0.693 * (R1ohms + R2ohms) * Cfarads;
    const T2 = 0.693 * R2ohms * Cfarads;
    const T = T1 + T2;
    const freq = 1 / T;
    const duty = (T1 / T) * 100;

    let freqDisplay;
    if (freq >= 1e6) {
      freqDisplay = (freq / 1e6).toFixed(3) + " MHz";
    } else if (freq >= 1e3) {
      freqDisplay = (freq / 1e3).toFixed(3) + " kHz";
    } else {
      freqDisplay = freq.toFixed(3) + " Hz";
    }

    resultDiv.innerHTML = `
      <p>Time High (T1): ${formatTime(T1)} (${T1.toFixed(6)} seconds)</p>
      <p>Time Low (T2): ${formatTime(T2)} (${T2.toFixed(6)} seconds)</p>
      <p>Period (T): ${formatTime(T)} (${T.toFixed(6)} seconds)</p>
      <p>Frequency: ${freqDisplay}</p>
      <p>Duty Cycle: ${duty.toFixed(2)}%</p>
    `;
  }

  function formatTime(seconds) {
    const hrs = Math.floor(seconds / 3600);
    const mins = Math.floor((seconds % 3600) / 60);
    const secs = (seconds % 60).toFixed(2);

    let result = "";

    if (hrs > 0) result += `${hrs} hr `;
    if (mins > 0 || hrs > 0) result += `${mins} min `;
    result += `${secs} sec`;

    return result.trim();
  }
</script>

