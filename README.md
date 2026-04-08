#include <iostream>
#include <vector>
#include <fstream>
#include <string>
using namespace std;


int N = 28;
int DONE;
int total_solutions = 0;
ofstream outFile;
int current_file_index = 1;
const int LIMIT_PER_FILE = 10000;
void printBoard(const vector<int>& queens) {
    total_solutions++;
    if ((total_solutions - 1) % LIMIT_PER_FILE == 0) {
        if (outFile.is_open()) {
            outFile.close();
        }
        string filename = "N-queens[a(28)]" + to_string(current_file_index) + ".txt";
        outFile.open(filename);
        cout << "\n========================================================\n";
        cout << ">>> DA DAY GIOI HAN! CHUYEN SANG GHI FILE: " << filename << " <<<\n";
        cout << "========================================================\n\n";
        current_file_index++;
    }


    cout << "Cach dat thu " << total_solutions << "\n";
    outFile << "Cach dat thu " << total_solutions << "\n";
   
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            if (queens[i] == j) {
                cout << "Q";
                outFile << "Q";
            } else {
                cout << " . ";
                outFile << " . ";
            }
        }
        cout << "\n";
        outFile << "\n";
    }
    cout << "\n";
    outFile << "\n";
}


void solve(int cols, int ld, int rd, vector<int>& queens, int row) {
    if (cols == DONE) {
        printBoard(queens);
        return;
    }


    int avail = ~(cols | ld | rd) & DONE;


    while (avail > 0) {
        int p = avail & -avail;
        avail ^= p;


        int col_index = __builtin_ctz(p);
        queens[row] = col_index;


        solve(cols | p, (ld | p) << 1, (rd | p) >> 1, queens, row + 1);
    }
}


int main() {
    DONE = (1 << N) - 1;
    vector<int> queens(N, 0);
   
    cout << "Bat dau tim kiem cac the co cho " << N << " quan hau...\n";
   
    solve(0, 0, 0, queens, 0);
    if (outFile.is_open()) {
        outFile.close();
    }
   
    cout << "Hoan tat! Da tim thay " << total_solutions << " cach dat.\n";
    cout << "Du lieu da duoc chia thanh " << current_file_index - 1 << " file (toi da " << LIMIT_PER_FILE << " cach/file).\n";
   
    return 0;
}

