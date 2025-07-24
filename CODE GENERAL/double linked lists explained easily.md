The main difference is simple:

    A singly linked list is like a one-way street. Each node only knows what's next.

    A doubly linked list is a two-way street. Each node knows what's next AND what's previous. This lets you travel both forwards and backwards along the list.

Let's break down your sir's code, file by file.

File 1: dlinkedlist.h (The Blueprint 🏗️)

This header file defines the structure and capabilities of your doubly linked list. Any other file that includes it knows what a "doubly linked list node" is and what functions are available to manipulate it.
C

// Top Right Pane
#include<stdio.h>
#include<stdlib.h>

#define MALLOC(p,s) ... // Same as before

typedef struct listNode *listPointer;
struct listNode {
    listPointer llink; // <-- THE NEW POINTER
    char data[10];      // <-- DATA TYPE CHANGED
    listPointer rlink; // <-- The 'next' pointer
};

// Function prototypes...

Line-by-Line Deep Dive

    #include <stdio.h> and #include <stdlib.h>: Same as before, for printing (printf) and memory allocation (malloc).

    #define MALLOC(...): Your safe memory allocation macro. It's a good practice.

    struct listNode { ... };: This is the heart of the change. Let's look inside a single node or "train car".

        listPointer llink;: This is the new part. It stands for "left link". It's a pointer that stores the memory address of the node before it. This is the backwards-facing coupler.

        char data[10];: Your data is no longer a single int. It's now an array of characters, which is a C-style string. It can hold a string up to 9 characters long (plus 1 for the null terminator \0).

        listPointer rlink;: This is the "right link". It's the same as the link pointer from the singly linked list. It points to the node after it. This is the forward-facing coupler.



Here is how the function to add a node to the end should look, based on the struct you defined.
C

// Corrected and logical version of addToList
void addToList(listPointer *first, char item_data[]) {
    listPointer ptr, temp;

    // 1. Build the new node
    MALLOC(temp, sizeof(struct listNode));
    strcpy(temp->data, item_data); // Use strcpy for strings
    temp->llink = NULL;
    temp->rlink = NULL;

    // 2. Handle the case where the list is empty
    if (!(*first)) {
        *first = temp;
        return;
    }

    // 3. If list is not empty, find the last node
    for (ptr = *first; ptr->rlink != NULL; ptr = ptr->rlink);

    // 4. Perform the two-way link
    ptr->rlink = temp;  // Old last node points forward to the new one
    temp->llink = ptr;  // New node points back to the old last one
}

    Step 1: Build the node (temp)

        MALLOC(...): Get memory for the new node.

        strcpy(temp->data, item_data): You cannot use = to assign strings in C. You must use strcpy (string copy) to copy the item_data into temp->data.

        temp->llink = NULL; and temp->rlink = NULL;: A new node is not connected to anything yet.

    Step 2: Handle Empty List

        if (!(*first)): If the list is empty (first is NULL), then our new node temp is the first node. We use the double pointer *first to change the original first in main.

    Step 3: Find the Last Node

        for (ptr = *first; ptr->rlink != NULL; ptr = ptr->rlink);: This loop walks to the end of the list. It starts ptr at the beginning and moves it forward (ptr = ptr->rlink) until it finds the node whose rlink is NULL. That's the last node.

    Step 4: Perform the Two-Way Link

        This is the most important part of a doubly linked list. You need to make two connections.

        ptr->rlink = temp;: The old last node (ptr) now points forward to our new node (temp).

        temp->llink = ptr;: Our new node (temp) now points backward to the old last node (ptr).

printList Function

C

// Your printList function
void printList(listPointer first) {
    printf(...);
    for(; first; first = first->rlink) { // Note: uses rlink
        printf("%s ", first->data);      // Note: uses %s for string
    }
    printf("\n");
}

This is very similar to before. It starts at the beginning and moves forward using the rlink until it hits NULL. It prints the data using %s because the data is now a string.



his is how the file should be written to work correctly with the logic we defined.
C

#include "dlinkedlist.h"
#include <string.h>

int main() {
    char dataArr[10][50];
    int num_elements = 3; // Let's get 3 strings

    printf("Enter %d strings:\n", num_elements);
    for (int i = 0; i < num_elements; i++) {
        scanf("%s", dataArr[i]); // Read strings from user
    }

    // Initialize an empty list
    listPointer first = NULL;

    // Loop through the input data and add each item to the list
    for (int i = 0; i < num_elements; i++) {
        // Call the function with the data itself.
        // The function will handle malloc and linking.
        addToList(&first, dataArr[i]);
    }

    // Print the final list
    printf("\nYour list contains: ");
    printList(first);

    return 0;
}

