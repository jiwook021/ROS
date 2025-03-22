# Code Overview: drone_control_node.cpp

This code is a **drone control system** implemented in C++ using the **Robot Operating System (ROS)** framework. It is designed to control a drone's movement in a 3D space (X, Y, Z axes) by sending velocity commands (`cmd_vel`) based on a target position or user input. The system uses **PID (Proportional-Integral-Derivative) controllers** to calculate the required velocity adjustments to reach the target position. Below is a detailed explanation of its purpose, functionality, and structure.

---

### **Purpose of the Code**
The code is designed to:
1. **Control a drone's movement** in a simulated environment (e.g., Gazebo) by sending velocity commands.
2. **Track the drone's current position** using data from Gazebo's `/gazebo/model_states` topic.
3. **Compute velocity commands** using PID controllers to move the drone toward a target position.
4. **Handle user input** for setting target positions, velocity commands, or adjusting PID gains.
5. **Run in a multi-threaded environment** to ensure smooth operation and responsiveness.

---

### **Main Functionality**
1. **PID Control**:
   - The system uses three PID controllers (one for each axis: X, Y, Z) to calculate the velocity required to move the drone toward the target position.
   - The PID controllers continuously adjust the drone's velocity based on the difference (error) between the target position and the drone's current position.

2. **Position Tracking**:
   - The drone's current position is obtained from Gazebo's `/gazebo/model_states` topic, which provides the positions of all models in the simulation.

3. **User Interaction**:
   - The user can input commands to:
     - Set a target position (e.g., `x y z`).
     - Directly set velocity commands (e.g., `vx vy vz wx wy wz`).
     - Adjust PID gains for each axis (e.g., `kp_x ki_x kd_x kp_y ki_y kd_y kp_z ki_z kd_z`).
     - Move the drone incrementally (e.g., `w` to move up, `s` to move down, etc.).

4. **Multi-threading**:
   - The system uses multiple threads to:
     - Continuously process incoming position data from Gazebo.
     - Run the PID control loop independently.
     - Handle user input and update the target position or velocity commands.

---

### **Algorithms Used**
1. **PID Control Algorithm**:
   - The PID controller calculates the control output (velocity) based on:
     - **Proportional (P) term**: Directly proportional to the error (difference between target and current position).
     - **Integral (I) term**: Accumulates past errors to eliminate steady-state errors.
     - **Derivative (D) term**: Predicts future error based on the rate of change of the error.
   - The formula used is:
     ```
     output = kp * error + ki * integral + kd * derivative
     ```

2. **Thread Synchronization**:
   - The system uses `std::mutex` to protect shared data (e.g., `cmd_vel`, `current_pos`, `target_pos`) from concurrent access by multiple threads.
   - `std::atomic` variables are used for thread-safe flags (e.g., `target_set`, `position_received`).

---

### **Overall Structure**
The code is structured into the following components:

1. **PIDController Class**:
   - Encapsulates the PID control logic.
   - Provides methods to compute the control output and adjust PID gains.

2. **Global Variables**:
   - Store shared data such as the drone's current position, target position, velocity commands, and thread-safe flags.

3. **Main Function**:
   - Initializes the ROS node.
   - Sets up publishers (to send velocity commands) and subscribers (to receive position updates).
   - Starts threads for handling user input and PID control.

4. **Thread Functions**:
   - `callbackThread`: Continuously processes incoming position data from Gazebo.
   - `pidControlThread`: Runs the PID control loop to compute and send velocity commands.

5. **User Input Handling**:
   - The user can input commands to set target positions, adjust PID gains, or directly control the drone's velocity.

---

### **How the Parts Work Together**
1. **Initialization**:
   - The ROS node is initialized, and publishers/subscribers are set up.
   - Threads are started to handle position updates and PID control.

2. **Position Updates**:
   - The `callbackThread` listens to Gazebo's `/gazebo/model_states` topic and updates the drone's current position.

3. **PID Control**:
   - The `pidControlThread` continuously computes velocity commands using the PID controllers and sends them to the drone.

4. **User Interaction**:
   - The user can input commands to set a target position, adjust PID gains, or directly control the drone's velocity.

5. **Termination**:
   - The system can be shut down gracefully using the `shutdown` flag.

---

### **Problem Being Solved**
The code solves the problem of **autonomous drone navigation** in a simulated environment. It ensures the drone can:
- Accurately reach a target position using PID control.
- Respond to user input in real-time.
- Operate efficiently in a multi-threaded environment.

---

### **Key Features**
1. **Modular Design**:
   - The PID controller is encapsulated in a class, making it reusable and easy to modify.

2. **Thread Safety**:
   - Shared data is protected using mutexes and atomic variables.

3. **Flexibility**:
   - The user can adjust PID gains, set target positions, or directly control the drone's velocity.

4. **Real-time Operation**:
   - The system runs continuously, updating the drone's position and computing velocity commands in real-time.

---

This code is a robust and flexible solution for controlling a drone in a simulated environment, combining PID control, ROS communication, and multi-threading to achieve smooth and accurate movement.