# **Gait Analysis: Video Processing, Angle Calculation & Data Interpolation**

## **Overview**
This script analyzes **human gait** using computer vision and data processing techniques. It extracts **joint and segment angles** from a video, saves the data in a CSV file, interpolates missing values, and visualizes the results.

## **1. Video Processing & Key Point Detection**
### **1.1 Loading and Extracting Frames**
- The script loads a **video file** based on user input (subject name and walking plane: left or right).
- It extracts frames from the video based on the **frames per second (FPS)** and processes a fixed duration.

### **1.2 Contour Detection & Key Point Extraction**
- Converts each frame to **grayscale** and applies **Gaussian blur** to reduce noise.
- Uses **binary thresholding** to segment the subject from the background.
- Detects **contours** (shapes) and finds **midpoints** of key body segments (trunk, thigh, leg, foot).
- Key points are sorted and selected based on their **spatial relationships**.

## **2. Angle Calculation & Interactive Display**
### **2.1 Computing Joint & Segment Angles**
- Calculates slopes between key points to determine:
  - **Joint angles:** Hip, knee, ankle.
  - **Segment angles:** Trunk, thigh, leg, foot.
- Ensures correct orientation based on the **left or right walking plane**.

### **2.2 Interactive Video Navigation**
- Overlays **angles and key points** on the video frames.
- Displays **frame numbers** for easy reference.
- Allows navigation using keyboard inputs (`f` for forward, `b` for backward, `q` to quit).

## **3. Data Interpolation & Visualization**
### **3.1 Handling Missing Data**
- The extracted gait angles are saved in a **CSV file**.
- Checks for **missing values** and fills them using **cubic spline interpolation** for a smooth, continuous dataset.

### **3.2 Plotting & Comparing Data**
- Creates **before-and-after plots** to visualize the raw data and interpolated data.
- Uses **subplot layouts, titles, labels, and grids** for clarity.

## **Conclusion**
This script provides a **comprehensive gait analysis** by automating **video processing, key point detection, angle calculation, data storage, and visualization**. The use of **cubic spline interpolation** ensures a **smooth representation** of gait angles, improving the accuracy of analysis.
