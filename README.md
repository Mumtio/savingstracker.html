<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Savings Tracker 💰</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Lexend:wght@300;400;600&family=Press+Start+2P&display=swap');

        :root {
            --bg-light: linear-gradient(135deg, #f8d7e8, #f0e2ff);
            --bg-dark: linear-gradient(135deg, #2b2b2b, #1e1e1e);
            --card-light: white;
            --card-dark: #444;
            --text-light: #ff5e78;
            --text-dark: #ffd1dc;
            --progress-light: #ffcc5c;
            --progress-dark: #ff8b00;
        }

        * {
            font-family: "Lexend", sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            transition: all 0.3s ease-in-out;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: var(--bg-light);
        }

        .dark-mode {
            background: var(--bg-dark);
            color: var(--text-dark);
        }

        .card {
            background: var(--card-light);
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0px 6px 15px rgba(0, 0, 0, 0.1);
            max-width: 380px;
            width: 90%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .dark-mode .card {
            background: var(--card-dark);
        }

        .title {
            font-size: 20px;
            font-weight: bold;
            color: var(--text-light);
            font-family: "Press Start 2P", cursive;
            margin-bottom: 15px;
        }

        .dark-mode .title {
            color: var(--text-dark);
        }

        .goal-display {
            font-size: 22px;
            font-weight: bold;
            margin-bottom: 15px;
            color: var(--text-light);
            background: #ffe5b4;
            padding: 8px 15px;
            border-radius: 15px;
            display: none;
        }

        .dark-mode .goal-display {
            background: #ffb84d;
            color: var(--text-dark);
        }

        .input-group {
            margin: 10px 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
        }

        input {
            width: 80%;
            padding: 8px;
            border: none;
            border-radius: 8px;
            background: #ffccd5;
            font-size: 16px;
            text-align: center;
        }

        .progress-container {
            width: 100%;
            background: #222;
            height: 20px;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 10px;
            border: 2px solid #ffb84d;
        }

        .dark-mode .progress-container {
            background: #666;
            border: 2px solid #ff7b00;
        }

        .progress-bar {
            height: 100%;
            width: 0%;
            background: var(--progress-light);
            transition: width 0.5s ease-in-out;
        }

        .dark-mode .progress-bar {
            background: var(--progress-dark);
        }

        .percentage {
            margin-top: 5px;
            font-weight: bold;
            font-size: 18px;
            color: var(--text-light);
        }

        .dark-mode .percentage {
            color: var(--text-dark);
        }

        button {
            background: linear-gradient(135deg, #ffb6c1, #ff7fa3);
            border: none;
            color: white;
            padding: 10px 18px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 10px;
            cursor: pointer;
            margin-top: 15px;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            box-shadow: 0px 4px 8px rgba(255, 105, 180, 0.3);
        }

        button:hover {
            transform: scale(1.05);
            box-shadow: 0px 6px 12px rgba(255, 105, 180, 0.5);
        }

        .toggle-mode {
            position: absolute;
            top: 10px;
            right: 20px;
            cursor: pointer;
            font-size: 18px;
        }
    </style>
</head>
<body>

    <div class="toggle-mode" onclick="toggleMode()">🌙</div>

    <div class="card">
        <div class="title">🎮 RPG Savings Tracker 💰</div>

        <div id="goalDisplay" class="goal-display"></div>

        <div class="input-group" id="goalInputGroup">
            <label>🎯 Set Your Goal ($):</label>
            <input type="number" id="goalAmount" placeholder="Enter your target savings">
            <button onclick="setGoal()">Confirm Goal</button>
        </div>

        <div class="input-group">
            <label>💵 Enter Amount Saved ($):</label>
            <input type="number" id="savedAmount" placeholder="Enter your savings">
        </div>

        <button onclick="updateProgress()">Update Progress</button>

        <div class="progress-container">
            <div class="progress-bar" id="progressBar"></div>
        </div>

        <div class="percentage" id="percentageText">0% Saved</div>
    </div>

    <script>
        function setGoal() {
            let goalAmount = document.getElementById("goalAmount").value;
            if (goalAmount <= 0 || isNaN(goalAmount)) {
                alert("Please enter a valid goal amount!");
                return;
            }
            localStorage.setItem("goalAmount", goalAmount);
            document.getElementById("goalDisplay").innerText = "🎯 Goal: $" + goalAmount;
            document.getElementById("goalDisplay").style.display = "block";
            document.getElementById("goalInputGroup").style.display = "none";
        }

        function updateProgress() {
            let goalAmount = parseFloat(localStorage.getItem("goalAmount"));
            let savedAmount = parseFloat(document.getElementById("savedAmount").value);
            let progressBar = document.getElementById("progressBar");
            let percentageText = document.getElementById("percentageText");

            if (isNaN(goalAmount) || isNaN(savedAmount) || goalAmount <= 0) {
                alert("Please set a goal first!");
                return;
            }

            let percentage = Math.min((savedAmount / goalAmount) * 100, 100).toFixed(2);
            progressBar.style.width = percentage + "%";
            percentageText.innerText = percentage + "% Saved 🎯";

            if (percentage >= 100) {
                percentageText.innerText = "🎉 Goal Reached! You did it!";
            }
        }

        function toggleMode() {
            document.body.classList.toggle("dark-mode");
        }

        window.onload = () => {
            let savedGoal = localStorage.getItem("goalAmount");
            if (savedGoal) {
                document.getElementById("goalDisplay").innerText = "🎯 Goal: $" + savedGoal;
                document.getElementById("goalDisplay").style.display = "block";
                document.getElementById("goalInputGroup").style.display = "none";
            }
        };
    </script>

</body>
</html>
