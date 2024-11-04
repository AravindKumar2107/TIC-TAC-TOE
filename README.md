import tkinter as tk
from tkinter import messagebox

# Initialize the main window
root = tk.Tk()
root.title("Tic-Tac-Toe")
root.geometry("400x550")  # Adjusted height for score display
root.configure(bg="#1e1e1e")  # Dark background color

# Global variables
current_player = "X"
board = [" " for _ in range(9)]  # Empty board
buttons = []  # List to store board buttons
score = {"X": 0, "O": 0, "Tie": 0}  # Score tracker

# Function to check if someone has won
def check_winner():
    win_combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  # Horizontal
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  # Vertical
        [0, 4, 8], [2, 4, 6]              # Diagonal
    ]
    for combo in win_combinations:
        if board[combo[0]] == board[combo[1]] == board[combo[2]] != " ":
            return True
    return False

# Function to check if the board is full
def is_board_full():
    return " " not in board

# Function to handle button clicks
def handle_click(position):
    global current_player

    if board[position] != " ":
        messagebox.showwarning("Invalid Move", "This spot is already taken!")
        return

    board[position] = current_player
    buttons[position].config(text=current_player, state="disabled", disabledforeground="#ffffff")

    if check_winner():
        messagebox.showinfo("Game Over", f"Player {current_player} wins!")
        score[current_player] += 1
        update_scoreboard()
        play_again_button.pack(pady=10)
        return

    if is_board_full():
        messagebox.showinfo("Game Over", "It's a tie!")
        score["Tie"] += 1
        update_scoreboard()
        play_again_button.pack(pady=10)
        return

    current_player = "O" if current_player == "X" else "X"

# Function to reset the board
def reset_board():
    global board, current_player
    board = [" " for _ in range(9)]
    current_player = "X"
    for button in buttons:
        button.config(text="", state="normal")
    play_again_button.pack_forget()  # Hide "Play Again" button

# Function to create the game board
def create_board():
    start_button.destroy()  # Remove the "Start Game" button

    # Create a frame to hold the board buttons
    board_frame = tk.Frame(root, bg="#1e1e1e")
    board_frame.pack(pady=20)

    for i in range(9):
        button = tk.Button(
            board_frame, text="", font=("Helvetica", 24), width=5, height=2,
            bg="#333333", fg="#ffffff", activebackground="#4caf50",
            command=lambda i=i: handle_click(i)
        )
        button.grid(row=i // 3, column=i % 3, padx=5, pady=5)
        buttons.append(button)

# Function to update the scoreboard display
def update_scoreboard():
    score_label.config(text=f"X: {score['X']}   O: {score['O']}   Ties: {score['Tie']}")

# Create the "Play Again" button, initially hidden
play_again_button = tk.Button(
    root, text="Play Again", font=("Helvetica", 14), bg="#4caf50", fg="#ffffff",
    activebackground="#388e3c", activeforeground="#ffffff", command=reset_board, width=12, height=1
)

# Create the "Start Game" button
start_button = tk.Button(
    root, text="Start the Game", font=("Helvetica", 18, "bold"), bg="#4caf50", fg="#ffffff",
    activebackground="#388e3c", activeforeground="#ffffff", command=create_board, width=15, height=2
)
start_button.pack(expand=True)

# Create the scoreboard label at the bottom
score_label = tk.Label(root, text="X: 0   O: 0   Ties: 0", font=("Helvetica", 16), bg="#1e1e1e", fg="#ffffff")
score_label.pack(side="bottom", pady=10)

# Start the Tkinter event loop
root.mainloop()
