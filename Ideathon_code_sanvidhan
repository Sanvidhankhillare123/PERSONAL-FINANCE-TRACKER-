#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Record {
    char name[50];
    char date[20];
    double amount;
    char type[20];
    struct Record *next; // Pointer to the next record with the same date
    struct Record *left;
    struct Record *right;
} Record;

// Function to create a new record node
Record* createRecord(char name[], char date[], double amount, char type[]) {
    Record *newNode = (Record*)malloc(sizeof(Record));
    if (newNode == NULL) {
        printf("Memory allocation failed!\n");
        exit(EXIT_FAILURE);
    }
    strcpy(newNode->name, name);
    strcpy(newNode->date, date);
    newNode->amount = amount;
    strcpy(newNode->type, type);
    newNode->next = NULL;
    newNode->left = newNode->right = NULL;
    return newNode;
}

// Function to insert or update record into BST
Record* insertOrUpdateRecord(Record *root, char name[], char date[], double amount, char type[]) {
    if (root == NULL) {
        return createRecord(name, date, amount, type);
    }

    // Compare the date
    int cmpDate = strcmp(date, root->date);

    // Insert or update the record into the binary search tree
    if (cmpDate < 0) {
        root->left = insertOrUpdateRecord(root->left, name, date, amount, type);
    } else if (cmpDate > 0) {
        root->right = insertOrUpdateRecord(root->right, name, date, amount, type);
    } else {
        // If the date matches, check if the type matches
        int cmpType = strcmp(type, root->type);
        if (cmpType == 0) {
            // If the type matches, update the amount
            root->amount += amount;
        } else {
            // If the type doesn't match, add the record to the linked list
            Record *newRecord = createRecord(name, date, amount, type);
            newRecord->next = root->next;
            root->next = newRecord;
        }
    }

    return root;
}

// Function to delete expense records by date and type
Record* deleteExpenseRecordByDateAndType(Record *root, char date[], char type[]) {
    if (root == NULL) {
        return NULL;
    }

    // Compare the date
    int cmpDate = strcmp(date, root->date);

    if (cmpDate == 0) {
        // If the date matches, traverse the linked list to find and delete the record with matching type
        Record *prev = NULL;
        Record *current = root;
        while (current != NULL) {
            if (strcmp(current->type, type) == 0) {
                // Delete the record
                if (prev == NULL) {
                    // If the record to delete is the first node in the linked list
                    root = current->next;
                    free(current);
                    return root;
                } else {
                    // If the record to delete is not the first node
                    prev->next = current->next;
                    free(current);
                    return root;
                }
            }
            prev = current;
            current = current->next;
        }
    } else if (cmpDate < 0) {
        root->left = deleteExpenseRecordByDateAndType(root->left, date, type);
    } else {
        root->right = deleteExpenseRecordByDateAndType(root->right, date, type);
    }

    return root;
}

// Function to view records (in-order traversal)
void viewRecords(Record *root, const char* category) {
    if (root != NULL) {
        viewRecords(root->left, category);
        
        // Print all records in the linked list
        Record *current = root;
        while (current != NULL) {
            printf("| %-13s| %-11s| %-20s| %-10s| %.2lf        |\n", current->name, current->date, current->type, category, current->amount);
            current = current->next;
        }
        
        viewRecords(root->right, category);
    }
}

