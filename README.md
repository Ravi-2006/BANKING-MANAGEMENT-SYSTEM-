//# BANKING-MANAGEMENT-SYSTEM-
//ATM MACHINE USING C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct BankAccount {
    int accountNumber;
    char accountHolder[50];
    float balance;
};

struct BankAccount createAccount(int accountNumber, const char *accountHolder, float initialBalance) {
    struct BankAccount account;
    account.accountNumber = accountNumber;
    strcpy(account.accountHolder, accountHolder);
    account.balance = initialBalance;
    return account;
}

void displayAccount(const struct BankAccount *account) {
    printf("Account Number: %d\n", account->accountNumber);
    printf("Account Holder: %s\n", account->accountHolder);
    printf("Balance: $%.2f\n", account->balance);
}

void withdraw(struct BankAccount *account, float amount) {
    if (amount > 0 && amount <= account->balance) {
        account->balance -= amount;
        printf("Withdrawal successful. New balance: $%.2f\n", account->balance);
    } else {
        printf("Invalid withdrawal amount or insufficient balance.\n");
    }
}

void deposit(struct BankAccount *account, float amount) {
    if (amount > 0) {
        account->balance += amount;
        printf("Deposit successful. New balance: $%.2f\n", account->balance);
    } else {
        printf("Invalid deposit amount.\n");
    }
}

void transfer(struct BankAccount *sender, struct BankAccount *receiver, float amount) {
    if (amount > 0 && amount <= sender->balance) {
        sender->balance -= amount;
        receiver->balance += amount;
        printf("Transfer successful. New balance for %s: $%.2f\n", sender->accountHolder, sender->balance);
        printf("New balance for %s: $%.2f\n", receiver->accountHolder, receiver->balance);
    } else {
        printf("Invalid transfer amount or insufficient balance in sender's account.\n");
    }
}

void updateAccount(struct BankAccount *account, const char *newAccountHolder) {
    strcpy(account->accountHolder, newAccountHolder);
    printf("Account details updated successfully. New account holder: %s\n", account->accountHolder);
}

void saveAccountsToFile(struct BankAccount *accounts, int numAccounts) {
    FILE *fptr;
    fptr = fopen("bankaccounts.txt", "w");
    if (fptr == NULL) {
        printf("Error opening file for writing.\n");
        return;
    }

    for (int i = 0; i < numAccounts; i++) {
        fprintf(fptr, "%d %s %.2f\n", accounts[i].accountNumber, accounts[i].accountHolder, accounts[i].balance);
    }

    fclose(fptr);
}

int loadAccountsFromFile(struct BankAccount *accounts) {
    FILE *fptr;
    fptr = fopen("bankaccounts.txt", "r");
    if (fptr == NULL) {
        printf("No existing account data found.\n");
        return 0;
    }

    int numAccounts = 0;
    while (fscanf(fptr, "%d %s %f", &accounts[numAccounts].accountNumber, accounts[numAccounts].accountHolder, &accounts[numAccounts].balance) != EOF) {
        numAccounts++;
    }

    fclose(fptr);
    return numAccounts;
}

void rating(int s) {
    if (s < 1 || s > 5) {
        printf("Invalid rating. Please rate between 1 and 5 stars.\n");
    } else {
        printf("You rated %d stars: ", s);
        for (int i = 0; i < s; i++) {
            printf("* ");
        }
        if (s <= 3) {
            printf("Thanks for your feedback.\n");
        } else {
            printf("Thanks for your positive rating!\n");
        }
    }
}

int authenticateUser(const char *username, const char *password) {
    FILE *fptr;
    fptr = fopen("accounts.txt", "r");
    if (fptr == NULL) {
        printf("Error in existence.\n");
        return 0;
    }
    char line[50 + 50 + 2];
    while (fgets(line, sizeof(line), fptr)) {
        char exist_username[50];
        char exist_password[50];
        sscanf(line, "%s %s", exist_username, exist_password);
        if (strcmp(username, exist_username) == 0 && strcmp(password, exist_password) == 0) {
            fclose(fptr);
            return 1;
        }
    }
    fclose(fptr);
    return 0;
}

