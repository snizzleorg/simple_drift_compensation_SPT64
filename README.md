# **Simple drift compensation for point measurements with SymPhoTime64**

## **Overview**
This script, written in a **PicoQuant SPT64 scripting language**, is designed to perform **fluorescence lifetime imaging (FLIM) and time-correlated single-photon counting (TCSPC) measurements** using a **PI-Scanner** (Piezo Scanner) and a **TCSPC module**. It systematically scans points and images, analyzes intensity, and determines new scanning positions dynamically.

---

## **Key Components**

### **1. Device Initialization and Setup**
- The script starts by obtaining instances of required hardware:
  - **`MyTCSPC`** → TCSPC device (for photon counting)
  - **`Scan`** → PI-Scanner (for positioning and imaging)
  - **`WS`** → Workspace for storing measurement data
- It verifies if both devices are properly connected.

### **2. Measurement Methods**
The script defines three core measurement functions:

#### **2.1 `WaitForMeasEnd(WithScanner: Bool)`**  
- Waits for the measurement to complete, ensuring both TCSPC and Scanner finish their processes.

#### **2.2 `MeasurePoint(x: Float; y: Float; Time_in_ms: Int)`**  
- Moves the scanner to a specific `(x, y)` position and records TCSPC data for `Time_in_ms`.
- Data is saved in a `.ptu` file.

#### **2.3 `MeasureImage(x: Float; y: Float; Size: Float; Pix: Int; Frames: Int)`**  
- Scans an area around `(x, y)`, capturing a **fluorescence intensity image**.
- Uses the fastest scanner speed, saving results as `.ptu` files.

---

## **3. Image Analysis and Adaptive Measurement**
### **`MagicHappens()`**
- **Analyzes** the acquired image to identify the **maximum intensity point**.
- Uses **image metadata** (pixel resolution, offsets) to determine new coordinates `(Nx, Ny)`.
- This dynamically updates the next measurement location.

---

## **4. Main Measurement Loop (`DoMeasurementProgram()`)**
- Repeats the following **4 times**:
  1. Measure a **single point**.
  2. Measure a **small image region** around that point.
  3. Analyze the image and adjust **Nx, Ny** dynamically for the next iteration.
- Saves the results in `.ptu` files.

---

## **Execution Flow**
1. **Initialize Devices & Check Connectivity**
   - Ensures both **TCSPC** and **PI-Scanner** are ready.
   - Retrieves the **current workspace directory** for storing data.

2. **Run the Measurement Cycle**
   - Starts with `(Nx, Ny) = (0,0)`.
   - Alternates between **point measurement** and **image scanning**.
   - Dynamically determines the next measurement point based on the **brightest detected area**.

3. **End of Execution**
   - Closes all files and outputs `"Measurement Done"`.

---

## **Main Purpose**
This script automates a **fluorescence lifetime measurement process** by:
- Scanning an initial **point** and **image**.
- Analyzing the **intensity distribution**.
- Moving to a new position based on the **brightest detected area**.
- Iterating this process **4 times**.

This approach is useful for experiments requiring **adaptive scanning**—for example, detecting **fluorescent molecules** or **biological samples** in microscopy.


---

## **Disclaimer**
This script is **experimental code** provided **as-is** with **no warranties or guarantees of correctness**.  
It is intended for demonstration purposes only. **Use at your own risk**.  
**No official support** is provided for this script.
