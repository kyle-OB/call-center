#include <iostream>
#include <thread>
#include <mutex>
#include <queue>
#include <condition_variable>
#include <chrono>

using namespace std;


mutex operatorMutex; // Mutex lock for operators
condition_variable cv; // Condition variable to manage waiting customers
int availableOperators = 3; // Number of available operators
queue<string> customerQueue; // Queue for customers waiting to be served
void assistCustomer(string customerName) {
{
// Lock the mutex before accessing the critical section
unique_lock<mutex> lock(operatorMutex);
// Wait if no operators are available
while (availableOperators == 0) {
cout << "All operators are currently busy assisting other callers..." << endl;
cv.wait(lock);
}
// Operator becomes available
availableOperators--;
cout << customerName << " is getting the connection to the operator ..." << endl;
}
// Simulate call duration
this_thread::sleep_for(chrono::seconds(2));
// After the call ends
{
unique_lock<mutex> lock(operatorMutex);
cout << customerName << " is now ending the conversation with the operator..." << endl;
availableOperators++;
cout << "Available operators=" << availableOperators << endl;
}
// Notify waiting threads
cv.notify_all();
}
void customerThread(string customerName) {
cout << customerName << " is waiting to speak to the operator..." <<
endl;
assistCustomer(customerName);
}
int main() {
cout << "Welcome to the call center. Please wait for the setup to start" << endl;
cout << "Initial operator availability = " << availableOperators <<
endl;
// Names of customers
string customers[] = {"Alice", "Bob", "John", "Mark", "Alex"};
// Threads for customers
thread customerThreads[5];
for (int i = 0; i < 5; i++) {
customerThreads[i] = thread(customerThread, customers[i]);
this_thread::sleep_for(chrono::milliseconds(500)); // Simulate staggered customer arrivals
}
for (int i = 0; i < 5; i++) {
customerThreads[i].join();
}
cout << "All calls are complete. Call center is now closed." << endl;
return 0;
}
