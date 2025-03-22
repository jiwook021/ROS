# Code Overview: main.cpp

This C++ code is a **simulation of a simplified ROS (Robot Operating System) node** that listens to messages on a topic and processes them. ROS is a framework widely used in robotics for communication between different components (nodes) of a robotic system. This code mimics the behavior of a ROS subscriber node, which listens to messages published on a specific topic and processes them using a callback function.

Let’s break down the **purpose**, **functionality**, and **structure** of the code:

---

### **Purpose**
The purpose of this code is to:
1. **Simulate a ROS subscriber node** that listens to messages on a topic (e.g., "chatter").
2. **Process incoming messages** using a callback function.
3. **Demonstrate multithreading** by simulating a publisher adding messages to the subscriber's queue in a separate thread.
4. **Handle graceful shutdown** when the user presses Ctrl+C (SIGINT).

This code is a **teaching tool** to help understand how ROS nodes work, how messages are processed, and how multithreading can be used in a ROS-like system.

---

### **Main Functionality**
The code achieves its purpose through the following key components:

1. **Message Structure (`Message`)**:
   - A simple `struct` that represents a message. It contains a single field, `data`, which holds the message content as a string.
   - This mimics the `std_msgs::String` message type used in ROS.

2. **Fake Subscriber (`FakeSubscriber`)**:
   - A class that simulates a ROS subscriber. It:
     - Takes a topic name and a callback function as input.
     - Maintains a queue of messages (`std::vector<Message>`).
     - Provides methods to add messages to the queue (`addMessage`) and process them (`processOne`).

3. **Global Control Variable (`keepRunning`)**:
   - A `volatile bool` that controls whether the program should keep running. It is set to `false` when the user presses Ctrl+C, triggering a graceful shutdown.

4. **Signal Handler (`signalHandler`)**:
   - A function that handles the Ctrl+C signal (SIGINT). It sets `keepRunning` to `false` and prints a shutdown message.

5. **Fake ROS Functions (`fakeRosOk` and `fakeRosSpin`)**:
   - `fakeRosOk`: Simulates the `ros::ok()` function in ROS, which checks if the node should continue running.
   - `fakeRosSpin`: Simulates the `ros::spin()` function, which keeps the node alive and processes messages in a loop.

6. **Callback Function (`myCallback`)**:
   - A function that processes incoming messages. It prints the message content to the console.

7. **Message Publisher Simulation (`addMessageFunction`)**:
   - A function that runs in a separate thread. It simulates a ROS publisher by adding messages to the subscriber's queue at regular intervals.

8. **Main Function**:
   - Sets up the signal handler.
   - Creates a `FakeSubscriber` object.
   - Adds initial messages to the queue.
   - Starts a thread to simulate a publisher.
   - Enters the `fakeRosSpin` loop to process messages.
   - Waits for the publisher thread to finish and exits gracefully.

---

### **Algorithms and Logic**
1. **Message Queue Management**:
   - The `FakeSubscriber` class uses a `std::vector<Message>` to store incoming messages.
   - Messages are added to the end of the vector (`push_back`) and removed from the front (`erase`), simulating a First-In-First-Out (FIFO) queue.

2. **Event Loop (`fakeRosSpin`)**:
   - The `fakeRosSpin` function runs in a loop, checking for new messages using `processOne`.
   - If no messages are available, it sleeps for 4 seconds to avoid consuming too much CPU.

3. **Multithreading**:
   - The `addMessageFunction` runs in a separate thread, adding messages to the queue every 100 milliseconds.
   - The main thread processes messages in the `fakeRosSpin` loop.
   - The two threads communicate through the shared `FakeSubscriber` object.

4. **Graceful Shutdown**:
   - When Ctrl+C is pressed, the `signalHandler` sets `keepRunning` to `false`.
   - Both the `fakeRosSpin` loop and the `addMessageFunction` thread check `keepRunning` and exit when it becomes `false`.

---

### **Overall Structure**
The code is structured as follows:
1. **Header Includes**:
   - Standard C++ libraries are included for basic functionality (e.g., `iostream`, `vector`, `thread`).

2. **Message Structure**:
   - Defines the `Message` struct to represent a message.

3. **Fake Subscriber Class**:
   - Implements the `FakeSubscriber` class to simulate a ROS subscriber.

4. **Global Variables and Functions**:
   - `keepRunning`: Controls the program's execution.
   - `signalHandler`: Handles Ctrl+C.
   - `fakeRosOk` and `fakeRosSpin`: Simulate ROS functionality.

5. **Callback Function**:
   - Defines `myCallback` to process messages.

6. **Publisher Simulation**:
   - Defines `addMessageFunction` to simulate a publisher.

7. **Main Function**:
   - Sets up the program, creates the subscriber, starts the publisher thread, and enters the spin loop.

---

### **How the Parts Work Together**
1. The `main` function initializes the program:
   - Sets up the signal handler.
   - Creates a `FakeSubscriber` object for the "chatter" topic.
   - Adds initial messages to the queue.
   - Starts a thread to simulate a publisher.

2. The `fakeRosSpin` function processes messages in a loop:
   - It calls `processOne` to process a message if available.
   - If no messages are available, it sleeps briefly.

3. The `addMessageFunction` thread adds messages to the queue at regular intervals.

4. When Ctrl+C is pressed:
   - The `signalHandler` sets `keepRunning` to `false`.
   - Both the `fakeRosSpin` loop and the `addMessageFunction` thread exit gracefully.

5. The program prints a shutdown message and exits cleanly.

---

### **Problem Being Solved**
This code solves the problem of **simulating a ROS-like system** for educational purposes. It demonstrates:
- How a subscriber node listens to messages on a topic.
- How messages are processed using a callback function.
- How multithreading can be used to simulate a publisher and subscriber running concurrently.
- How to handle graceful shutdown using signals.

---

### **Approach Taken**
The code takes a **modular and object-oriented approach**:
- The `FakeSubscriber` class encapsulates the functionality of a ROS subscriber.
- The `Message` struct represents a message.
- Global functions like `fakeRosSpin` and `addMessageFunction` simulate ROS behavior.
- Multithreading is used to simulate concurrent execution of the publisher and subscriber.

This approach makes the code easy to understand, extend, and modify for different use cases.

---

### **Summary**
This code is a **simplified simulation of a ROS subscriber node**. It demonstrates:
- Message handling using a queue.
- Callback-based message processing.
- Multithreading for concurrent execution.
- Graceful shutdown using signals.

It’s a great example of how ROS-like systems work under the hood, and it’s designed to be educational and easy to understand.