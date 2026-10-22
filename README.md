<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة إكس أو</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f2f5;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            color: #333;
            margin-bottom: 10px;
        }
        #status {
            font-size: 1.2rem;
            margin-bottom: 20px;
            color: #555;
            font-weight: bold;
        }
        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            background-color: #333;
            padding: 5px;
            border-radius: 10px;
        }
        .cell {
            background-color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            font-weight: bold;
            cursor: pointer;
            user-select: none;
            border-radius: 5px;
            transition: background-color 0.2s;
        }
        .cell:hover {
            background-color: #e0e0e0;
        }
        #restartBtn {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 1rem;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        #restartBtn:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>

    <h1>لعبة إكس أو (Tic-Tac-Toe)</h1>
    <div id="status">دور اللاعب: X</div>
    
    <div class="board" id="board">
        <div class="cell" data-index="0"></div>
        <div class="cell" data-index="1"></div>
        <div class="cell" data-index="2"></div>
        <div class="cell" data-index="3"></div>
        <div class="cell" data-index="4"></div>
        <div class="cell" data-index="5"></div>
        <div class="cell" data-index="6"></div>
        <div class="cell" data-index="7"></div>
        <div class="cell" data-index="8"></div>
    </div>

    <button id="restartBtn" onclick="restartGame()">إعادة اللعبة</button>

    <script>
        const cells = document.querySelectorAll('.cell');
        const statusText = document.getElementById('status');
        
        let options = ["", "", "", "", "", "", "", "", ""];
        let currentPlayer = "X";
        let running = true;

        const winConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // صفوف
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // أعمدة
            [0, 4, 8], [2, 4, 6]             // أقطار
        ];

        initializeGame();

        function initializeGame() {
            cells.forEach(cell => cell.addEventListener('click', cellClicked));
            statusText.textContent = `دور اللاعب: ${currentPlayer}`;
        }

        function cellClicked() {
            const cellIndex = this.getAttribute('data-index');

            if (options[cellIndex] != "" || !running) {
                return;
            }

            updateCell(this, cellIndex);
            checkWinner();
        }

        function updateCell(cell, index) {
            options[index] = currentPlayer;
            cell.textContent = currentPlayer;
            cell.style.color = currentPlayer === "X" ? "#ff4757" : "#2ed573";
        }

        function changePlayer() {
            currentPlayer = (currentPlayer == "X") ? "O" : "X";
            statusText.textContent = `دور اللاعب: ${currentPlayer}`;
        }

        function checkWinner() {
            let roundWon = false;

            for (let i = 0; i < winConditions.length; i++) {
                const condition = winConditions[i];
                const cellA = options[condition[0]];
                const cellB = options[condition[1]];
                const cellC = options[condition[2]];

                if (cellA == "" || cellB == "" || cellC == "") {
                    continue;
                }
                if (cellA == cellB && cellB == cellC) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                statusText.textContent = `فاز اللاعب ${currentPlayer}! 🎉`;
                running = false;
            } else if (!options.includes("")) {
                statusText.textContent = `تعادل! 🤝`;
                running = false;
            } else {
                changePlayer();
            }
        }

        function restartGame() {
            currentPlayer = "X";
            options = ["", "", "", "", "", "", "", "", ""];
            statusText.textContent = `دور اللاعب: ${currentPlayer}`;
            cells.forEach(cell => {
                cell.textContent = "";
                cell.style.color = "#333";
            });
            running = true;
        }
    </script>
</body>
</html>
