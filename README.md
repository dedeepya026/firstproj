# firstproj
//code for a 9*9 sudoku game.
#include <iostream>
using namespace std;

// Size of the Sudoku grid
#define SIZE 9

// Function to print the Sudoku grid
void printGrid(int grid[SIZE][SIZE]) {
    for (int row = 0; row < SIZE; row++) {
        for (int col = 0; col < SIZE; col++) {
            cout << grid[row][col] << " ";
        }
        cout << endl;
    }
}

// Function to check if the current number is already present in the row
bool isPresentInRow(int grid[SIZE][SIZE], int row, int num) {
    for (int col = 0; col < SIZE; col++) {
        if (grid[row][col] == num) {
            return true;
        }
    }
    return false;
}

// Function to check if the current number is already present in the column
bool isPresentInCol(int grid[SIZE][SIZE], int col, int num) {
    for (int row = 0; row < SIZE; row++) {
        if (grid[row][col] == num) {
            return true;
        }
    }
    return false;
}

// Function to check if the current number is already present in the 3x3 box
bool isPresentInBox(int grid[SIZE][SIZE], int startRow, int startCol, int num) {
    for (int row = 0; row < 3; row++) {
        for (int col = 0; col < 3; col++) {
            if (grid[row + startRow][col + startCol] == num) {
                return true;
            }
        }
    }
    return false;
}

// Function to check if it's safe to place the number at the given position
bool isSafe(int grid[SIZE][SIZE], int row, int col, int num) {
    return !isPresentInRow(grid, row, num) &&
           !isPresentInCol(grid, col, num) &&
           !isPresentInBox(grid, row - row % 3, col - col % 3, num) &&
           grid[row][col] == 0;
}

// Function to find the next empty position in the grid
bool findEmptyPosition(int grid[SIZE][SIZE], int& row, int& col) {
    for (row = 0; row < SIZE; row++) {
        for (col = 0; col < SIZE; col++) {
            if (grid[row][col] == 0) {
                return true;
            }
        }
    }
    return false;
}

// Function to solve the Sudoku grid using backtracking
bool solveSudoku(int grid[SIZE][SIZE]) {
    int row, col;

    if (!findEmptyPosition(grid, row, col)) {
        // If no empty position left, puzzle is solved
        return true;
    }

    for (int num = 1; num <= 9; num++) {
        if (isSafe(grid, row, col, num)) {
            // Try placing the number in the current position
            grid[row][col] = num;

            // Recursively solve the puzzle
            if (solveSudoku(grid)) {
                return true;
            }

            // If the current number doesn't lead to a solution, backtrack
            grid[row][col] = 0;
        }
    }

    return false;
}

int main() {
    int grid[SIZE][SIZE] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    if (solveSudoku(grid)) {
        cout << "Sudoku Solution:" << endl;
        printGrid(grid);
    } else {
        cout << "No solution exists!" << endl;
    }

    return 0;
}
