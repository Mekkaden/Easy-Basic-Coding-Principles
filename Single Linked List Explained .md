 What is a Linked List (Singly)?

A singly linked list is like a train. Each coach (node) has:

    Some data

    A link (pointer) to the next coach

There’s no backward movement — it only moves forward (like a singly chain).
 THE BIG PICTURE OF THESE 3 FILES:

    linkedlist.h → Contains struct definitions + function declarations.

    linkedlist.c → Contains actual logic (function definitions).

    main() function part → Contains the logic that runs the program.

Now let’s dive file-by-file, line-by-line, bro.
 FILE 1: linkedlist.h – The Header File

This file sets up definitions for your program. Think of it as a blueprint 📘.
Line-by-line:

#include<stdlib.h>

    Includes the Standard Library.

    Needed for things like malloc() and exit() functions.

#define MALLOC(p,s) \
    if(!((p)=malloc(s))) \
    { \
        fprintf(stderr,"Insufficient memory"); \
        exit(EXIT_FAILURE); \
    }

    This is a macro — like a shortcut/template.

    It ensures memory allocation is safe:

        (p)=malloc(s) tries to allocate memory of size s.

        If malloc fails (returns NULL), it prints an error and exits.

    stderr is where errors are printed.

    exit(EXIT_FAILURE) quits the program.



    --------------------------------------------------------------------------------------------------------------------------
    Line 11-14 (struct listNode): This is the most important part. It defines the structure of a single "node" or "link" in your chain.

    struct: A keyword in C that lets you create a complex data type by grouping variables.

    int data: The actual value you want to store in this node (e.g., the number 5, 10, etc.).

    listPointer link: This is the magic. link is a pointer. It stores the memory address of the next listNode in the chain. Because it points to a structure of its own type, it's called a self-referential structure. This is what allows you to link nodes together.

Line 10 (typedef ...): typedef is a keyword that lets you create a nickname or an alias for a data type.

    Here, typedef struct listNode *listPointer; means "from now on, instead of typing the long struct listNode *, I can just type the shorter listPointer". It makes the code much cleaner.

    --------------------------------------------------------------------------------------------------------------------------
    ADD LIST


    This is Self contained add list code : 

    #include <stdio.h>
#include <stdlib.h>

// Helper macro for safe memory allocation
#define MALLOC(p, s) if (!((p) = malloc(s))) { \
    fprintf(stderr, "Insufficient memory"); \
    exit(EXIT_FAILURE); \
}

// Structure for a single node
typedef struct listNode {
    int data;
    struct listNode *link;
} listNode;


void addToList(listPointer *first, int item_data) {
    listPointer ptr, temp;

    // 1. Create a new node and allocate memory for it.
    MALLOC(temp, sizeof(listNode));
    temp->data = item_data; // Set the data for the new node.
    temp->link = NULL;      // The new node will be the last, so its link is NULL.

    // 2. Check if the list is empty.
    if (*first) {
        // If the list is NOT empty, find the last node.
        // We start 'ptr' at the beginning of the list.
        // The loop continues as long as the current node has a link to another node.
        for (ptr = *first; ptr->link != NULL; ptr = ptr->link);
        
        // 'ptr' is now at the last node. Link the new node to it.
        ptr->link = temp;
    } else {
        // If the list IS empty, the new node becomes the first node.
        // We modify the original 'first' pointer from main() here.
        *first = temp;
    }
}






    Let's zoom in on addToList. This is often the trickiest part to understand. I'll use a simple analogy: think of your linked list as a train 🚂.

    Each struct listNode is a train car.

    The data is the cargo inside the car.

    The link pointer is the coupler that connects it to the next car.

    The first pointer in your main function is like the stationmaster's note that tells you where the first car of the train is.

The addToList function has two main jobs:

    Build a brand new train car (temp).

    Attach this new car to the end of the existing train.

The Most Important Concept: The Double Pointer (listPointer *first)

This is the key. In C, when you pass a variable to a function, the function gets a copy.

    If you pass first (the address of the first car), the function gets a copy of that address. It can follow the address to see the train, but it can't change the stationmaster's original note.

    But what if there is no train yet? The stationmaster's note is blank (first is NULL). When we add the very first car, we need to change the note itself.

To change the original note, we must pass its location (its memory address). So, we pass &first.

    first is a listPointer (a pointer to a node).

    &first is the address of that pointer, making it a listPointer * (a pointer to a pointer to a node).

Inside addToList, we use *first to refer back to the original first pointer in main.

The Code, Step-by-Step

Let's look at the logical flow.
C

