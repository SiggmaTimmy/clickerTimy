<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clicker Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 20px;
        }
        #game-container {
            max-width: 600px;
            margin: auto;
        }
        .button {
            padding: 15px 30px;
            font-size: 20px;
            cursor: pointer;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            margin: 20px;
        }
        .button:active {
            background-color: #45a049;
        }
        .upgrade-button, .rebirth-button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #008CBA;
            color: white;
            border: none;
            border-radius: 5px;
            margin-top: 20px;
        }
        .upgrade-button:active, .rebirth-button:active {
            background-color: #007b9e;
        }
        .info {
            font-size: 18px;
            margin: 10px 0;
        }
        .rebirth-button {
            position: fixed;
            bottom: 20px;
            right: 20px;
        }
    </style>
</head>
<body>

<div id="game-container">
    <h1>Clicker Game</h1>
    <div class="info">
        <p>Points: <span id="points">0</span></p>
        <p>Points Per Click: <span id="ppc">1</span></p>
        <p>Multiplier: <span id="multiplier">1x</span></p>
    </div>
    <button class="button" id="click-button">Click me!</button>

    <div class="info">
        <p>Upgrade Click: <span id="upgrade-cost">50</span> Points</p>
        <button class="upgrade-button" id="upgrade-button" disabled>Upgrade Click</button>
    </div>
</div>

<button class="rebirth-button" id="rebirth-button" disabled>Rebirth</button>

<script>
    // Initial game state variables
    let points = 0;
    let pointsPerClick = 1;
    let multiplier = 1;
    let upgradeCost = 50;
    let rebirthCost = 100000;
    let rebirthMultiplier = 2;

    // DOM Elements
    const pointsElement = document.getElementById('points');
    const ppcElement = document.getElementById('ppc');
    const multiplierElement = document.getElementById('multiplier');
    const clickButton = document.getElementById('click-button');
    const upgradeButton = document.getElementById('upgrade-button');
    const upgradeCostElement = document.getElementById('upgrade-cost');
    const rebirthButton = document.getElementById('rebirth-button');

    // Handle button click
    clickButton.addEventListener('click', () => {
        points += pointsPerClick * multiplier; // Increase points by points per click and multiplier
        updateDisplay();
    });

    // Handle upgrade button click
    upgradeButton.addEventListener('click', () => {
        if (points >= upgradeCost) {
            points -= upgradeCost;
            pointsPerClick += 1; // Increase points per click by 1
            upgradeCost = Math.floor(upgradeCost * 1.5); // Increase upgrade cost by 50%
            updateDisplay();
            upgradeCostElement.textContent = upgradeCost;
            upgradeButton.disabled = points < upgradeCost;
        }
    });

    // Handle rebirth button click
    rebirthButton.addEventListener('click', () => {
        if (points >= rebirthCost) {
            points = 0; // Reset points
            pointsPerClick = 1; // Reset points per click
            multiplier = rebirthMultiplier; // Apply rebirth multiplier
            rebirthMultiplier = rebirthMultiplier === 2 ? 3 : rebirthMultiplier === 3 ? 4 : rebirthMultiplier === 4 ? 6 : rebirthMultiplier === 6 ? 10 : 2; // Increase multiplier in sequence
            rebirthCost = Math.floor(rebirthCost * 1.5); // Increase rebirth cost by 50% for next rebirth
            updateDisplay();
            rebirthButton.disabled = points < rebirthCost;
        }
    });

    // Update the display of points, points per click, and multiplier
    function updateDisplay() {
        pointsElement.textContent = points;
        ppcElement.textContent = pointsPerClick;
        multiplierElement.textContent = `${multiplier}x`;
        upgradeButton.disabled = points < upgradeCost;
        rebirthButton.disabled = points < rebirthCost;
    }

    // Initialize the display
    updateDisplay();
</script>

</body>
</html>
