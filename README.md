#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Contact {
    char name[50];
    char phone[15];
    char email[50];
    struct Contact* next;
    struct Contact* prev;
};

struct Contact* head = NULL;
struct Contact* tail = NULL;

void addContact() {
    struct Contact* newContact = (struct Contact*)malloc(sizeof(struct Contact));
    printf("Enter name: ");
    scanf(" %[^\n]", newContact->name);
    printf("Enter phone: ");
    scanf(" %[^\n]", newContact->phone);
    printf("Enter email: ");
    scanf(" %[^\n]", newContact->email);
    newContact->next = NULL;
    newContact->prev = tail;

    if (head == NULL) {
        head = tail = newContact;
    } else {
        tail->next = newContact;
        tail = newContact;
    }
    printf("Contact added!\n");
}

void displayContactsForward() {
    struct Contact* temp = head;
    if (temp == NULL) {
        printf("No contacts available.\n");
        return;
    }
    printf("\nContacts (Forward):\n");
    while (temp != NULL) {
        printf("Name: %s\nPhone: %s\nEmail: %s\n\n", temp->name, temp->phone, temp->email);
        temp = temp->next;
    }
}

void displayContactsBackward() {
    struct Contact* temp = tail;
    if (temp == NULL) {
        printf("No contacts available.\n");
        return;
    }
    printf("\nContacts (Backward):\n");
    while (temp != NULL) {
        printf("Name: %s\nPhone: %s\nEmail: %s\n\n", temp->name, temp->phone, temp->email);
        temp = temp->prev;
    }
}

void deleteContact() {
    char name[50];
    printf("Enter name to delete: ");
    scanf(" %[^\n]", name);
    struct Contact* temp = head;

    while (temp != NULL) {
        if (strcmp(temp->name, name) == 0) {
            if (temp->prev) temp->prev->next = temp->next;
            if (temp->next) temp->next->prev = temp->prev;
            if (temp == head) head = temp->next;
            if (temp == tail) tail = temp->prev;
            free(temp);
            printf("Contact deleted.\n");
            return;
        }
        temp = temp->next;
    }
    printf("Contact not found.\n");
}

void searchContact() {
    char name[50];
    printf("Enter name to search: ");
    scanf(" %[^\n]", name);
    struct Contact* temp = head;

    while (temp != NULL) {
        if (strcmp(temp->name, name) == 0) {
            printf("Found Contact:\nName: %s\nPhone: %s\nEmail: %s\n", temp->name, temp->phone, temp->email);
            return;
        }
        temp = temp->next;
    }
    printf("Contact not found.\n");
}

void updateContact() {
    char name[50];
    printf("Enter name to update: ");
    scanf(" %[^\n]", name);
    struct Contact* temp = head;

    while (temp != NULL) {
        if (strcmp(temp->name, name) == 0) {
            printf("Enter new phone: ");
            scanf(" %[^\n]", temp->phone);
            printf("Enter new email: ");
            scanf(" %[^\n]", temp->email);
            printf("Contact updated.\n");
            return;
        }
        temp = temp->next;
    }
    printf("Contact not found.\n");
}

int main() {
    int choice;
    do {
        printf("\n--- Contact Management ---\n");
        printf("1. Add Contact\n2. Delete Contact\n3. Update Contact\n4. Search Contact\n5. View Contacts Forward\n6. View Contacts Backward\n0. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1: addContact(); break;
            case 2: deleteContact(); break;
            case 3: updateContact(); break;
            case 4: searchContact(); break;
            case 5: displayContactsForward(); break;
            case 6: displayContactsBackward(); break;
            case 0: printf("Exiting...\n"); break;
            default: printf("Invalid choice.\n");
        }
    } while (choice != 0);

    return 0;
}
