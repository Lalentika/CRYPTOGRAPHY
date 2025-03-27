# CRYPTOGRAPHY
HILL CIPHER
EX. NO: 1(C) AIM:
 

IMPLEMENTATION OF HILL CIPHER
 
## To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used
 for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:

STEP-1: Read the plain text and key from the user. STEP-2: Split the plain text into groups of length three. STEP-3: Arrange the keyword in a 3*3 matrix.
STEP-4: Multiply the two matrices to obtain the cipher text of length three.
STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM 
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MOD 26

// Function to compute determinant of a matrix
int determinant(int matrix[3][3], int n) {
    int det = 0;
    if (n == 2) {
        return (matrix[0][0] * matrix[1][1] - matrix[0][1] * matrix[1][0]);
    }
    for (int p = 0; p < n; p++) {
        int temp[3][3], subi = 0;
        for (int i = 1; i < n; i++) {
            int subj = 0;
            for (int j = 0; j < n; j++) {
                if (j == p) continue;
                temp[subi][subj] = matrix[i][j];
                subj++;
            }
            subi++;
        }
        det = det + (p % 2 == 0 ? 1 : -1) * matrix[0][p] * determinant(temp, n - 1);
    }
    return det;
}

// Function to find modular inverse of a number under modulo MOD
int modInverse(int a) {
    a = a % MOD;
    for (int x = 1; x < MOD; x++) {
        if ((a * x) % MOD == 1) {
            return x;
        }
    }
    return -1;
}

// Function to find inverse of a matrix modulo 26
void inverseMatrix(int matrix[3][3], int n, int inverse[3][3]) {
    int det = determinant(matrix, n) % MOD;
    if (det < 0) det += MOD;
    int detInv = modInverse(det);
    if (detInv == -1) {
        printf("Matrix is not invertible.\n");
        exit(1);
    }
    int temp[3][3];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int subMatrix[3][3], subi = 0;
            for (int x = 0; x < n; x++) {
                if (x == i) continue;
                int subj = 0;
                for (int y = 0; y < n; y++) {
                    if (y == j) continue;
                    subMatrix[subi][subj] = matrix[x][y];
                    subj++;
                }
                subi++;
            }
            temp[j][i] = ((determinant(subMatrix, n - 1) % MOD + MOD) * ( (i + j) % 2 == 0 ? 1 : -1)) % MOD;
        }
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            inverse[i][j] = (temp[i][j] * detInv) % MOD;
            if (inverse[i][j] < 0) inverse[i][j] += MOD;
        }
    }
}

// Function to encrypt plaintext
void encrypt(int key[3][3], char message[], int n) {
    int len = strlen(message);
    for (int i = 0; i < len; i += n) {
        int vec[3] = {0};
        for (int j = 0; j < n; j++) vec[j] = message[i + j] - 'A';
        int result[3] = {0};
        for (int x = 0; x < n; x++) {
            for (int y = 0; y < n; y++) {
                result[x] += key[x][y] * vec[y];
            }
            result[x] %= MOD;
        }
        for (int j = 0; j < n; j++) message[i + j] = result[j] + 'A';
    }
}

// Function to decrypt ciphertext
void decrypt(int key[3][3], char message[], int n) {
    int inverse[3][3];
    inverseMatrix(key, n, inverse);
    encrypt(inverse, message, n);
}

int main() {
    int key[3][3] = {
        {6, 24, 1},
        {13, 16, 10},
        {20, 17, 15}
    };
    char message[] = "ACT";
    int n = 3;
    printf("Original message: %s\n", message);
    encrypt(key, message, n);
    printf("Encrypted message: %s\n", message);
    decrypt(key, message, n);
    printf("Decrypted message: %s\n", message);
    return 0;
}

## OUTPUT

![image](https://github.com/user-attachments/assets/70a77116-dcfd-462f-8bd7-d17e42730a91)

## RESULT

Implemented the hill cipher substitution code successfully

