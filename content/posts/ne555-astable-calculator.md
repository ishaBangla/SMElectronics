---
title: "NE555 Astable Calculator"
date: 2025-03-26T12:00:00+06:00
draft: false
tags: ["NE555", "Electronics", "Circuit Calculator"]
categories: ["Calculators", "Electronics"]
---

<html>
<head>
    <title>NE555 Astable Calculator</title>
    <style>
        body { font-family: Arial, sans-serif; }
        .container { width: 95%; max-width: 400px; margin: 0 auto; padding: 5px; }
        label { display: block; margin-bottom: 5px; }
        .input-group { display: flex; justify-content: space-between; align-items: center; }
        select, input[type="number"], input[type="text"], button {
            width: 100%; padding: 10px; margin-bottom: 15px; border: 1px solid #ccc; border-radius: 5px; box-sizing: border-box;
        }
        button { background-color: #007bff; color: white; cursor: pointer; }
        .error-message { color: red; font-size: 15px; margin-bottom: 15px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>NE555 Astable Calculator</h1>

        <label for="capacitanceValue">Capacitance (C):</label>
        <div class="input-group">
            <input type="number" id="capacitanceValue" min="0" step="any">
            <select id="capacitanceUnit" onchange="calculate()">
                <option value="1e-6">μF</option>
                <option value="1e-9">nF</option>
            </select>
        </div>

        <label for="resistance1Value">Resistance 1 (R1):</label>
        <div class="input-group">
            <input type="number" id="resistance1Value" min="0" step="any">
            <select id="resistance1Unit" onchange="calculate()">
                <option value="1">Ω</option>
                <option value="1000">KΩ</option>
                <option value="1000000">MΩ</option>
            </select>
        </div>

        <label for="resistance2Value">Resistance 2 (R2):</label>
        <div class="input-group">
            <input type="number" id="resistance2Value" min="0" step="any">
            <select id="resistance2Unit" onchange="calculate()">
                <option value="1">Ω</option>
                <option value="1000">KΩ</option>
                <option value="1000000">MΩ</option>
            </select>
        </div>

        <button onclick="calculate()">Calculate</button>

        <div class="error-message" id="errorMessage"></div>

        <label for="timeLowValue">Time Low (T2):</label>
        <input type="text" id="timeLowValue" readonly>

        <label for="timeHighValue">Time High (T1):</label>
        <input type="text" id="timeHighValue" readonly>

        <label for="periodValue">Period (T):</label>
        <input type="text" id="periodValue" readonly>

        <label for="frequencyValue">Frequency:</label>
        <input type="text" id="frequencyValue" readonly>

        <label for="dutyCycleValue">Duty Cycle:</label>
        <input type="text" id="dutyCycleValue" readonly>
    </div>

    <script>
        function calculate() {
            const capacitanceValue = parseFloat(document.getElementById("capacitanceValue").value);
            const selectedCapacitanceUnit = parseFloat(document.getElementById("capacitanceUnit").value);
            const resistance1Value = parseFloat(document.getElementById("resistance1Value").value);
            const selectedResistance1Unit = parseFloat(document.getElementById("resistance1Unit").value);
            const resistance2Value = parseFloat(document.getElementById("resistance2Value").value);
            const selectedResistance2Unit = parseFloat(document.getElementById("resistance2Unit").value);

            const capacitanceInFarads = capacitanceValue * selectedCapacitanceUnit;
            const resistance1InOhms = resistance1Value * selectedResistance1Unit;
            const resistance2InOhms = resistance2Value * selectedResistance2Unit;

            if (isNaN(capacitanceInFarads) || isNaN(resistance1InOhms) || isNaN(resistance2InOhms)) {
                document.getElementById("errorMessage").textContent = "Please enter valid values!";
                return;
            }

            document.getElementById("errorMessage").textContent = "";

            const timeLow = 0.693 * resistance2InOhms * capacitanceInFarads;
            const timeHigh = 0.693 * (resistance1InOhms + resistance2InOhms) * capacitanceInFarads;
            const period = timeLow + timeHigh;
            const frequency = 1 / period;
            const dutyCycle = (timeHigh / period) * 100;

            document.getElementById("timeLowValue").value = timeLow.toFixed(6) + " s";
            document.getElementById("timeHighValue").value = timeHigh.toFixed(6) + " s";
            document.getElementById("periodValue").value = period.toFixed(6) + " s";
            document.getElementById("frequencyValue").value = frequency.toFixed(3) + " Hz";
            document.getElementById("dutyCycleValue").value = dutyCycle.toFixed(3) + "%";
        }
    </script>
</body>
</html>
