---
title: "LED Resistor Calculator"
date: 2025-05-14T10:00:00+05:30
draft: false
---
Want to connect an LED safely?  
Use this LED Resistor Calculator to determine the ideal resistor value.

<style>
  .calculator {
    max-width: 400px;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 10px;
    font-family: sans-serif;
  }

  .calculator label, .calculator input {
    display: block;
    margin: 0.5rem 0;
    width: 100%;
    padding-left: 0.5rem;
  }

  .calculator input {
    border: 2px solid #ccc;
    border-radius: 5px;
    padding: 0.5rem;
    outline: none;
    box-sizing: border-box;
  }

  .calculator input:focus {
    border-color: #007bff;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.3);
  }

  .calculator button {
    margin-top: 1rem;
    padding: 0.5rem;
    width: 100%;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .result {
    margin-top: 1rem;
    font-weight: bold;
  }
</style>

<div class="calculator">
  <label for="supply">Supply Voltage (V):</label>
  <input type="number" id="supply" placeholder="e.g., 9" />

  <label for="forward">LED Forward Voltage (V):</label>
  <input type="number" id="forward" placeholder="e.g., 2" />

  <label for="current">Desired LED Current (mA):</label>
  <input type="number" id="current" placeholder="e.g., 20" />

  <button onclick="calculateResistor()">Calculate Resistor</button>

  <div class="result" id="resistorResult"></div>
</div>

<script>
  function calculateResistor() {
    const supply = parseFloat(document.getElementById('supply').value);
    const forward = parseFloat(document.getElementById('forward').value);
    const current = parseFloat(document.getElementById('current').value) / 1000; // mA to A

    if (supply && forward && current && supply > forward) {
      const resistance = (supply - forward) / current;

      // If resistance is greater than 1000, append 'K' to result
      let result = resistance.toFixed(0);
      if (resistance >= 1000) {
        result = (resistance / 1000).toFixed(2) + 'K'; // Convert to KΩ if greater than 1000
      }

      document.getElementById('resistorResult').innerText =
        'Required Resistor: ' + result + ' Ω';
    } else {
      document.getElementById('resistorResult').innerText =
        'Please enter valid values. Supply voltage must be greater than LED voltage.';
    }
  }
</script>
