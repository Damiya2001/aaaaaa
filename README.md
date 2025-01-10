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