int main() {
    struct BankAccount accounts[10];
    int numAccounts = 0;

    char username[50];
    char password[50];

    printf("\n\n\n");
    printf("\t\tWelcome to The User Authentication Program:\n");
    printf("\t\tYou are requested to enter valid credentials to access the program\n");
    printf("\t\t\t\tElse No Access Would Be Given\n\n");
    printf("Enter the username: ");
    scanf(" %s", username);
    printf("Enter the password: ");
    scanf(" %s", password);

    if (!authenticateUser(username, password)) {
        printf("\n\t\tAuthentication Failed!!\n\n");
        return 1;
    }

    printf("\n\t\t\t\t\tWelcome, %s\n\n", username);

    numAccounts = loadAccountsFromFile(accounts);

    int choice;
    do {
        printf("\n");
        printf("\t\t\t\t\t\tWelcome to Agarwal Bank\n");
        printf("\t\t\t\t\tBanking Made Simple and Hassle Free\n");
        printf("\t\tProgram especially belongs to staff and customer is instructed to use their own program\n");
        printf("\t\t\t\t\tEnter the valid inputs as given below:\n\n");
        printf("\n");

        printf("\t\t\t\t1. Create New Account\n");
        printf("\t\t\t\t2. Update Account Details\n");
        printf("\t\t\t\t3. Check Account Details\n");
        printf("\t\t\t\t4. Withdraw Money\n");
        printf("\t\t\t\t5. Deposit Money\n");
        printf("\t\t\t\t6. Transfer Money\n");
        printf("\t\t\t\t7. Exit\n");
        printf("\t\t\t\tEnter your choice: ");
        scanf("%d", &choice);
        while (getchar() != '\n'); // Clear the input buffer

        switch (choice) {
            case 1: {
                // Call the function to create a new account
                int accountNumber;
                char accountHolder[50];
                float initialBalance;

                printf("\nEnter Account Number: ");
                scanf("%d", &accountNumber);
                while (getchar() != '\n'); // Clear the input buffer

                printf("Enter Account Holder: ");
                fgets(accountHolder, sizeof(accountHolder), stdin);
                accountHolder[strcspn(accountHolder, "\n")] = '\0'; // Remove the newline character

                printf("Enter Initial Balance: $");
                scanf("%f", &initialBalance);

                accounts[numAccounts] = createAccount(accountNumber, accountHolder, initialBalance);
                numAccounts++;
                printf("Account created successfully!\n");
                break;
            }
            case 2: {
                // Call the function to update account details
                int accountNum;
                char newAccountHolder[50];

                printf("Enter Account Number to Update: ");
                scanf("%d", &accountNum);
                while (getchar() != '\n'); // Clear the input buffer

                printf("Enter New Account Holder Name: ");
                fgets(newAccountHolder, sizeof(newAccountHolder), stdin);
                newAccountHolder[strcspn(newAccountHolder, "\n")] = '\0'; // Remove the newline character

                int found = 0;
                for (int i = 0; i < numAccounts; i++) {
                    if (accounts[i].accountNumber == accountNum) {
                        updateAccount(&accounts[i], newAccountHolder);
                        found = 1;
                        break;
                    }
                }
                if (!found) {
                    printf("Account not found!\n");
                } else {
                    printf("Account details updated successfully!\n");
                }
                break;
            }
            case 3: {
                // Call the function to display account details
                int accountNum;

                printf("Enter Account Number to Check Details: ");
                scanf("%d", &accountNum);

                int found = 0;
                for (int i = 0; i < numAccounts; i++) {
                    if (accounts[i].accountNumber == accountNum) {
                        displayAccount(&accounts[i]);
                        found = 1;
                        break;
                    }
                }
                if (!found) {
                    printf("Account not found!\n");
                }
                break;
            }
            case 4: {
                // Call the function to withdraw money
                int accountNum;
                float amount;

                printf("Enter Account Number to Withdraw From: ");
                scanf("%d", &accountNum);
                printf("Enter Withdrawal Amount: $");
                scanf("%f", &amount);

                int found = 0;
                for (int i = 0; i < numAccounts; i++) {
                    if (accounts[i].accountNumber == accountNum) {
                        withdraw(&accounts[i], amount);
                        found = 1;
                        break;
                    }
                }
                if (!found) {
                    printf("Account not found!\n");
                }
                break;
            }
            case 5: {
                // Call the function to deposit money
                int accountNum;
                float amount;

                printf("Enter Account Number to Deposit To: ");
                scanf("%d", &accountNum);
                printf("Enter Deposit Amount: $");
                scanf("%f", &amount);

                int found = 0;
                for (int i = 0; i < numAccounts; i++) {
                    if (accounts[i].accountNumber == accountNum) {
                        deposit(&accounts[i], amount);
                        found = 1;
                        break;
                    }
                }
                if (!found) {
                    printf("Account not found!\n");
                }
                break;
            }
            case 6: {
                // Call the function to transfer money
                int senderAccountNum, receiverAccountNum;
                float amount;

                printf("Enter Sender's Account Number: ");
                scanf("%d", &senderAccountNum);
                printf("Enter Receiver's Account Number: ");
                scanf("%d", &receiverAccountNum);
                printf("Enter Transfer Amount: $");
                scanf("%f", &amount);

                int senderFound = 0, receiverFound = 0;
                for (int i = 0; i < numAccounts; i++) {
                    if (accounts[i].accountNumber == senderAccountNum) {
                        senderFound = 1;
                    }
                    if (accounts[i].accountNumber == receiverAccountNum) {
                        receiverFound = 1;
                    }
                    if (senderFound && receiverFound) {
                        break;
                    }
                }
                if (senderFound && receiverFound) {
                    for (int i = 0; i < numAccounts; i++) {
                        if (accounts[i].accountNumber == senderAccountNum) {
                            withdraw(&accounts[i], amount);
                        }
                        if (accounts[i].accountNumber == receiverAccountNum) {
                            deposit(&accounts[i], amount);
                        }
                    }
                    printf("Transfer successful!\n");
                } else {
                    printf("Invalid account numbers!\n");
                }
                break;
            }
            case 7: {
                // Save account information to file before exiting
                saveAccountsToFile(accounts, numAccounts);
                printf("\t\t\t\tExiting the program. Have a nice day!\n");
                printf("\t\t\t\tPlease rate the program between 1 and 5 stars: ");
                int s;
                scanf("%d", &s);
                rating(s);
                break;
            }
            default: {
                printf("\t\t\t\tInvalid choice. Please try again.\n");
                break;
            }
        }
    } while (choice != 7);

    return 0;
}
