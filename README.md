# To-do-list-app
I create a to do list app using c programming language

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char *Todotask;
    int completed;
} Task;

Task *Todotasks = NULL;
int len = 0;

void addTodotask(const char *Todotask) {
    Todotasks = (Task *)realloc(Todotasks, (len + 1) * sizeof(Task));
    Todotasks[len].Todotask = (char *)malloc(strlen(Todotask) + 1);
    strcpy(Todotasks[len].Todotask, Todotask);
    Todotasks[len].completed = 0;
    len++;
    printf("Task added\n");
}

void listTodotasks() {
    char status;
    for (int i = 0; i < len; i++) {
        if (Todotasks[i].completed == 1) {
            status = 'd';
        } else {
            status = 'n';
        }
        printf("%d. %s [%c]\n", i + 1, Todotasks[i].Todotask, status);
    }
}

void markcompleted(int index) {
    if (index <= len && index > 0) {
        Todotasks[index - 1].completed = 1;
        printf("Task marked as completed\n");
    } else {
        printf("Invalid index\n");
    }
}

void removeTodotask(int index) {
    if (index <= len && index > 0) {
        index = index - 1;
        free(Todotasks[index].Todotask);
        for (int i = index; i < len - 1; i++) {
            Todotasks[i] = Todotasks[i + 1];
        }
        len--;
        Todotasks = (Task *)realloc(Todotasks, len * sizeof(Task));
        printf("Task removed\n");
    } else {
        printf("Invalid index\n");
    }
}

void editTodotask(int index, const char *todotask) {
    if (index <= len && index > 0) {
        index = index - 1;
        char *editedTodotask = (char *)realloc(Todotasks[index].Todotask, strlen(todotask) + 1);
        if (editedTodotask != NULL) {
            Todotasks[index].Todotask = editedTodotask;
            strcpy(Todotasks[index].Todotask, todotask);
            printf("Task updated\n");
        } else {
            printf("Memory allocation failed\n");
        }
    } else {
        printf("Invalid index\n");
    }
}

void freeTodotask() {
    for (int i = 0; i < len; i++) {
        free(Todotasks[i].Todotask);
    }
    free(Todotasks);
}

int main() {
    int choice;
    int indexinput;
    char taskinput[100];
    int running = 1;

    printf("\nOptions:\n");
    printf("1. Add Todotask\n");
    printf("2. List all Todotasks\n");
    printf("3. Mark Todotask as complete\n");
    printf("4. Remove Todotask\n");
    printf("5. Edit Todotask\n");
    printf("6. Exit\n");

    while (running) {
        printf("Enter your choice (1-6): ");
        scanf("%d", &choice);
        getchar(); // Consume newline left in input buffer

        switch (choice) {
            case 1:
                printf("Enter Todotask: ");
                fgets(taskinput, sizeof(taskinput), stdin);
                taskinput[strcspn(taskinput, "\n")] = '\0';
                addTodotask(taskinput);
                break;

            case 2:
                listTodotasks();
                break;

            case 3:
                printf("Enter index of Todotask: ");
                scanf("%d", &indexinput);
                markcompleted(indexinput);
                break;

            case 4:
                printf("Enter index of Todotask: ");
                scanf("%d", &indexinput);
                removeTodotask(indexinput);
                break;

            case 5:
                printf("Enter index of Todotask: ");
                scanf("%d", &indexinput);
                getchar(); // Consume newline left in input buffer
                printf("Enter edited Todotask: ");
                fgets(taskinput, sizeof(taskinput), stdin);
                taskinput[strcspn(taskinput, "\n")] = '\0';
                editTodotask(indexinput, taskinput);
                break;

            case 6:
                running = 0;
                break;

            default:
                printf("Invalid choice\n");
                break;
        }
    }

    freeTodotask();
    return 0;
}
