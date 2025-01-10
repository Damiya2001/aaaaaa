

/*#include <stdio.h>

#define MAXLIST 15
#define NUM_MEAN_VAR 4

typedef struct {
    int count;
    double entry[MAXLIST];
}List;

void bubbleSort(double arr[],int n){
        for(int i=0;i<n-1;i++){
            for(int j=0;j<n-1-i;j++){
                if(arr[j]>arr[j+1]){
                    double temp = arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=temp;
                }
            }
        }
    }

double computeMean(double arr[],int n){
    double sum=0.0;
    for(int i=0;i<n;i++){
        sum+=arr[i];
    }
    return sum/n;
}

double computeVariance(double arr[],int n,double mean){
    double sum=0.0;
    for(int i=0;i<n;i++){
        sum+= (arr[i]-mean)*(arr[i]-mean);
    }
    return sum/n;
}

int main() {
    List L;
    // 3.5 5.1 0.6 97.1 51.2 41.8 900.8 9.3 32.4 56.7
    printf("Enter number of elements (upto %d) :",MAXLIST);
    scanf("%d",&L.count);

    if(L.count>MAXLIST || L.count<=0){
        printf("Invalid number of elements\n");
        return 1;
    }

    printf("Enter %d elements :\n",L.count);
    for(int i=0;i<L.count;i++){
        scanf("%lf",&L.entry[i]);
    }

    printf("Original List:\n");
    for(int i=0;i<L.count;i++){
        printf("%.2lf ",L.entry[i]);
    }

    printf("\n\n");

    bubbleSort(L.entry,L.count);

    printf("Sorted list:\n");
    for (int i = 0; i < L.count; i++) {
        printf("%.2lf ", L.entry[i]);
    }
    printf("\n\n");

    double mean= computeMean(L.entry,NUM_MEAN_VAR);
    printf("Mean of the first four numbers: %.2lf\n", mean);

    double variance = computeVariance(L.entry,NUM_MEAN_VAR,mean);
    printf("Variance of the first four numbers: %.2lf\n", variance);



    return 0;
}




//CT

#include <stdio.h>

#define MAX_QUEUE 10  // Maximum size of the queue
#define MAX_LIST 10   // Maximum size of the list

typedef struct {
    int count;
    double entry[MAX_LIST];
} List;

typedef struct {
    int front;
    int rear;
    double items[MAX_QUEUE];
} Queue;

void createQ(Queue *q) {
    q->front = 0;
    q->rear = -1;
}

int isFull(Queue *q) {
    return q->rear == MAX_QUEUE - 1;
}

int isEmpty(Queue *q) {
    return q->rear < q->front;
}

void appendQ(Queue *q, double value) {
    if (isFull(q)) {
        printf("Queue is full\n");
        return;
    }
    q->rear++;
    q->items[q->rear] = value;
}

double serveQ(Queue *q) {
    if (isEmpty(q)) {
        printf("Queue is empty\n");
        return -1;  // Return an invalid value to indicate error
    }
    double value = q->items[q->front];
    q->front++;
    return value;
}

void bubbleSort(double arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {  // Sort in ascending order
                double temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

double computeMeanOfLargestFour(double arr[], int n) {
    double sum = 0.0;
    for (int i = n - 4; i < n; i++) {  // Sum the last 4 elements (largest 4)
        sum += arr[i];
    }
    return sum / 4.0;
}

int main() {
    Queue q;
    createQ(&q);

    double numbers[] = {5.4, 3.6, 106.8, 0.9, 272, 8.4, 1900.5, 4.5};
    int n = sizeof(numbers) / sizeof(numbers[0]);

    for (int i = 0; i < n; i++) {
        appendQ(&q, numbers[i]);
    }

    List L;
    L.count = 0;

    while (!isEmpty(&q)) {
        L.entry[L.count++] = serveQ(&q);
    }

    bubbleSort(L.entry, L.count);

    printf("Sorted List:\n");
    for (int i = 0; i < L.count; i++) {
        printf("%.2lf ", L.entry[i]);
    }
    printf("\n");

    // Compute the mean of the largest four numbers
    double mean = computeMeanOfLargestFour(L.entry, L.count);
    printf("Mean of the largest four numbers: %.2lf\n", mean);

    return 0;
}








#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    double data;
    struct Node *next;
} Node;

// Queue structure
typedef struct {
    Node *front;
    Node *rear;
} Queue;

// List structure
typedef struct {
    Node *head;
    int count;
} List;

// Function to create a new node
Node* createNode(double value) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    if (!newNode) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    newNode->data = value;
    newNode->next = NULL;
    return newNode;
}

// Initialize the Queue
void createQueue(Queue *q) {
    q->front = NULL;
    q->rear = NULL;
}

// Check if the Queue is empty
int isQueueEmpty(Queue *q) {
    return q->front == NULL;
}

// Append an item to the Queue
void appendQueue(Queue *q, double value) {
    Node *newNode = createNode(value);
    if (isQueueEmpty(q)) {
        q->front = q->rear = newNode;
    } else {
        q->rear->next = newNode;
        q->rear = newNode;
    }
}

// Serve an item from the Queue
double serveQueue(Queue *q) {
    if (isQueueEmpty(q)) {
        printf("Queue is empty\n");
        return -1; // Return invalid value
    }
    Node *temp = q->front;
    double value = temp->data;
    q->front = q->front->next;
    free(temp);
    return value;
}

// Initialize the List
void createList(List *l) {
    l->head = NULL;
    l->count = 0;
}

// Append an item to the List
void appendList(List *l, double value) {
    Node *newNode = createNode(value);
    if (!l->head) {
        l->head = newNode;
    } else {
        Node *temp = l->head;
        while (temp->next) {
            temp = temp->next;
        }
        temp->next = newNode;
    }
    l->count++;
}

// Bubble sort for the List
void bubbleSortList(List *l) {
    if (!l->head || l->count <= 1) return;

    Node *i, *j;
    for (i = l->head; i->next; i = i->next) {
        for (j = l->head; j->next; j = j->next) {
            if (j->data > j->next->data) {
                double temp = j->data;
                j->data = j->next->data;
                j->next->data = temp;
            }
        }
    }
}

// Compute mean of the largest four numbers in the List
double computeMeanLargestFour(List *l) {
    if (l->count < 4) {
        printf("Not enough elements to compute mean of largest four numbers\n");
        return -1;
    }

    Node *temp = l->head;
    double sum = 0.0;
    int skip = l->count - 4;

    for (int i = 0; i < skip; i++) {
        temp = temp->next; // Skip to the last 4 elements
    }

    for (int i = 0; i < 4; i++) {
        sum += temp->data;
        temp = temp->next;
    }

    return sum / 4.0;
}

// Display the List
void displayList(List *l) {
    Node *temp = l->head;
    while (temp) {
        printf("%.2lf ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

int main() {
    Queue q;
    createQueue(&q);

    double numbers[] = {5.4, 3.6, 106.8, 0.9, 272, 8.4, 1900.5, 4.5};
    int n = sizeof(numbers) / sizeof(numbers[0]);

    // Append numbers to the Queue
    for (int i = 0; i < n; i++) {
        appendQueue(&q, numbers[i]);
    }

    List L;
    createList(&L);

    // Serve Queue items and populate List
    while (!isQueueEmpty(&q)) {
        appendList(&L, serveQueue(&q));
    }

    // Sort the List
    bubbleSortList(&L);

    printf("Sorted List:\n");
    displayList(&L);

    // Compute the mean of the largest four numbers
    double mean = computeMeanLargestFour(&L);
    printf("Mean of the largest four numbers: %.2lf\n", mean);

    return 0;
}















#include <stdio.h>
#include <stdlib.h>

// Node structure for a linked list
typedef struct Node {
    double data;
    struct Node *next;
} Node;

// List structure
typedef struct {
    Node *head;
    int count;
} List;

// Function to create a new node
Node* createNode(double value) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    if (!newNode) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    newNode->data = value;
    newNode->next = NULL;
    return newNode;
}

// Function to initialize the list
void createList(List *l) {
    l->head = NULL;
    l->count = 0;
}

// Append a value to the list
void appendToList(List *l, double value) {
    Node *newNode = createNode(value);
    if (!l->head) {
        l->head = newNode;
    } else {
        Node *temp = l->head;
        while (temp->next) {
            temp = temp->next;
        }
        temp->next = newNode;
    }
    l->count++;
}

// Bubble sort for a linked list
void bubbleSortList(List *l) {
    if (!l->head || l->count <= 1) return;

    Node *i, *j;
    for (i = l->head; i; i = i->next) {
        for (j = l->head; j->next; j = j->next) {
            if (j->data > j->next->data) {
                double temp = j->data;
                j->data = j->next->data;
                j->next->data = temp;
            }
        }
    }
}

// Compute the mean of the first `n` elements in the list
double computeMean(List *l, int n) {
    Node *temp = l->head;
    double sum = 0.0;
    for (int i = 0; i < n && temp; i++) {
        sum += temp->data;
        temp = temp->next;
    }
    return sum / n;
}

// Compute the variance of the first `n` elements in the list
double computeVariance(List *l, int n, double mean) {
    Node *temp = l->head;
    double sum = 0.0;
    for (int i = 0; i < n && temp; i++) {
        sum += (temp->data - mean) * (temp->data - mean);
        temp = temp->next;
    }
    return sum / n;
}

// Display the contents of the list
void displayList(List *l) {
    Node *temp = l->head;
    while (temp) {
        printf("%.2lf ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

// Main function
int main() {
    List L;
    createList(&L);

    int n;
    printf("Enter number of elements (up to 15): ");
    scanf("%d", &n);

    if (n > 15 || n <= 0) {
        printf("Invalid number of elements\n");
        return 1;
    }

    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++) {
        double value;
        scanf("%lf", &value);
        appendToList(&L, value);
    }

    printf("Original List:\n");
    displayList(&L);
    printf("\n");

    bubbleSortList(&L);

    printf("Sorted List:\n");
    displayList(&L);
    printf("\n");

    // Compute the mean and variance of the first 4 numbers
    double mean = computeMean(&L, 4);
    printf("Mean of the first four numbers: %.2lf\n", mean);

    double variance = computeVariance(&L, 4, mean);
    printf("Variance of the first four numbers: %.2lf\n", variance);

    return 0;
}






#include <stdio.h>
#include <stdbool.h>

#define MAX_STACK 15
#define NUM_MEAN_VAR 4

typedef struct {
    int top;
    double data[MAX_STACK];
} Stack;

// Stack operations
void createStack(Stack *s) {
    s->top = -1;
}

bool isStackFull(Stack *s) {
    return s->top == MAX_STACK - 1;
}

bool isStackEmpty(Stack *s) {
    return s->top == -1;
}

void push(Stack *s, double value) {
    if (isStackFull(s)) {
        printf("Stack is full! Cannot push %.2lf\n", value);
        return;
    }
    s->data[++s->top] = value;
}

double pop(Stack *s) {
    if (isStackEmpty(s)) {
        printf("Stack is empty! Cannot pop\n");
        return -1;
    }
    return s->data[s->top--];
}

// Bubble sort for the stack array
void bubbleSort(double arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                double temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

// Compute the mean of the first `n` elements
double computeMean(double arr[], int n) {
    double sum = 0.0;
    for (int i = 0; i < n; i++) {
        sum += arr[i];
    }
    return sum / n;
}

// Compute the variance of the first `n` elements
double computeVariance(double arr[], int n, double mean) {
    double sum = 0.0;
    for (int i = 0; i < n; i++) {
        sum += (arr[i] - mean) * (arr[i] - mean);
    }
    return sum / n;
}

// Main function
int main() {
    Stack s;
    createStack(&s);

    int n;
    printf("Enter number of elements (up to %d): ", MAX_STACK);
    scanf("%d", &n);

    if (n > MAX_STACK || n <= 0) {
        printf("Invalid number of elements\n");
        return 1;
    }

    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++) {
        double value;
        scanf("%lf", &value);
        push(&s, value);
    }

    // Transfer stack data to an array for sorting
    double arr[MAX_STACK];
    int count = 0;
    while (!isStackEmpty(&s)) {
        arr[count++] = pop(&s);
    }

    // Sort the array
    bubbleSort(arr, count);

    printf("Sorted list:\n");
    for (int i = 0; i < count; i++) {
        printf("%.2lf ", arr[i]);
    }
    printf("\n\n");

    // Compute mean and variance of the first 4 numbers
    double mean = computeMean(arr, NUM_MEAN_VAR);
    printf("Mean of the first four numbers: %.2lf\n", mean);

    double variance = computeVariance(arr, NUM_MEAN_VAR, mean);
    printf("Variance of the first four numbers: %.2lf\n", variance);

    return 0;
}






#include <stdio.h>
#include <stdbool.h>

#define MAX_QUEUE 15
#define NUM_MEAN_VAR 4

typedef struct {
    int front;
    int rear;
    int count;
    double data[MAX_QUEUE];
} CircularQueue;

// Circular Queue operations
void createQueue(CircularQueue *q) {
    q->front = 0;
    q->rear = -1;
    q->count = 0;
}

bool isQueueFull(CircularQueue *q) {
    return q->count == MAX_QUEUE;
}

bool isQueueEmpty(CircularQueue *q) {
    return q->count == 0;
}

void enqueue(CircularQueue *q, double value) {
    if (isQueueFull(q)) {
        printf("Queue is full! Cannot enqueue %.2lf\n", value);
        return;
    }
    q->rear = (q->rear + 1) % MAX_QUEUE;
    q->data[q->rear] = value;
    q->count++;
}

double dequeue(CircularQueue *q) {
    if (isQueueEmpty(q)) {
        printf("Queue is empty! Cannot dequeue\n");
        return -1;
    }
    double value = q->data[q->front];
    q->front = (q->front + 1) % MAX_QUEUE;
    q->count--;
    return value;
}

// Bubble sort for an array
void bubbleSort(double arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                double temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

// Compute the mean of the first `n` elements
double computeMean(double arr[], int n) {
    double sum = 0.0;
    for (int i = 0; i < n; i++) {
        sum += arr[i];
    }
    return sum / n;
}

// Compute the variance of the first `n` elements
double computeVariance(double arr[], int n, double mean) {
    double sum = 0.0;
    for (int i = 0; i < n; i++) {
        sum += (arr[i] - mean) * (arr[i] - mean);
    }
    return sum / n;
}

// Main function
int main() {
    CircularQueue q;
    createQueue(&q);

    int n;
    printf("Enter number of elements (up to %d): ", MAX_QUEUE);
    scanf("%d", &n);

    if (n > MAX_QUEUE || n <= 0) {
        printf("Invalid number of elements\n");
        return 1;
    }

    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++) {
        double value;
        scanf("%lf", &value);
        enqueue(&q, value);
    }

    // Transfer data from queue to array for sorting
    double arr[MAX_QUEUE];
    int count = 0;
    while (!isQueueEmpty(&q)) {
        arr[count++] = dequeue(&q);
    }

    // Sort the array
    bubbleSort(arr, count);

    printf("Sorted list:\n");
    for (int i = 0; i < count; i++) {
        printf("%.2lf ", arr[i]);
    }
    printf("\n\n");

    // Compute mean and variance of the first 4 numbers
    double mean = computeMean(arr, NUM_MEAN_VAR);
    printf("Mean of the first four numbers: %.2lf\n", mean);

    double variance = computeVariance(arr, NUM_MEAN_VAR, mean);
    printf("Variance of the first four numbers: %.2lf\n", variance);

    return 0;
}








void selectionSort(double arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        // Swap the found minimum element with the first element
        if (minIndex != i) {
            double temp = arr[minIndex];
            arr[minIndex] = arr[i];
            arr[i] = temp;
        }
    }
}









void insertionSort(double arr[], int n) {
    for (int i = 1; i < n; i++) {
        double key = arr[i];
        int j = i - 1;

        // Move elements of arr[0..i-1] that are greater than key
        // to one position ahead of their current position
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}













void merge(double arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Temporary arrays
    double L[n1], R[n2];

    // Copy data to temp arrays L[] and R[]
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];

    // Merge the temp arrays back into arr[left..right]
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copy the remaining elements of L[], if any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy the remaining elements of R[], if any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(double arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;

        // Sort the first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
}



mergeSort(L.entry, 0, L.count - 1);

