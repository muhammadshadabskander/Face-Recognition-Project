# Real-Time Face Recognition Using OpenCV and face_recognition Library
This project demonstrates real-time face recognition using Python's OpenCV library, the face_recognition library, and a webcam. Below is a detailed guide on setting up, running, and publishing this project on GitHub, complete with visual outputs.

# Table of Contents 
Introduction
Project Setup
Install Required Libraries
Directory Structure
Code Explanation
Loading Images
Webcam Capture and Face Detection
Displaying Results
Running the Project
Sample Outputs
Publishing to GitHub
Step 1: Create a Repository
Step 2: Clone the Repository
Step 3: Add Your Project Files
Step 4: Commit and Push
# Conclusion
# Introduction
Face recognition technology has revolutionized security systems, attendance monitoring, and user authentication systems. This project uses Python libraries like OpenCV and face_recognition to build a real-time facial recognition system using your webcam. The program compares faces in the live video stream with preloaded images and identifies the people in the video.

# Project Setup
Install Required Libraries
Ensure that you have the required libraries installed on your system. You can install the necessary dependencies using pip:

bash

```pip install opencv-python numpy face_recognition```
Directory Structure
Set up your project directory as follows:

```
Face-Recognition-Project
   ├── /Images                
   ├── face_recognition.py     
   └── README.md
```         
The /Images folder contains images of people you want to recognize in the webcam feed.
The face_recognition.py script is the main program for detecting and recognizing faces.
Code Explanation
Let’s break down the core elements of the project.

# Loading Images
The script first loads images from the Images directory and converts them into encodings using the face_recognition library. These encodings are used for comparison with the faces captured from the webcam.

python
Copy code
```
path = 'Images'
images = []
className = []
myList = os.listdir(path)

for cl in myList:
    curImg = cv2.imread(f'{path}/{cl}')
    images.append(curImg)
    className.append(os.path.splitext(cl)[0])

def FindEncodings(images):
    encodeList = []
    for img in images:
        img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
        eccode = face_recognition.face_encodings(img)[0]
        encodeList.append(eccode)
    return encodeList

encodeListKnown = FindEncodings(images)
print('Encoding complete')
```
# Images Loading: The script reads images from the folder and stores them in the images list.
Encoding: The FindEncodings function generates face encodings from each image using the face_recognition.face_encodings() function.
Webcam Capture and Face Detection
The webcam is activated using cv2.VideoCapture(0). For each frame captured from the webcam, the face is located and encoded in real-time.

python
Copy code
```
cap = cv2.VideoCapture(0)

while True:
    success, img = cap.read()
    imgS = cv2.resize(img, (0, 0), None, 0.25, 0.25)
    imgS = cv2.cvtColor(imgS, cv2.COLOR_BGR2RGB)

    facesCurFrame = face_recognition.face_locations(imgS)
    encodesCurFrame = face_recognition.face_encodings(imgS, facesCurFrame)
```
# Resizing: The webcam frame is resized to 25% of its original size for faster processing.
Face Location: The face_locations function identifies the locations of faces in the frame.
Face Encoding: The face_encodings function encodes the detected faces for comparison.
Displaying Results
After comparing the detected face encodings with known face encodings, the results are displayed on the webcam feed. The recognized face's name is shown in a green bounding box, while unrecognized faces show an error message.

python
Copy code
```for encodeFace, faceLoc in zip(encodesCurFrame, facesCurFrame):
    matches = face_recognition.compare_faces(encodeListKnown, encodeFace)
    faceDis = face_recognition.face_distance(encodeListKnown, encodeFace)

    matchIndex = np.argmin(faceDis)

    if matches[matchIndex]:
        name = className[matchIndex].upper()
        y1, x2, y2, x1 = faceLoc
        y1, x2, y2, x1 = y1*4, x2*4, y2*4, x1*4

        cv2.rectangle(img, (x1, y1), (x2, y2), (0, 255, 0), 2)
        cv2.rectangle(img, (x1, y2-35), (x2, y2), (0, 255, 0), cv2.FILLED)
        cv2.putText(img, name, (x1+6, y2-6), cv2.FONT_HERSHEY_COMPLEX, 1, (255, 255, 255), 2)

    cv2.imshow('Webcam', img)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```
Running the Project
Place your images in the Images folder.
Run the script using the following command:
bash
Copy code
```
python face_recognition.py
```
Quit the script by pressing the 'q' key.
Sample Outputs
Below are visual outputs from running the face recognition project.

# 1. Recognizing a Known Face
When a known face is detected, the system draws a green bounding box around the face and displays the name.


# 2. Unrecognized Face
If the system doesn't recognize a face, a red message "Not verified" appears.

# Conclusion
This guide provided a complete walkthrough of creating and deploying a real-time face recognition system using Python. The project identifies faces using OpenCV and the face_recognition library and shows real-time results via webcam. You also learned how to publish this project on GitHub. Feel free to improve this project by adding features like multi-face recognition or database integration.
