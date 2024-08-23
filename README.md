# Time-Shifted Live Camera Feed
This script captures video from your webcam and applies a time shift effect by delaying the display of frames.

# Code
```
import cv2
import collections
```
# Start the camera
```
cap = cv2.VideoCapture(0)
```
Start the Camera: Initializes the webcam using cv2.VideoCapture(0). The argument 0 refers to the default camera.

# Check if the camera opened successfully
```
if not cap.isOpened():
    print("Error: Cannot open camera!")
    exit()
```
Check Camera Status: Verifies if the camera was opened successfully. If not, an error message is printed, and the script exits.

# Get the camera FPS (frames per second)
```
fps = cap.get(cv2.CAP_PROP_FPS)
delay_time = 3  # 3-second delay
buffer_size = int(fps * delay_time)
```
Calculate Buffer Size: Retrieves the frames per second (FPS) of the camera and calculates the number of frames needed to create a 3-second delay.

# Create a frame buffer with a max size corresponding to the delay
```
frame_buffer = collections.deque(maxlen=buffer_size)
```
Initialize Frame Buffer: Creates a deque with a maximum length equal to the number of frames corresponding to the delay. This buffer stores frames for time shifting.
```
while True:
    # Read a frame from the camera
    ret, frame = cap.read()
```
Read Frames: Continuously reads frames from the camera inside a loop.
```
    # If the frame is not read correctly, exit the loop
    if not ret:
        print("Error: Cannot retrieve frame!")
        break
```
Check Frame Retrieval: If a frame cannot be read, prints an error message and breaks the loop.
```
    # Add the current frame to the buffer
    frame_buffer.append(frame)
```
Store Frames: Adds the current frame to the buffer. If the buffer is full, the oldest frame will be automatically removed.
```
    # If the buffer is full, display the oldest frame to create a delay effect
    if len(frame_buffer) == buffer_size:
        cv2.imshow('Time-Shifted Live Camera', frame_buffer[0])
    else:
        # If the buffer is not full yet, show the current frame
        cv2.imshow('Time-Shifted Live Camera', frame)
```
Display Frames:
If the buffer is full, the script displays the oldest frame from the buffer to create a time-shifted effect.
If the buffer is not full yet, it shows the current frame.
```
    # Exit the loop if the 'q' key is pressed
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
```
Exit Condition: The loop exits when the 'q' key is pressed.
# Release the camera and close all OpenCV windows
```
cap.release()
cv2.destroyAllWindows()
```
Clean Up: Releases the camera and closes all OpenCV windows after the loop exits.
How It Works
Initialization: Starts the camera and initializes the frame buffer.
Frame Capture: Continuously captures frames from the camera.
Buffer Management: Stores frames in a buffer to create a delay effect.
Display: Shows the delayed frame when the buffer is full, otherwise shows the current frame.
Exit: Closes the camera and windows when 'q' is pressed.
