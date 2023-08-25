#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};

typedef struct Node Node;

Node* newNode(int data) {
    Node* node = (Node*)malloc(sizeof(Node));
    node->data = data;
    node->next = NULL;
    return node;
}

Node* addLists(Node* list1, Node* list2) {
    Node* result = NULL;
    Node* prev = NULL;
    Node* temp;
    int carry = 0, sum;

    while (list1 != NULL || list2 != NULL) {
        sum = carry + (list1 ? list1->data : 0) + (list2 ? list2->data : 0);
        carry = sum / 10;
        sum %= 10;

        temp = newNode(sum);

        if (result == NULL) {
            result = temp;
        } else {
            prev->next = temp;
        }

        prev = temp;

        if (list1) list1 = list1->next;
        if (list2) list2 = list2->next;
    }

    if (carry > 0) {
        temp->next = newNode(carry);
    }

    return result;
}

void printList(Node* list) {
    while (list != NULL) {
        printf("%d", list->data);
        list = list->next;
    }
    printf("\n");
}

int main() {
    Node* num1 = newNode(9);
    num1->next = newNode(9);
    num1->next->next = newNode(9);

    Node* num2 = newNode(1);

    Node* result = addLists(num1, num2);

    printf("Number 1: ");
    printList(num1);

    printf("Number 2: ");
    printList(num2);

    printf("Sum: ");
    printList(result);

    return 0;
}
#include <stdio.h>

void rotateArray(int arr[], int n, int k) {
    int temp[k];

    // Store the first k elements in a temporary array
    for (int i = 0; i < k; i++) {
        temp[i] = arr[i];
    }

    // Shift the remaining elements to the left by k positions
    for (int i = k; i < n; i++) {
        arr[i - k] = arr[i];
    }

    // Copy the elements from the temporary array back to the original array
    for (int i = 0; i < k; i++) {
        arr[n - k + i] = temp[i];
    }
}

