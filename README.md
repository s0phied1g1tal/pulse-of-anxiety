# Pulse of anxiety

**Sophie Van Schil**

The installation is a symbolic representation of the inner struggle of someone with social anxiety disorder. The heart, the central element of the installation, beats as a metaphor for the inner turmoil and tension experienced by people with social anxiety. Through interactive elements such as proximity sensors, pointing gestures, and the number of people in front of the artwork, the heart responds to the presence and interaction of the audience. The installation aims to raise awareness and empathy for the challenges of people with social anxiety and encourages viewers to pay more attention to their interactions with others, understanding the invisible struggles that others may experience.

My installation is created with HTML, CSS, and JavaScript placed in a Vite project. External libraries were also used, such as TensorFlow models, which helped me piece together my code. Node-Red connected to my Raspberry Pi was used for the movement of the heart.

# Tools
[![HTML](https://img.shields.io/badge/-HTML-orange?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS](https://img.shields.io/badge/-CSS-blue?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/-JavaScript-yellow?style=for-the-badge&logo=javascript&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Figma](https://img.shields.io/badge/-Figma-purple?style=for-the-badge&logo=figma&logoColor=white)](https://www.figma.com/)
[![TensorFlow](https://img.shields.io/badge/-TensorFlow-orange?style=for-the-badge&logo=tensorflow&logoColor=white)](https://www.tensorflow.org/)
[![Node-RED](https://img.shields.io/badge/-Node--RED-red?style=for-the-badge&logo=node.js&logoColor=white)](https://nodered.org/)
[![GitHub](https://img.shields.io/badge/-GitHub-black?style=for-the-badge&logo=github&logoColor=white)](https://github.com/)

# Materials
* a frame (40x40 cm)
* a webcam ( Hama c-400 webcam)
* Raspberry Pi
* HDMI cable
* Rotating platform mechanism (or Rotating servo motor setup)
* materials to craft a heart, to your liking ( I used a silicone halloween mask and formo clay)
  
# Research
I wanted this project to be easy on the eyes yet mentally engaging. Working with a theme surrounding social anxiety required careful research to ensure it genuinely represented the struggles of individuals with this disorder.

I researched behaviors in the DSM5 that people with social anxiety find discomforting and identified interactions that induce anxiety. I concluded that issues such as close proximity, critical body language like pointing, and being surrounded by large groups were the main concerns. So, I decided to incorporate these aspects into my project.

# Project Overview

## Research
I wanted this project to be aesthetically pleasing while also thought-provoking. Exploring the theme of social anxiety required careful research to ensure it authentically represented the experiences of those affected by this disorder. I researched behaviors disliked by people with social anxiety, such as close proximity, critical body language like pointing, and being surrounded by large groups. These findings influenced the design of the project.

## Sketches
Sketches played a crucial role in shaping the final product. I started by sketching ideas and creating a mood board to visualize the aesthetic direction. The goal was for the installation to blend seamlessly into its surroundings, revealing its true nature only upon interaction. The heart was designed to look realistic and grim, set against a technical background of circuits and motherboards.

## Construction
I began by constructing the heart. Using a small balloon and a silicone Halloween clown mask, I created the base of the heart. Formo clay was used for the top, and a hot glue gun was used to add realistic veins. After painting with acrylics and applying a mod podge varnish, the heart looked slimy and slippery. I then placed the heart within a cheap Ikea frame (30x40) alongside circuit boards.

## Code
### Hand Detection
Hand detection was essential to capture user interactions such as pointing or touching, which would increase the heartbeat simulation. I implemented this using the MediaPipe Handpose library from TensorFlow, which provides a palm detector and hand-skeleton finger tracking model.

In the code above i start with importing the handpose models. Then i added the function so it detects when there is a hand detected an when there isnt. when a hand would be detected i wanted the interface to say “hand detected” 

### Proximity Detection
This function calculates the proximity of a person to the camera, crucial for adjusting heartbeat speed based on perceived interaction levels. It determines the distance between the center of the video frame and the center of the person's bounding box.
```javascript
// Function to calculate proximity
function calculateProximity(bbox, videoElement) {
    const videoWidth = videoElement.videoWidth;
    const videoHeight = videoElement.videoHeight;
    const centerX = videoWidth / 2;
    const centerY = videoHeight / 2;
    const faceCenterX = bbox[0] + bbox[2] / 2;
    const faceCenterY = bbox[1] + bbox[3] / 2;
    const distance = Math.sqrt(Math.pow(centerX - faceCenterX, 2) + Math.pow(centerY - faceCenterY, 2));
    return distance;
}
```

The function calculates the proximity of the detected person to the camera by determining the distance between the center of the video frame and the center of the person's bounding box.
It uses the width and height of the video frame to determine the center ```javascript (centerX, centerY)```.

Then, it calculates the center of the person's bounding box ```javascript (faceCenterX, faceCenterY)``` by adding half of the width and height to the top-left coordinates of the bounding box.
Finally, it calculates the distance between these two points using the Euclidean distance formula ```javascript Math.sqrt(Math.pow(centerX - faceCenterX, 2) + Math.pow(centerY - faceCenterY, 2))```
### Amount of People
This function counts the number of people detected in the camera frame, informing the social context and influencing heartbeat speed adjustments.
```javascript
// Function to handle hand and person detection results
function handleHandAndPersonResults(handResults, personResults) {
    let personCount = 0;
    for (const prediction of personResults) {
        if (prediction.class === 'person') {
            personCount++;
        }
    }
    // Logic to handle the number of people detected
}
```
The function iterates through each prediction in ```javascript personResults ``` using a loop ```javascript (for (const prediction of personResults))```.
For each prediction, it checks if the class label is 'person' ```javascript(if (prediction.class === 'person'))```.
If the label matches 'person', it increments a counter variable ```javascript(personCount++)```, indicating the presence of a person in the frame.

For both of these functions the same models are used .
The handpose model is imported from the @tensorflow-models/handpose package. This model is responsible for detecting and estimating the positions of hands in the camera frame.
```javascript 
import * as cocoSsd from '@tensorflow-models/coco-ssd';
```
This import statement brings in the handpose model functionality, allowing the code to utilize its methods for hand detection and tracking.
COCO-SSD Model:
The COCO-SSD model is imported from the @tensorflow-models/coco-ssd package. This model is used for general object detection, including the detection of people in the camera frame.
```javascript
import * as cocoSsd from '@tensorflow-models/coco-ssd';
```
This import statement imports the COCO-SSD model, enabling the code to perform object detection and identify people among other objects in the camera feed.

### Movement of Heart
The heart's movement is controlled by a servo motor connected to a Raspberry Pi. I used Node-RED to synchronize the motor's movement with the heartbeat simulation, facilitated through a WebSocket connection.

## WebSocket Implementation
WebSocket communication was employed to enable real-time interaction with the project. A WebSocket connection was established with a server, allowing bidirectional communication. Messages sent and received through the WebSocket facilitated various interactions and adjustments within the project.

A WebSocket connection is established with a server using the WebSocket constructor. In this case, the WebSocket server is located at the address <b>"ws://192.168.100.1:1880/bpm".</b>
```javascript 
var ws = new WebSocket("ws://192.168.100.1:1880/bpm");
```
This line creates a new WebSocket instance ```javascript  (ws)``` and establishes a connection to the WebSocket server at the specified URL ```javascript("ws://192.168.100.1:1880/bpm")```.

WebSocket Events:
Event handlers are attached to the WebSocket instance to handle different WebSocket events such as ```javascript onopen``` and ```javascript onmessage ```.
```javascript 
ws.onopen = function() { // Code to execute when the WebSocket connection is successfully opened };
ws.onmessage = function (evt) { // Code to execute when a message is received from the WebSocket server };
 ```
The onopen event handler is triggered when the WebSocket connection is successfully established with the server. Any code within this handler executes when the connection is open.
The onmessage event handler is triggered when a message is received from the WebSocket server. The received message is available in the evt parameter, and the code within this handler processes the received message.
WebSocket Communication:
Once the WebSocket connection is open```javascript(onopen event) ```, a message is sent to the server using the ```javascript send()``` method.
```javascript 
ws.onopen = function() { ws.send("Hello websocket server!"); };
 ```
In this example, the message ```javascript "Hello websocket server!" ``` is sent to the server when the WebSocket connection is opened.
Handling Received Messages:
When a message is received from the WebSocket server ```javascript (onmessage event) ```, a callback function is executed to handle the received message. 
```javascript
ws.onmessage = function (evt) { console.log(evt.data); };
 ```
In this example, the received message is logged to the console using console.log(). You can replace this line with any custom logic to process the received message as needed.



