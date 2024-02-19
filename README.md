import random

def print_board(board):
    for row in board:
        print(" ".join(row))

def is_winner(board, player):
    # Check rows, columns, and diagonals for a win
    return any(all(cell == player for cell in row) for row in board) or \
           any(all(row[i] == player for row in board) for i in range(3)) or \
           all(board[i][i] == player for i in range(3)) or \
           all(board[i][2 - i] == player for i in range(3))

def is_full(board):
    # Check if the board is full
    return all(cell != ' ' for row in board for cell in row)

def get_empty_cells(board):
    # Return a list of empty cells on the board
    return [(i, j) for i in range(3) for j in range(3) if board[i][j] == ' ']

def minimax(board, depth, maximizing_player):
    if is_winner(board, 'X'):
        return -1  # Human wins
    elif is_winner(board, 'O'):
        return 1  # AI wins
    elif is_full(board):
        return 0  # Draw

    if maximizing_player:
        max_eval = float('-inf')
        for i, j in get_empty_cells(board):
            board[i][j] = 'O'
            eval = minimax(board, depth + 1, False)
            board[i][j] = ' '
            max_eval = max(max_eval, eval)
        return max_eval
    else:
        min_eval = float('inf')
        for i, j in get_empty_cells(board):
            board[i][j] = 'X'
            eval = minimax(board, depth + 1, True)
            board[i][j] = ' '
            min_eval = min(min_eval, eval)
        return min_eval

def best_move(board):
    best_val = float('-inf')
    best_move = None

    for i, j in get_empty_cells(board):
        board[i][j] = 'O'
        move_val = minimax(board, 0, False)
        board[i][j] = ' '

        if move_val > best_val:
            best_move = (i, j)
            best_val = move_val

    return best_move

def main():
    board = [[' ' for _ in range(3)] for _ in range(3)]

    while True:
        print_board(board)

        # Human's move
        row = int(input("Enter the row (0, 1, or 2): "))
        col = int(input("Enter the column (0, 1, or 2): "))
        if board[row][col] == ' ':
            board[row][col] = 'X'
        else:
            print("Cell already occupied. Try again.")
            continue

        if is_winner(board, 'X'):
            print_board(board)
            print("You win!")
            break
        elif is_full(board):
            print_board(board)
            print("It's a draw!")
            break

        # AI's move
        print("AI's move:")
        ai_row, ai_col = best_move(board)
        board[ai_row][ai_col] = 'O'

        if is_winner(board, 'O'):
            print_board(board)
            print("AI wins!")
            break
        elif is_full(board):
            print_board(board)
            print("It's a draw!")
            break

if _name_ == "_main_":
    main()
