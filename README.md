# CODESOFT_2
#TIC-TAC-TOE AI
import math

# Initialize the board
board = [" " for _ in range(9)]

# Print the board
def print_board():
    for row in [board[i*3:(i+1)*3] for i in range(3)]:
        print("| " + " | ".join(row) + " |")

# Check for a winner or tie
def check_winner(b, player):
    win_conditions = [
        [0,1,2], [3,4,5], [6,7,8], # rows
        [0,3,6], [1,4,7], [2,5,8], # columns
        [0,4,8], [2,4,6]           # diagonals
    ]
    for cond in win_conditions:
        if b[cond[0]] == b[cond[1]] == b[cond[2]] == player:
            return True
    return False

def is_draw(b):
    return " " not in b

# Get empty positions
def available_moves(b):
    return [i for i, spot in enumerate(b) if spot == " "]

# Minimax algorithm
def minimax(b, depth, is_maximizing):
    if check_winner(b, "O"):
        return 1
    elif check_winner(b, "X"):
        return -1
    elif is_draw(b):
        return 0

    if is_maximizing:
        best_score = -math.inf
        for move in available_moves(b):
            b[move] = "O"
            score = minimax(b, depth + 1, False)
            b[move] = " "
            best_score = max(score, best_score)
        return best_score
    else:
        best_score = math.inf
        for move in available_moves(b):
            b[move] = "X"
            score = minimax(b, depth + 1, True)
            b[move] = " "
            best_score = min(score, best_score)
        return best_score

# Get best move for AI
def best_move():
    best_score = -math.inf
    move = None
    for i in available_moves(board):
        board[i] = "O"
        score = minimax(board, 0, False)
        board[i] = " "
        if score > best_score:
            best_score = score
            move = i
    return move

# Main game loop
def play_game():
    print("Welcome to Tic-Tac-Toe!")
    print_board()

    while True:
        # Human turn
        user_move = int(input("Enter your move (0-8): "))
        if board[user_move] != " ":
            print("Invalid move. Try again.")
            continue
        board[user_move] = "X"

        print_board()
        if check_winner(board, "X"):
            print("You win!")
            break
        elif is_draw(board):
            print("It's a draw!")
            break

        # AI turn
        ai_move = best_move()
        board[ai_move] = "O"
        print("AI played:")
        print_board()
        if check_winner(board, "O"):
            print("AI wins!")
            break
        elif is_draw(board):
            print("It's a draw!")
            break

# Start the game
if __name__ == "__main__":
    play_game()
