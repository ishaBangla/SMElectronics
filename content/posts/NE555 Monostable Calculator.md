---
title: "NE555 Monostable Calculator"
date: 2025-05-14T11:00:00+05:30
draft: false
---

Calculate the pulse duration for your NE555 timer in monostable mode. Enter your resistance and capacitance values to find the output pulse width.

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
  }

  .calculator .input-group input,
  .calculator .input-group select {
    flex: 1;
    padding: 0.4rem;
    border: 2px solid #007bff; /* Blue border */
    border-radius: 5px;
    outline: none;
    box-sizing: border-box;
  }

  .calculator .input-group input:focus,
  .calculator .input-group select:focus {
    border-color: #0056b3;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.3); /* Subtle blue glow */
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

  .error-message {
    color: red;
    margin-top: 1rem;
  }

  .result {
    margin-top: 1rem;
    font-weight: bold;
  }
</style>

<div class="calculator">
  <label for="capacitanceValue">Capacitance:</label>
  <div class="input-group">
    <input type="number" id="capacitanceValue" min="0" step="any">
    <select id="capacitanceUnit">
      <option value="1e-6">micro Farad (μF)</option>
      <option value="1e-9">nano Farad (nF)</option>
    </select>
  </div>
  
  <label for="resistanceValue">Resistance:</label>
  <div class="input-group">
    <input type="number" id="resistanceValue" min="0" step="any">
    <select id="resistanceUnit">
      <option value="1">ohm (Ω)</option>
      <option value="1000">Kilo ohm (KΩ)</option>
      <option value="1000000">Mega ohm (MΩ)</option>
    </select>
  </div>
  
  <label for="timeValue">Output Pulse Width Time:</label>
  <div class="input-group">
    <input type="number" id="timeValue" step="any">
    <select id="timeUnit" onchange="calculate()">
      <option value="1">Seconds</option>
      <option value="60">Minutes</option>
      <option value="3600">Hours</option>
    </select>
  </div>
  
  <button onclick="calculate()">Calculate</button>
  
  <div class="error-message" id="errorMessage"></div>
  <div class="result" id="resultFields"></div>
</div>

<script>
  function calculate() {
    const capacitanceValue = parseFloat(document.getElementById("capacitanceValue").value);
    const capacitanceUnit = parseFloat(document.getElementById("capacitanceUnit").value);
    const resistanceValue = parseFloat(document.getElementById("resistanceValue").value);
    const resistanceUnit = parseFloat(document.getElementById("resistanceUnit").value);
    const timeValue = parseFloat(document.getElementById("timeValue").value);
    const timeUnit = parseFloat(document.getElementById("timeUnit").value);
    const errorMessage = document.getElementById("errorMessage");

    const capacitanceInFarads = capacitanceValue * capacitanceUnit;
    const resistanceInOhms = resistanceValue * resistanceUnit;
    const timeInSeconds = timeValue * timeUnit;

    if (isFinite(capacitanceInFarads) && isFinite(resistanceInOhms)) {
      errorMessage.textContent = "";
      const calculatedTime = 1.1 * resistanceInOhms * capacitanceInFarads;
      updateTimeField(calculatedTime);
    } else if (isFinite(capacitanceInFarads) && isFinite(timeInSeconds)) {
      errorMessage.textContent = "";
      const calculatedResistance = timeInSeconds / (1.1 * capacitanceInFarads);
      updateResistanceField(calculatedResistance);
    } else if (isFinite(resistanceInOhms) && isFinite(timeInSeconds)) {
      errorMessage.textContent = "";
      const calculatedCapacitance = timeInSeconds / (1.1 * resistanceInOhms);
      updateCapacitanceField(calculatedCapacitance);
    } else {
      errorMessage.textContent = "Invalid input";
    }
  }

  function updateTimeField(calculatedTime) {
    const timeUnit = parseFloat(document.getElementById("timeUnit").value);
    if (timeUnit === 60) {
      document.getElementById("timeValue").value = (calculatedTime / 60).toFixed(6);
    } else if (timeUnit === 3600) {
      document.getElementById("timeValue").value = (calculatedTime / 3600).toFixed(6);
    } else {
      document.getElementById("timeValue").value = calculatedTime.toFixed(6);
    }
  }

  function updateResistanceField(calculatedResistance) {
    const resistanceUnit = parseFloat(document.getElementById("resistanceUnit").value);
    if (resistanceUnit === 1000) {
      document.getElementById("resistanceValue").value = (calculatedResistance / 1000).toFixed(6);
    } else if (resistanceUnit === 1000000) {
      document.getElementById("resistanceValue").value = (calculatedResistance / 1000000).toFixed(6);
    } else {
      document.getElementById("resistanceValue").value = calculatedResistance.toFixed(6);
    }
  }

  function updateCapacitanceField(calculatedCapacitance) {
    const capacitanceUnit = parseFloat(document.getElementById("capacitanceUnit").value);
    if (capacitanceUnit === 1e-6) {
      document.getElementById("capacitanceValue").value = (calculatedCapacitance * 1e6).toFixed(6);
    } else {
      document.getElementById("capacitanceValue").value = (calculatedCapacitance * 1e9).toFixed(6);
    }
  }
</script>
