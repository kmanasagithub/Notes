ultithreading

What is Synchronization in Java?
Synchronization is a locking mechanism in java which will be achieved using synchronize keyword.
It ensures that at a given point of time only one thread can access the critical section.

Why Synchronization is needed?
Without synchronization:
Multiple threads access same data simultaneously
Leads to race condition
Data becomes inconsistent

Types of Synchronization:
a. Method Synchronization: Entire method is locked for one thread at a time.
synchronized void display() {
// critical section
}

b. Block Synchronization: Only a specific block of code is synchronized.
void display() {
synchronized(this) {
// critical section
}
}

c. Static Synchronization: It locks a class level object.
static synchronized void show() {
// class-level lock
}

What is a Race Condition in Java?
In Java, a race condition occurs when:
Multiple threads access and modify shared data at the same time, and the final result depends on the timing (order) of execution.
👉 Because threads “race” to update the same resource, the output becomes unpredictable and incorrect.
⚠️ Why does it happen?
Threads run concurrently
Shared variable is accessed without synchronization.
Operations are not atomic (they take multiple steps)

What is Deadlock in Java?
In Java, a deadlock is a situation where:
Two or more threads are permanently blocked because each is waiting for a resource held by the other.
👉 As a result, none of the threads can proceed, and the program gets stuck.
Therefore, multithread in a java program, the synchronized keyword can potentially cause a deadlock condition.

What is volatile keyword in Java?
In Java, the volatile keyword is used to ensure that a variable’s value is always read from and written to main memory, not from a thread’s local cache.

Why do we need volatile?
In multithreading:
Each thread may keep a local copy (cache) of variables
Changes made by one thread may not be visible to others
👉 This leads to inconsistent data

When to use volatile
Flags (true/false signals)
Status variables
When only read/write (no complex operations)

What is join() method?
In java, the Thread class provides the join() method where allow one thread to wait for another thread to complete its execution.
This is useful when the output of thread is dependent on the execution of another object.

What are wait(), notify(), notifyAll() in Java?
In java, these are the method used in inter-thread communication.
They help threads coordinate with each other instead of running blindly.

wait(): It pauses the current thread and release the lock, until it gets notified by another thread.

notify(): Wakes up one waiting thread.

notifyAll(): wakes up all waiting threads.