void addToList(listPointer *first, int item_data) {
    listPointer ptr, temp;

    // STEP 1: Build the new train car
    MALLOC(temp, sizeof(*temp));  // Get memory for a new car
    temp->data = item_data;       // Load cargo into it
    temp->link = NULL;            // Its rear coupler is unattached

This first part is always the same. We create a new node temp and set its link to NULL because no matter what, it's going to be the new last car on the train.

Now, we have to figure out where to attach it. There are two possibilities.

Scenario 1: The Train Yard is Empty (List is NULL)

This is the first time we're adding a node. In main, first is NULL.
C

    // STEP 2: Check if the train exists
    if (*first) { 
        // ... code for non-empty list ...
    } else {
        // This 'else' block runs because *first is NULL
        
        // STEP 3: The new car IS the train!
        *first = temp; 
    }
}

    if (*first): The function checks the value of the original first pointer from main. Since it's NULL, the condition is false, and we go to the else block.

    *first = temp;: This is the magic. It says, "Go back to the original first pointer in main and make it point to our new car temp."

Visually:

Before:
main(): first ---> NULL

After addToList runs once:
main(): first ---> [ Car 1 | data: 10 | link: NULL ] 🚂

Scenario 2: The Train Already Exists

Now let's say we call addToList again. The list is no longer empty.
C

    // STEP 2: Check if the train exists
    if (*first) { 
        // This 'if' block runs because *first is NOT NULL

        // STEP 3: Walk to the end of the train
        for(ptr = *first; ptr->link != NULL; ptr = ptr->link);

        // STEP 4: Attach the new car at the end
        ptr->link = temp; 
    } else {
        // ... code for empty list ...
    }
}

    if (*first): This is now true. *first points to our first car.

    The for loop: We can't just jump to the end. We have to walk down the train, car by car, looking for the last one.

        ptr = *first;: We create a temporary pointer ptr (like a worker walking alongside the train) and start it at the first car.

        ptr->link != NULL;: The loop condition. It asks, "Is the current car coupled to another one?" If yes, the loop continues. If ptr->link is NULL, it means we've found the last car, so the loop stops.

        ptr = ptr->link;: "Move the worker to the next car."

    ptr->link = temp;: When the loop finishes, ptr is pointing at the last car. This line attaches our new car (temp) to the end of it.

Visually:

Train Before:
first ---> [ Car 1 | link ] ---> [ Car 2 | link: NULL ] 🚂--🚂

Our new car:
temp ---> [ New Car | link: NULL ]

After the loop, ptr is pointing at Car 2.

The final step ptr->link = temp; does this:
first ---> [ Car 1 | link ] ---> [ Car 2 | link ] ---> [ New Car | link: NULL ] 🚂--🚂--🚂

So, in summary:

    The double pointer *first is needed to handle the special case of an empty list, allowing the function to change the first pointer back in main.

    The for loop is the standard way to "traverse" or "walk" the list to find the end.




    -------------------------------------------------------------------------------------------------------------------------
    TO PRINT LIST 


    15 void printList(listPointer first) {
16     printf("The list contains: ");
17     for(; first; first=first->link) {
18         printf("%4d", first->data);
19     }
20     printf("\n");
21 }

    void printList(listPointer first): This function is much simpler. It only needs a listPointer (a single pointer), not a pointer-to-a-pointer, because it's only reading the list. It doesn't need to change the original first variable in main.

    for(; first; first=first->link): A classic loop for traversing a linked list.

        Initialization: (empty) The first pointer is already our starting point.

        Condition: first. In C, NULL is treated as false and any other pointer value is true. So the loop continues as long as first is not NULL.

        Iteration: first = first->link. This is the key step. It moves the first pointer to the next node in the list for the next iteration.

    printf("%4d", first->data): Prints the data part of the current node. %4d pads the number with spaces to make it 4 characters wide, for neat alignment.

    Here is how the main file should look to work correctly with your other files.
C

#include "linkedlist.h" // Gives us access to our list functions

int main() {
    int dataArr[10];
    
    printf("Enter 10 integers:\n");
    for(int i = 0; i < 10; i++) {
        scanf("%d", &dataArr[i]); // Correctly read integers
    }
    
    // Initialize the list. The head pointer is NULL because it's empty.
    listPointer first = NULL; 
    
    // Loop through the array and add each element to the list
    for(int i = 0; i < 10; i++) {
        // Call addToList to create a node with the data from the array
        // and add it to the end of our list.
        // We pass the address of 'first' so the function can modify it.
        addToList(&first, dataArr[i]); 
    }
    
    // Print the entire list to see the result.
    printList(first); 
    
    return 0; // Indicate successful program execution
}.