// Function to free memory allocated for BST nodes
void freeTree(Record *root) {
    if (root != NULL) {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}

// Function to search for expense records by date
void searchExpenseByDate(Record *root, char date[]) {
    if (root == NULL) {
        printf("No expense record found for the given date.\n");
        return;
    }

    int cmpDate = strcmp(date, root->date);
    if (cmpDate == 0) {
        // If the date matches, traverse the linked list to print all records
        Record *current = root;
        while (current != NULL) {
            printf("| %-13s| %-11s| %-20s| %-10s| %.2lf        |\n", current->name, current->date, current->type, "Expense", current->amount);
            current = current->next;
        }
    } else if (cmpDate < 0) {
        searchExpenseByDate(root->left, date);
    } else {
        searchExpenseByDate(root->right, date);
    }
}

// Function to calculate total amount in the records
double calculateTotal(Record *root) {
    if (root == NULL) {
        return 0.0;
    }
    double total = 0.0;
    // Traverse the BST and add up amounts
    total += calculateTotal(root->left);
    Record *current = root;
    while (current != NULL) {
        total += current->amount;
        current = current->next;
    }
    total += calculateTotal(root->right);
    return total;
}

// Function to retrieve all expense records up to a given date
void getExpenseRecordsUpToDate(Record *root, char date[]) {
    if (root != NULL) {
        getExpenseRecordsUpToDate(root->left, date);

        int cmpDate = strcmp(date, root->date);
        if (cmpDate >= 0) {
            // If the current node's date is less than or equal to the target date, print all expense records up to this node
            Record *current = root;
            while (current != NULL) {
                printf("| %-13s| %-11s| %-20s| %-10s| %.2lf        |\n", current->name, current->date, current->type, "Expense", current->amount);
                current = current->next;
            }
        }

        getExpenseRecordsUpToDate(root->right, date);
    }
}

int main() {
    Record *incomeRoot = NULL;
    Record *expenseRoot = NULL;
    int option;
    double amount;
    char name[50], date[20], type[20];

    do {
        printf("\n1. Insert Income\n");
        printf("2. Insert or Update Expense\n");
        printf("3. Delete Expense Record by Date and Type\n");
        printf("4. View Income Records\n");
        printf("5. View Expense Records\n");
        printf("6. View Net Balance\n");
        printf("7. Get Expense Records up to a Given Date\n");
        printf("8. Exit\n");
        printf("Enter your option: ");
        scanf("%d", &option);

        switch (option) {
            case 1:
                printf("Enter name: ");
                scanf("%s", name);
                printf("Enter date (DD/MM/YYYY): ");
                scanf("%s", date);
                printf("Enter income amount: ");
                scanf("%lf", &amount);
                incomeRoot = insertOrUpdateRecord(incomeRoot, name, date, amount, "");
                break;
            case 2:
                printf("Enter name: ");
                scanf("%s", name);
                printf("Enter date (DD/MM/YYYY): ");
                scanf("%s", date);
                printf("Enter expense amount: ");
                scanf("%lf", &amount);
                printf("Enter type of expense: ");
                scanf("%s", type);
                expenseRoot = insertOrUpdateRecord(expenseRoot, name, date, amount, type);
                break;
            case 3:
                printf("Enter date (DD/MM/YYYY) of the expense record to delete: ");
                scanf("%s", date);
                printf("Enter type of the expense record to delete: ");
                scanf("%s", type);
                expenseRoot = deleteExpenseRecordByDateAndType(expenseRoot, date, type);
                break;
            case 4:
                printf("+-------------------------------------------------------------------------------------+\n");
                printf("|               VIEW INCOME RECORDS                                                   |\n");
                printf("+-------------------------------------------------------------------------------------+\n");
                viewRecords(incomeRoot, "Income");
                printf("+-------------------------------------------------------------------------------------+\n");
                break;
            case 5:
                printf("Enter date (DD/MM/YYYY) to view expense records: ");
                scanf("%s", date);
                printf("+-------------------------------------------------------------------------------------+\n");
                printf("|               VIEW EXPENSE RECORDS                                                  |\n");
                printf("+-------------------------------------------------------------------------------------+\n");
                searchExpenseByDate(expenseRoot, date);
                printf("+-------------------------------------------------------------------------------------+\n");
                break;
            case 6:
                printf("Net Balance: %.2lf\n", calculateTotal(incomeRoot) - calculateTotal(expenseRoot));
                break;
            case 7:
                printf("Enter date (DD/MM/YYYY) to get expense records up to: ");
                scanf("%s", date);
                printf("+-------------------------------------------------------------------------------------+\n");
                printf("|               GET EXPENSE RECORDS UP TO DATE                                         |\n");
                printf("+-------------------------------------------------------------------------------------+\n");
                getExpenseRecordsUpToDate(expenseRoot, date);
                printf("+-------------------------------------------------------------------------------------+\n");
                break;
            case 8:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid option!\n");
        }
    } while (option != 8);

    // Free allocated memory before exiting
    freeTree(incomeRoot);
    freeTree(expenseRoot);

    return 0;
}
