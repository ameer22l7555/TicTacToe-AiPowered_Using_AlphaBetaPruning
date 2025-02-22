# 🎮 Tic-Tac-Toe with AI Opponent 🤖

[![Status](https://img.shields.io/badge/Status-Completed-brightgreen)](https://github.com/ameer22l7555/TicTacToe-AiPowered_Using_AlphaBetaPruning)  [![GitHub Repo](https://img.shields.io/badge/GitHub-Repo-blue)](https://github.com/ameer22l7555/TicTacToe-AiPowered_Using_AlphaBetaPruning)

This repository contains a **Tic-Tac-Toe** game implementation where a human player can play against an AI opponent. The AI uses the **Minimax Algorithm** with **Alpha-Beta Pruning** to make optimal moves, ensuring a challenging and fair gameplay experience.

👉 Check out the project on GitHub: [TicTacToe-AiPowered_Using_AlphaBetaPruning](https://github.com/ameer22l7555/TicTacToe-AiPowered_Using_AlphaBetaPruning)

---

## 📋 Table of Contents

1. [Game Overview](#game-overview)
2. [Functions Explanation](#functions-explanation)
3. [Game Loop](#game-loop)
4. [End of Game](#end-of-game)
5. [How to Run](#how-to-run)
6. [Contributing](#contributing)
7. [License](#license)

---

## 🎮 Game Overview

The game is played on a **3x3 grid**, where two players take turns marking a cell with their symbol (`X` or `O`). The first player to align three of their symbols horizontally, vertically, or diagonally wins the game. If all cells are filled without a winner, the game ends in a **draw**.

- **Human Player**: You!
- **AI Opponent**: Uses the **Minimax Algorithm** with **Alpha-Beta Pruning** to make optimal moves.

---

## 🛠️ Functions Explanation

### 1. `initial_state()`
- **Purpose**: Creates and returns the initial empty Tic-Tac-Toe board.
- **How it works**: Initializes a 3x3 list where each cell is set to `EMPTY`, indicating that no moves have been made yet.

```python
def initial_state():
    return [[EMPTY, EMPTY, EMPTY],
            [EMPTY, EMPTY, EMPTY],
            [EMPTY, EMPTY, EMPTY]]
```

---

### 2. `player(board)`
- **Purpose**: Determines whose turn it is to play (`X` or `O`) based on the current state of the board.
- **How it works**: Counts the number of filled cells. If the count is even, it's `X`'s turn; otherwise, it's `O`'s turn.

```python
def player(board):
    count = 0
    for i in board:
        for j in i:
            if j:
                count += 1
    if count % 2 != 0:
        return O
    return X
```

---

### 3. `result(board, action)`
- **Purpose**: Applies an action (move) to the board and returns the resulting board state.
- **How it works**: Takes the current board and an action (a tuple representing the row and column of the move). It creates a deep copy of the board, updates the specified cell with the current player's symbol, and returns the modified board.

```python
def result(board, action):
    curr_player = player(board)
    result_board = copy.deepcopy(board)
    (i, j) = action
    result_board[i][j] = curr_player
    return result_board
```

---

### 4. `winner(board)`
- **Purpose**: Checks if there is a winner and returns the winning player's symbol (`X` or `O`), or `None` if there is no winner yet.
- **How it works**: Iterates through all possible winning combinations (rows, columns, and diagonals) and checks if any of them have the same symbol.

```python
def winner(board):
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2]:  
            return board[i][0]
        if board[0][i] == board[1][i] == board[2][i]:
            return board[0][i]
    if board[0][0] == board[1][1] == board[2][2]:
        return board[0][0]
    if board[0][2] == board[1][1] == board[2][0]:
        return board[0][2]
    return None
```

---

### 5. `terminal(board)`
- **Purpose**: Checks if the game has ended (either a player has won or there are no more empty cells).
- **How it works**: First checks if there is a winner using the `winner()` function. If there is a winner, it returns `True`. Otherwise, it checks if there are any empty cells on the board. If there are no empty cells, it returns `True`, indicating a draw.

```python
def terminal(board):
    if winner(board) != None:
        return True
    for i in board:
        for j in i:
            if j == EMPTY:
                return False
    return True
```

---

### 6. `utility(board)`
- **Purpose**: Assigns a numerical value to the board state based on the outcome of the game.
- **How it works**: If `X` has won, it returns `1`. If `O` has won, it returns `-1`. If the game is a draw, it returns `0`.

```python
def utility(board):
    winner_val = winner(board)
    if winner_val == X:
        return 1
    elif winner_val == O:
        return -1
    return 0
```

---

### 7. `actions(board)`
- **Purpose**: Returns a set of all possible actions (moves) that can be taken from the current board state.
- **How it works**: Iterates through all cells on the board and adds the coordinates of empty cells to a set. This set represents all the valid moves that can be made.

```python
def actions(board):
    res = set()
    board_len = len(board)
    for i in range(board_len):
        for j in range(board_len):
            if board[i][j] == EMPTY:
                res.add((i, j))
    return res
```

---

### 8. `is_valid_action(board, coordinates)`
- **Purpose**: Checks if the move by the user is valid or not.
- **How it works**: Checks if the coordinates lie on the board and if they do, checks if the coordinate is empty.

```python
def is_valid_action(board, coordinates):
    x_corr, y_corr = coordinates
    if 0 <= x_corr <= 2 and 0 <= y_corr <= 2:
        if board[x_corr][y_corr] == EMPTY:
            return True
        return False
    return False
```

---

### 9. `get_player_move(board)`
- **Purpose**: Gets the coordinates from the user.
- **How it works**: Calls `is_valid_action` with the user's move and keeps prompting until a valid action is entered by the user.

```python
def get_player_move(board):
    while True:
        try:
            x = int(input("Enter the Row Value (1-3): "))
            y = int(input("Enter the Column Value (1-3): "))
            x = x - 1
            y = y - 1
            if is_valid_action(board, (x, y)):
                return (x, y)
            else:
                print("Invalid move. Try again.")
        except ValueError:
            print("Invalid input. Please enter a number between 1 and 3.")
```

---

### 10. `max_value(board, alpha, beta)` & `min_value(board, alpha, beta)`
- **Purpose**: These functions are part of the **Minimax Algorithm** and represent the maximizing (`X`) and minimizing (`O`) players, respectively.
- **How it works**: They recursively explore possible moves, maximizing or minimizing the score while considering the opponent's responses. **Alpha-Beta Pruning** is used to optimize the search by eliminating branches.

```python
def max_value(board, alpha, beta):
    if terminal(board):
        return utility(board)
    v = -math.inf
    for action in actions(board):
        v = max(v, min_value(result(board, action), alpha, beta))
        if v >= beta:
            return v
        alpha = max(alpha, v)
    return v

def min_value(board, alpha, beta):
    if terminal(board):
        return utility(board)
    v = math.inf
    for action in actions(board):
        v = min(v, max_value(result(board, action), alpha, beta))
        if v <= alpha:
            return v
        beta = min(beta, v)
    return v
```

---

### 11. `alpha_beta_search(board)`
- **Purpose**: This is the main function for AI move selection using the **Minimax Algorithm** with **Alpha-Beta Pruning**.
- **How it works**: Determines the current player, then calls either `min_value` or `max_value` (depending on the player) to find the optimal move, ultimately returning the best action (coordinates).

```python
def alpha_beta_search(board):
    current_player = player(board)
    if current_player == X:
        best_score = -math.inf
        best_action = None
        for action in actions(board):
            score = min_value(result(board, action), -math.inf, math.inf)
            if score > best_score:
                best_score = score
                best_action = action
        return best_action
    else: 
        best_score = math.inf
        best_action = None
        for action in actions(board):
            score = max_value(result(board, action), -math.inf, math.inf)
            if score < best_score:
                best_score = score
                best_action = action
        return best_action
```

---

### 12. `printboard(board)`
- **Purpose**: Prints the current state of the board.
- **How it works**: Iterates through the rows and prints the elements in a visually appealing format.

```python
def printboard(board):
    print("  1   2   3")
    for i, row in enumerate(board):
        print("  --- --- ---") 
        print(i + 1, end="|") 
        for j, cell in enumerate(row):
            if cell is None:
                print("   |", end="")
            else:
                print(f" {cell} |", end="")
        print() 
    print("  --- --- ---") 
```

---

## 🔁 Game Loop

The `while` loop in the main part of the program continues as long as the game is not over (`terminal(board)` is `False`). Inside the loop:

1. The current player is determined.
2. If it's the AI's turn, it calls `alpha_beta_search()` to get the optimal move.
3. If it's the human player's turn, it prompts for input to get their move.
4. The selected move is applied to the board using `result()`.

The loop continues until the game ends.

---

## 🏁 End of Game

After the loop ends (when the game is over), the final board state is printed, and the winner (if any) is declared.

```python
if winner(board):
    print(f"Winner: {winner(board)}")
else:
    print("It's a draw!")
```

---

## ▶️ How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/ameer22l7555/TicTacToe-AiPowered_Using_AlphaBetaPruning.git
   ```
2. Navigate to the project directory:
   ```bash
   cd TicTacToe-AiPowered_Using_AlphaBetaPruning
   ```
3. Run the game:
   ```bash
   python main.py
   ```

---

## 👥 Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request. Let's make this game even better together! 🚀

---

## 📜 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

🎉 Enjoy the game! 🎉

---