This corrected version does it right: it calls addToList and passes the string data. The addToList function then correctly mallocs a new, unique node for each string and links it to the end of the list.



 A dry run is the best way to see how the pointers really work. Let's trace the corrected code with a simple example.

Imagine the user enters 3 strings: "Apple", "Ball", and "Cat".

Initial State in main

Before the loop starts, memory looks like this:

    dataArr = {"Apple", "Ball", "Cat"}

    listPointer first = NULL (This is crucial! The list is empty.)

Iteration 1: Adding "Apple"

    main loop (i=0): Calls addToList(&first, "Apple");

    Inside addToList:

        A new node, temp, is created with malloc. Let's say its memory address is 0x100.

        temp->data becomes "Apple".

        temp->llink is set to NULL.

        temp->rlink is set to NULL.

        The if (!(*first)) condition is checked. Since first in main is NULL, *first is NULL, and the condition is true.

        The code *first = temp; runs. This changes the first pointer in main to point to our new node.

Result after Iteration 1:
The first pointer now points to our first node.

main(): first ─> [ 0x100: llink:NULL | data:"Apple" | rlink:NULL ]

Iteration 2: Adding "Ball"

    main loop (i=1): Calls addToList(&first, "Ball");

    Inside addToList:

        A new node, temp, is created. Let's say its address is 0x200.

        temp->data becomes "Ball".

        temp->llink and temp->rlink are set to NULL.

        The if (!(*first)) condition is false because first now points to 0x100.

        The for loop runs to find the last node: for (ptr = *first; ptr->rlink != NULL; ptr = ptr->rlink);

            ptr starts at *first (address 0x100).

            It checks the condition: Is ptr->rlink (which is NULL) not equal to NULL? NULL != NULL is false.

            The loop stops immediately. ptr is left pointing at the node at 0x100.

        Now, the two-way linking happens:

            ptr->rlink = temp;  (The rlink of the "Apple" node is set to 0x200).

            temp->llink = ptr;  (The llink of the "Ball" node is set to 0x100).

Result after Iteration 2:
The two nodes are now linked in both directions.

main(): first ─> [ 0x100: llink:NULL | data:"Apple" | rlink:0x200 ]
[ 0x200: llink:0x100 | data:"Ball"  | rlink:NULL  ] <─

Iteration 3: Adding "Cat"

    main loop (i=2): Calls addToList(&first, "Cat");

    Inside addToList:

        A new node, temp, is created at address 0x300.

        temp->data becomes "Cat". llink and rlink are NULL.

        The if condition is false.

        The for loop runs to find the last node:

            ptr starts at *first (0x100).

            Check 1: Is ptr->rlink (0x200) not equal to NULL? Yes. So, the loop continues.

            ptr moves to the next node: ptr becomes 0x200.

            Check 2: Is ptr->rlink (which is NULL) not equal to NULL? False.

            The loop stops. ptr is now pointing at the "Ball" node (0x200).

        The two-way linking happens:

            ptr->rlink = temp;  (The rlink of the "Ball" node is set to 0x300).

            temp->llink = ptr;  (The llink of the "Cat" node is set to 0x200).

Final List Structure in Memory:

main(): first ─> [ 0x100: ... | rlink:0x200 ] ─> [ 0x200: ... | rlink:0x300 ] ─> [ 0x300: ... | rlink:NULL ]
<─ [ 0x200: llink:0x100 | ... ]     <─ [ 0x300: llink:0x200 | ... ]

Final Step: Printing the List

    main calls printList(first);. The function receives the starting address, 0x100.

    The printList for loop begins:

        Run 1: first is 0x100 (not NULL). Print first->data ("Apple"). first becomes first->rlink (0x200).

        Run 2: first is 0x200 (not NULL). Print first->data ("Ball"). first becomes first->rlink (0x300).

        Run 3: first is 0x300 (not NULL). Print first->data ("Cat"). first becomes first->rlink (NULL).

        Run 4: first is now NULL. The loop condition is false. The loop terminates.

Final Output on your screen:

Your list contains: Apple Ball Cats