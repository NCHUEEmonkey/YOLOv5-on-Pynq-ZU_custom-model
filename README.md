# YOLOv5-on-Pynq-ZU

This repository contains the implementation and deployment of YOLOv5 on the Pynq-ZU FPGA board using Vitis AI.

Table of Contents
Project Overview
Modifying the Activation Function
FPGA Architecture: PS, PL, and DPU
Deployment Process
System Collaboration
Advantages and Benefits
Project Overview
Objective: Deploying YOLOv5, a popular object detection model, on the Pynq-ZU FPGA board using Vitis AI to leverage hardware acceleration for real-time inference.

Challenges: FPGAs are powerful but require specific optimizations, such as replacing the SiLU activation function with a more FPGA-friendly alternative like Leaky ReLU, to ensure compatibility and performance.

Modifying the Activation Function
SiLU to Leaky ReLU: SiLU (Swish) is computationally intensive due to its non-linear sigmoid component. Leaky ReLU, on the other hand, is simpler and more efficient for FPGA implementation, as it involves basic linear operations.

Code Modification: In models/common.py, replace instances of SiLU with Leaky ReLU by altering the class definition and ensuring all relevant layers use this new function.

Training Considerations: After modifying the activation function, the model’s performance might change. It’s advisable to retrain or fine-tune the model to restore or enhance its accuracy.

FPGA Architecture: PS, PL, and DPU
PS (Processing System): Comprises embedded processors, like ARM Cortex-A, responsible for running the operating system (e.g., Linux), managing peripherals, and handling general computation tasks.

PL (Programmable Logic): This section contains the reconfigurable FPGA fabric, where custom digital circuits, including the DPU, can be implemented. It handles specific acceleration tasks like neural network inference.

DPU (Deep Learning Processing Unit): An IP core within the PL designed for high-performance deep learning inference. It accelerates the execution of convolutional layers, pooling, and fully connected layers typically used in CNNs.

Deployment Process
Model Preparation: Convert the PyTorch YOLOv5 model to ONNX format. Then, use the Vitis AI quantizer to reduce the model’s precision (e.g., to 8-bit integers), making it more suitable for FPGA processing.

Compilation: Use the Vitis AI compiler to transform the quantized model into an FPGA-compatible format, generating an ELF file that the DPU can execute.

Deployment: Transfer the compiled model to the Pynq-ZU board. A Python script, leveraging the Vitis AI runtime, will load the model and run inference on input data through the DPU.

Optimization: Fine-tune the system by adjusting parameters, such as batch size or clock frequencies, to maximize inference speed and minimize power consumption without sacrificing accuracy.

System Collaboration
Data Flow: The PS manages overall system operations, such as capturing input data (e.g., images from a camera), and prepares it for processing.

Inference Execution: The PL, with the DPU, receives the data from the PS and executes the YOLOv5 inference at high speed, thanks to the parallel processing capabilities of the FPGA.

Result Handling: After inference, the DPU outputs the results (e.g., detected objects and their bounding boxes) back to the PS, which then processes and displays or stores the results.

Advantages and Benefits
Real-time Performance: By utilizing the DPU in the PL, the system can perform object detection tasks much faster than a CPU or even some GPUs, making it ideal for real-time applications like surveillance, autonomous driving, or robotics.

Energy Efficiency: FPGAs are known for their energy efficiency, particularly when executing highly parallelizable tasks like CNN inference. This makes the Pynq-ZU a suitable platform for embedded systems where power consumption is critical.

Flexibility: The reconfigurability of the PL allows for continual optimization and customization of the model, making it adaptable to different use cases or new models without changing the hardware.

This comprehensive approach ensures that deploying YOLOv5 on the Pynq-ZU FPGA not only achieves high performance but also maintains flexibility and efficiency, making it a robust solution for various real-time object detection scenarios.
