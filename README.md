# skillcraft_task-3
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sudoku Solver</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #101820;
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }
    h1 {
      color: #00e6e6;
    }
    table {
      border-collapse: collapse;
      margin: 20px 0;
    }
    td {
      border: 1px solid #00e6e6;
      width: 40px;
      height: 40px;
      text-align: center;
    }
    input {
      width: 38px;
      height: 38px;
      text-align: center;
      font-size: 18px;
      border: none;
      background: #1a1a2e;
      color: #00e6e6;
    }
    input:focus {
      outline: 2px solid #00e6e6;
    }
    button {
      background: #00e6e6;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      color: #101820;
      cursor: pointer;
      border-radius: 5px;
      margin: 5px;
    }
    button:hover {
      background: #009999;
    }
  </style>
</head>
<body>
  <h1>Sudoku Solver</h1>
  <table id="sudoku-grid"></table>
  <div>
    <button onclick="solveSudoku()">Solve</button>
    <button onclick="clearGrid()">Clear</button>
  </div>

  <script>
    const grid = document.getElementById("sudoku-grid");

    // Create 9x9 input grid
    for (let r = 0; r < 9; r++) {
      const row = document.createElement("tr");
      for (let c = 0; c < 9; c++) {
        const cell = document.createElement("td");
        const input = document.createElement("input");
        input.type = "number";
        input.min = 1;
        input.max = 9;
        cell.appendChild(input);
        row.appendChild(cell);
      }
      grid.appendChild(row);
    }

    function getGridValues() {
      const values = [];
      for (let r = 0; r < 9; r++) {
        const row = [];
        for (let c = 0; c < 9; c++) {
          const val = grid.rows[r].cells[c].firstChild.value;
          row.push(val ? parseInt(val) : 0);
        }
        values.push(row);
      }
      return values;
    }

    function setGridValues(values) {
      for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
          grid.rows[r].cells[c].firstChild.value = values[r][c] === 0 ? "" : values[r][c];
        }
      }
    }

    function isValid(board, row, col, num) {
      for (let x = 0; x < 9; x++) {
        if (board[row][x] === num || board[x][col] === num) return false;
      }
      const startRow = Math.floor(row / 3) * 3;
      const startCol = Math.floor(col / 3) * 3;
      for (let r = 0; r < 3; r++) {
        for (let c = 0; c < 3; c++) {
          if (board[startRow + r][startCol + c] === num) return false;
        }
      }
      return true;
    }

    function solve(board) {
      for (let row = 0; row < 9; row++) {
        for (let col = 0; col < 9; col++) {
          if (board[row][col] === 0) {
            for (let num = 1; num <= 9; num++) {
              if (isValid(board, row, col, num)) {
                board[row][col] = num;
                if (solve(board)) return true;
                board[row][col] = 0;
              }
            }
            return false;
          }
        }
      }
      return true;
    }

    function solveSudoku() {
      const board = getGridValues();
      if (solve(board)) {
        setGridValues(board);
        alert("Sudoku Solved!");
      } else {
        alert("No solution found.");
      }
    }

    function clearGrid() {
      for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
          grid.rows[r].cells[c].firstChild.value = "";
        }
      }
    }
  </script>
</body>
</html>
