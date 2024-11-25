# dsa-lab-7-2-
# dsa-lab-7
Exercise 1:

#include <iostream>
using namespace std;

class LinearQueue {
private:
    int* queue;
    int front;
    int rear;
    int capacity;

public:
    // Constructor
    LinearQueue(int size) {
        capacity = size;
        queue = new int[capacity];
        front = -1;
        rear = -1;
    }

    // Destructor
    ~LinearQueue() {
        delete[] queue;
    }

    // Enqueue operation to add an element to the queue
    bool Enqueue(int value) {
        if (Full()) {
            cout << "Queue is full. Cannot enqueue " << value << "." << endl;
            return false;
        }
        if (Empty()) {
            front = 0; // Initialize front when first element is added
        }
        queue[++rear] = value;
        return true;
    }

    // Dequeue operation to remove an element from the front of the queue
    bool Dequeue() {
        if (Empty()) {
            cout << "Queue is empty. Cannot dequeue." << endl;
            return false;
        }
        cout << "Dequeued: " << queue[front] << endl;
        front++;
        // Reset the queue if all elements are dequeued
        if (front > rear) {
            front = rear = -1;
        }
        return true;
    }

    // Check if the queue is empty
    bool Empty() const {
        return front == -1;
    }

    // Check if the queue is full
    bool Full() const {
        return rear == capacity - 1;
    }

    // Get the front element of the queue
    int getFront() const {
        if (Empty()) {
            cout << "Queue is empty. No front element." << endl;
            return -1; // Return -1 to indicate the queue is empty
        }
        return queue[front];
    }

    // Display the queue (for debugging)
    void displayQueue() const {
        if (Empty()) {
            cout << "Queue is empty." << endl;
            return;
        }
        cout << "Queue: ";
        for (int i = front; i <= rear; i++) {
            cout << queue[i] << " ";
        }
        cout << endl;
    }
};

int main() {
    LinearQueue q(5); // Create a queue with a capacity of 5

    // Test the queue operations
    q.Enqueue(10);
    q.Enqueue(20);
    q.Enqueue(30);
    q.displayQueue();

    cout << "Front element: " << q.getFront() << endl;

    q.Dequeue();
    q.displayQueue();

    q.Enqueue(40);
    q.Enqueue(50);
    q.Enqueue(60); // This will fail as the queue is full
    q.displayQueue();

    while (!q.Empty()) {
        q.Dequeue();
    }
    return 0;
}

OUTPUT:::
Queue: 10 20 30 
Front element: 10
Dequeued: 10
Queue: 20 30 
Queue: 20 30 40 50 
Queue is full. Cannot enqueue 60.
Dequeued: 20
Dequeued: 30
Dequeued: 40
Dequeued: 50
Queue is empty. Cannot dequeue.


Exercise 2:

#include <iostream>
using namespace std;

class CircularQueue {
private:
    int* queue;
    int front;
    int rear;
    int capacity;
    int size;

public:
    // Constructor
    CircularQueue(int maxSize) {
        capacity = maxSize;
        queue = new int[capacity];
        front = -1;
        rear = -1;
        size = 0;
    }

    // Destructor
    ~CircularQueue() {
        delete[] queue;
    }

    // Enqueue operation to add an element to the queue
    bool Enqueue(int value) {
        if (Full()) {
            cout << "Queue is full. Cannot enqueue " << value << "." << endl;
            return false;
        }
        if (Empty()) {
            front = rear = 0; // Initialize front and rear when first element is added
        } else {
            rear = (rear + 1) % capacity; // Move rear in a circular manner
        }
        queue[rear] = value;
        size++;
        return true;
    }

    // Dequeue operation to remove an element from the front of the queue
    bool Dequeue() {
        if (Empty()) {
            cout << "Queue is empty. Cannot dequeue." << endl;
            return false;
        }
        cout << "Dequeued: " << queue[front] << endl;
        if (front == rear) { // Reset if the queue becomes empty
            front = rear = -1;
        } else {
            front = (front + 1) % capacity; // Move front in a circular manner
        }
        size--;
        return true;
    }

    // Check if the queue is empty
    bool Empty() const {
        return size == 0;
    }

    // Check if the queue is full
    bool Full() const {
        return size == capacity;
    }

    // Get the front element of the queue
    int getFront() const {
        if (Empty()) {
            cout << "Queue is empty. No front element." << endl;
            return -1; // Return -1 to indicate the queue is empty
        }
        return queue[front];
    }

    // Display the queue (for debugging)
    void displayQueue() const {
        if (Empty()) {
            cout << "Queue is empty." << endl;
            return;
        }
        cout << "Queue: ";
        for (int i = 0; i < size; i++) {
            cout << queue[(front + i) % capacity] << " ";
        }
        cout << endl;
    }
};

int main() {
    CircularQueue q(5); // Create a circular queue with a capacity of 5

    // Test the queue operations
    q.Enqueue(10);
    q.Enqueue(20);
    q.Enqueue(30);
    q.displayQueue();

    cout << "Front element: " << q.getFront() << endl;

    q.Dequeue();
    q.displayQueue();

    q.Enqueue(40);
    q.Enqueue(50);
    q.Enqueue(60); // This will fail as the queue is full
    q.displayQueue();

    q.Dequeue();
    q.Enqueue(70); // Circular nature in action
    q.displayQueue();

    while (!q.Empty()) {
        q.Dequeue();
    }

    q.Dequeue(); // Attempt to dequeue from an empty queue

    return 0;
}
OUTPUT:::

Queue: 10 20 30 
Front element: 10
Dequeued: 10


Exercise 3:

#include <iostream>
using namespace std;

// Node structure for the linked list
struct Node {
    int data;
    Node* next;