int main() {
    int arr[] = {1, 2, 3, 4, 5, 6, 7};
    int n = sizeof(arr) / sizeof(arr[0]);
    int k = 3;

    rotateArray(arr, n, k);

    printf("Rotated array: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }

    return 0;
}
#include <stdio.h>

void rotateMatrix(int n, int mat[n][n]) {
    for (int i = 0; i < n / 2; i++) {
        for (int j = i; j < n - i - 1; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[n - j - 1][i];
            mat[n - j - 1][i] = mat[n - i - 1][n - j - 1];
            mat[n - i - 1][n - j - 1] = mat[j][n - i - 1];
            mat[j][n - i - 1] = temp;
        }
    }
}

void printMatrix(int n, int mat[n][n]) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("%d ", mat[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int n = 4; // Change this to the desired size of the matrix
    int mat[4][4] = {{1, 2, 3, 4},
                    {5, 6, 7, 8},
                    {9, 10, 11, 12},
                    {13, 14, 15, 16}};

    printf("Original Matrix:\n");
    printMatrix(n, mat);

    rotateMatrix(n, mat);

    printf("\nMatrix after rotation:\n");
    printMatrix(n, mat);

    return 0;
}
#include <stdio.h>
#include <stdlib.h>
struct Node {
    int coefficient;
    int exponent;
    struct Node* next;
};
struct Node* createNode(int coeff, int exp) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->coefficient = coeff;
    newNode->exponent = exp;
    newNode->next = NULL;
    return newNode;
}
void insertTerm(struct Node** poly, int coeff, int exp) {
    struct Node* newNode = createNode(coeff, exp);
    if (*poly == NULL) {
        *poly = newNode;
    } else {
        struct Node* current = *poly;
        while (current->next != NULL) {
            current = current->next;
        }
        current->next = newNode;
    }
}
struct Node* addPolynomials(struct Node* poly1, struct Node* poly2) {
    struct Node* result = NULL;
    while (poly1 != NULL || poly2 != NULL) {
        int coeff1 = (poly1 != NULL) ? poly1->coefficient : 0;
        int coeff2 = (poly2 != NULL) ? poly2->coefficient : 0;
        int exp1 = (poly1 != NULL) ? poly1->exponent : 0;
        int exp2 = (poly2 != NULL) ? poly2->exponent : 0;
        if (exp1 == exp2) {
            insertTerm(&result, coeff1 + coeff2, exp1);
            poly1 = poly1->next;
            poly2 = poly2->next;
        } else if (exp1 > exp2) {
            insertTerm(&result, coeff1, exp1);
            poly1 = poly1->next;
        } else {
            insertTerm(&result, coeff2, exp2);
            poly2 = poly2->next;
        }
    }

    return result;
}
void displayPolynomial(struct Node* poly) {
    struct Node* current = poly;
    while (current != NULL) {
        printf("%dx^%d", current->coefficient, current->exponent);
        current = current->next;
        if (current != NULL) {
            printf(" + ");
        }
    }
    printf("\n");
}
int main() {
    struct Node* poly1 = NULL;
    struct Node* poly2 = NULL;
    insertTerm(&poly1, 5, 2);
    insertTerm(&poly1, 4, 1);
    insertTerm(&poly1, 2, 0);
    insertTerm(&poly2, 3, 2);
    insertTerm(&poly2, 2, 1);
    insertTerm(&poly2, 1, 0);
    printf("First Polynomial: ");
    displayPolynomial(poly1);
    printf("Second Polynomial: ");
    displayPolynomial(poly2);
    struct Node* result = addPolynomials(poly1, poly2);
    printf("Sum of Polynomials: ");
    displayPolynomial(result);
    return 0;
}
#include <stdio.h>
#include <stdlib.h>
struct Node {
    int coefficient;
    int exponent;
    struct Node* next;
};
struct Node* createNode(int coeff, int exp) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->coefficient = coeff;
    newNode->exponent = exp;
    newNode->next = NULL;
    return newNode;
}
void insertTerm(struct Node** poly, int coeff, int exp) {
    struct Node* newNode = createNode(coeff, exp);
    if (*poly == NULL) {
        *poly = newNode;
    } else {
        struct Node* current = *poly;
        while (current->next != NULL) {
            current = current->next;
        }
        current->next = newNode;
    }
}
struct Node* multiplyPolynomials(struct Node* poly1, struct Node* poly2) {
    struct Node* result = NULL;
    while (poly1 != NULL) {
        struct Node* temp = poly2;
        while (temp != NULL) {
            int coeff = poly1->coefficient * temp->coefficient;
            int exp = poly1->exponent + temp->exponent;
            insertTerm(&result, coeff, exp);
            temp = temp->next;
        }
        poly1 = poly1->next;
    }
    return result;
}
void displayPolynomial(struct Node* poly) {
    struct Node* current = poly;
    while (current != NULL) {
        printf("%dx^%d", current->coefficient, current->exponent);
        current = current->next;
        if (current != NULL) {
            printf(" + ");
        }
    }
    printf("\n");
}
void freePolynomial(struct Node* poly) {
    struct Node* current = poly;
    while (current != NULL) {
        struct Node* temp = current;
        current = current->next;
        free(temp);
    }
}
int main() {
    struct Node* poly1 = NULL;
    struct Node* poly2 = NULL;
    int n, coeff, exp;
    printf("Enter the number of terms in the first polynomial: ");
    scanf("%d", &n);
    printf("Enter the terms of the first polynomial (coeff exp): ");
    for (int i = 0; i < n; i++) {
        scanf("%d %d", &coeff, &exp);
        insertTerm(&poly1, coeff, exp);
    }
    printf("Enter the number of terms in the second polynomial: ");
    scanf("%d", &n);
    printf("Enter the terms of the second polynomial (coeff exp): ");
    for (int i = 0; i < n; i++) {
        scanf("%d %d", &coeff, &exp);
        insertTerm(&poly2, coeff, exp);
    }
    printf("First Polynomial: ");
    displayPolynomial(poly1);
    printf("Second Polynomial: ");
    displayPolynomial(poly2);
    struct Node* result = multiplyPolynomials(poly1, poly2);
    printf("Product of Polynomials: ");
    displayPolynomial(result);
    freePolynomial(poly1);
    freePolynomial(poly2);
    freePolynomial(result);
    return 0;
}