    Node(int value) : data(value), next(nullptr) {}
};

// LinearQueue class implemented using a linked list
class LinearQueue {
private:
    Node* front;
    Node* rear;

public:
    // Constructor
    LinearQueue() : front(nullptr), rear(nullptr) {}

    // Destructor to clean up memory
    ~LinearQueue() {
        while (!Empty()) {
            Dequeue();
        }
    }

    // Enqueue operation to add an element at the end of the queue
    void Enqueue(int value) {
        Node* newNode = new Node(value);
        if (Empty()) {
            front = rear = newNode; // If queue is empty, both front and rear point to the new node
        } else {
            rear->next = newNode; // Link the new node at the end
            rear = newNode;       // Update rear to the new node
        }
        cout << "Enqueued: " << value << endl;
    }

    // Dequeue operation to remove the front element of the queue
    bool Dequeue() {
        if (Empty()) {
            cout << "Queue is empty. Cannot dequeue." << endl;
            return false;
        }
        Node* temp = front;      // Store the current front node
        front = front->next;     // Move front to the next node
        cout << "Dequeued: " << temp->data << endl;
        delete temp;             // Free the memory of the dequeued node

        if (!front) {            // If the queue becomes empty, set rear to nullptr
            rear = nullptr;
        }
        return true;
    }

    // Check if the queue is empty
    bool Empty() const {
        return front == nullptr;
    }

    // Get the front element of the queue
    int getFront() const {
        if (Empty()) {
            cout << "Queue is empty. No front element." << endl;
            return -1; // Return -1 to indicate the queue is empty
        }
        return front->data;
    }

    // Display the queue
    void displayQueue() const {
        if (Empty()) {
            cout << "Queue is empty." << endl;
            return;
        }
        cout << "Queue: ";
        Node* temp = front;
        while (temp) {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }
};

int main() {
    LinearQueue q;

    // Test the queue operations
    q.Enqueue(10);
    q.Enqueue(20);
    q.Enqueue(30);
    q.displayQueue();

    cout << "Front element: " << q.getFront() << endl;

    q.Dequeue();
    q.displayQueue();

    q.Enqueue(40);
    q.Enqueue(50);
    q.displayQueue();

    while (!q.Empty()) {
        q.Dequeue();
    }

    q.Dequeue(); // Attempt to dequeue from an empty queue

    return 0;
}

OUTPUT:::
Enqueued: 10
Enqueued: 20
Enqueued: 30
Queue: 10 20 30 
Front element: 10
Dequeued: 10
Queue: 20 30 
Enqueued: 40
Enqueued: 50
Queue: 20 30 40 50 
Dequeued: 20
Dequeued: 30
Dequeued: 40
Dequeued: 50
Queue is empty. Cannot dequeue.


Exercise 4:

#include <iostream>
using namespace std;

class AscendingPriorityQueue {
private:
    int* queue;
    int capacity;
    int size;

public:
    // Constructor
    AscendingPriorityQueue(int maxSize) {
        capacity = maxSize;
        queue = new int[capacity];
        size = 0;
    }

    // Destructor
    ~AscendingPriorityQueue() {
        delete[] queue;
    }

    // Enqueue operation to add an element to the queue
    bool Enqueue(int value) {
        if (size == capacity) {
            cout << "Queue is full. Cannot enqueue " << value << "." << endl;
            return false;
        }
        queue[size++] = value; // Add the new value and increase size
        cout << "Enqueued: " << value << endl;
        return true;
    }

    // Dequeue operation to remove the minimum element from the queue
    bool Dequeue() {
        if (Empty()) {
            cout << "Queue is empty. Cannot dequeue." << endl;
            return false;
        }

        // Find the index of the minimum element
        int minIndex = 0;
        for (int i = 1; i < size; i++) {
            if (queue[i] < queue[minIndex]) {
                minIndex = i;
            }
        }

        // Remove the minimum element
        cout << "Dequeued: " << queue[minIndex] << endl;

        // Shift elements to the left to fill the gap
        for (int i = minIndex; i < size - 1; i++) {
            queue[i] = queue[i + 1];
        }

        size--; // Decrease size
        return true;
    }

    // Check if the queue is empty
    bool Empty() const {
        return size == 0;
    }

    // Display the queue
    void displayQueue() const {
        if (Empty()) {
            cout << "Queue is empty." << endl;
            return;
        }
        cout << "Queue: ";
        for (int i = 0; i < size; i++) {
            cout << queue[i] << " ";
        }
        cout << endl;
    }
};

int main() {
    AscendingPriorityQueue q(10); // Create a queue with a capacity of 10

    // Test the queue operations
    q.Enqueue(30);
    q.Enqueue(10);
    q.Enqueue(20);
    q.displayQueue();

    q.Dequeue(); // Removes 10
    q.displayQueue();

    q.Enqueue(5);
    q.Enqueue(40);
    q.displayQueue();

    q.Dequeue(); // Removes 5
    q.displayQueue();

    q.Enqueue(15);
    q.Enqueue(35);
    q.displayQueue();

    while (!q.Empty()) {
        q.Dequeue();
    }

    q.Dequeue(); // Attempt to dequeue from an empty queue

    return 0;
}

OUTPUT:::
Enqueued: 30
Enqueued: 10
Enqueued: 20
Queue: 30 10 20 
Dequeued: 10
Queue: 30 20 
Enqueued: 5
Enqueued: 40
Queue: 30 20 5 40 
Dequeued: 5
Queue: 30 20 40 
Enqueued: 15
Enqueued: 35
Queue: 30 20 40 15 35 
Dequeued: 15
Dequeued: 20
Dequeued: 30
Dequeued: 35
Dequeued: 40
Queue is empty. Cannot dequeue.
