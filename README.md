# fACE
===========================================

Overview
--------

The Interview Confidence Analyzer is a web-based application designed to help users practice and improve their interview skills by analyzing their eye contact and speech confidence in real-time. It leverages MediaPipe's Face Mesh library to track eye movement and provides feedback based on gaze stability, simulating natural eye contact in a virtual interview setting. The application runs entirely in the browser, ensuring privacy by processing all data locally.

### Key Features

-   Real-Time Eye Contact Scoring: Measures gaze stability using iris tracking, providing a percentage score.

-   Speech Confidence Placeholder: Displays a simulated speech confidence score (randomized for now).

-   Calibration Phase: Ensures proper setup of camera and audio before analysis.

-   Session History: Stores past session results locally and allows exporting as a CSV file.

-   Feedback System: Offers actionable feedback based on performance metrics.

-   Privacy-Focused: No data is sent to external servers; all processing is client-side.

### Current Version

-   Date: March 13, 2025 (aligned with the provided context)

-   Dependencies: MediaPipe Face Mesh (v0.4), MediaPipe Camera Utils (v0.3)

* * * * *

System Requirements
-------------------

-   Browser: Modern browsers with WebRTC support (e.g., Chrome, Firefox, Edge). Safari may require additional configuration.

-   Hardware:

-   Webcam for video input.

-   Microphone (optional, currently unused beyond calibration).

-   Internet: Required only for loading external MediaPipe scripts from CDN; no server communication during runtime.

* * * * *

Installation and Setup
----------------------

1.  Clone or Download the Code:

-   Save the provided HTML file (e.g., index.html) to your local machine.

3.  Host Locally (Optional):

-   For best results, serve the file via a local web server (e.g., using Python's http.server):\
    bash\
    CollapseWrapCopy\
    python -m http.server 8000

-   Access it at http://localhost:8000/index.html.

-   Alternatively, open directly in a browser, but some features (e.g., camera access) may require a secure context (HTTPS or localhost).

5.  Grant Permissions:

-   When prompted, allow browser access to the camera and microphone.

* * * * *

Usage Instructions
------------------

### 1\. Starting the Application

-   Open the HTML file in a compatible browser.

-   The interface displays a video feed, metrics, controls, and a history table.

### 2\. Calibration

-   Purpose: Initializes the camera and ensures the system is ready.

-   Steps:

1.  Click Start Calibration.

2.  Look forward at the camera and speak clearly for 5 seconds.

3.  Wait for the countdown to complete (status updates from "Calibrating..." to "Calibration complete").

-   Outcome: The "Start Analysis" button becomes enabled.

### 3\. Running Analysis

-   Steps:

1.  Click Start Analysis.

2.  Maintain a steady gaze toward the camera and speak as you would in an interview.

3.  Observe real-time updates to:

-   Eye Contact Score: Percentage based on gaze stability.

-   Speech Confidence: Randomized placeholder (0-100%).

-   Feedback: Color-coded messages (green = good, yellow = warning, red = poor).

-   Duration: Runs until stopped manually.

### 4\. Stopping Analysis

-   Click Stop Analysis to end the session.

-   Results are saved to local storage, and the "Export Results" button becomes available.

### 5\. Viewing History

-   Past session averages (date, eye contact, speech confidence) are displayed in the "Past Sessions" table.

-   Updated automatically after each session.

### 6\. Exporting Results

-   Click Export Results to download a CSV file (confidence_results.csv) containing all session data.

* * * * *

Technical Details
-----------------

### Architecture

-   HTML/CSS: Defines the user interface with a video feed, metrics display, and control buttons.

-   JavaScript: Handles logic, including:

-   MediaPipe integration for face and iris tracking.

-   Gaze stability calculation.

-   Real-time UI updates and session management.

-   External Libraries:

-   face_mesh.js: Tracks facial landmarks, including iris positions.

-   camera_utils.js: Manages webcam input for continuous frame processing.

### Core Components

#### 1\. Gaze Stability Calculation (calculateGazeStability)

-   Input: MediaPipe Face Mesh results (results.multiFaceLandmarks).

-   Process:

-   Tracks iris positions (landmarks 468 and 473) over a 30-frame window.

-   Computes variance in x and y coordinates.

-   Converts low variance (steady gaze) to a high score (max 100%).

-   Formula: score = max(0, 100 - totalVariance * 1000).

-   Tuning: Adjust the 1000 multiplier for sensitivity.

#### 2\. Real-Time Processing (onResults)

-   Triggered for each video frame processed by MediaPipe.

-   Updates eyeContactScore based on gaze stability.

-   Refreshes the UI with the latest score.

#### 3\. Feedback System (provideFeedback)

-   Logic:

-   Good (>70% eye, >70% speech): "Great job! Steady gaze and confident speech."

-   Warning (>50% eye, >50% speech): "Good effort. Try maintaining a steadier gaze."

-   Poor (otherwise): "Needs work. Keep your gaze steady and speak clearly."

-   Visuals: Green, yellow, or red background based on performance.

#### 4\. Session Management

-   Storage: Uses localStorage to persist history.

-   Export: Generates a CSV file from session data.

### File Structure

-   Single file (index.html) containing:

-   HTML for structure.

-   CSS for styling.

-   JavaScript for functionality.

-   External dependencies loaded via CDN.

* * * * *

Limitations
-----------

-   Speech Confidence: Currently a random placeholder. Real speech analysis (e.g., volume, clarity) requires additional implementation.

-   Device Variability: Gaze stability accuracy may vary with camera quality or lighting conditions.

-   Browser Compatibility: Requires WebRTC support; may not work fully in older browsers.

-   Sensitivity: The gaze stability score may need calibration per user or device.

* * * * *

Future Enhancements
-------------------

1.  Speech Analysis:

-   Integrate Web Audio API to measure volume, pitch, or pauses for a true speech confidence score.

3.  Visual Debugging:

-   Overlay iris positions on the video feed for user feedback.

5.  Customization:

-   Allow users to adjust sensitivity thresholds or feedback criteria.

7.  Multi-Face Support:

-   Extend to detect multiple faces (e.g., for group interviews).

9.  Performance Optimization:

-   Reduce frame processing rate if lag occurs on low-end devices.

* * * * *

Troubleshooting
---------------

-   Eye Contact Score Stuck at 0%:

-   Ensure camera permissions are granted.

-   Check console for MediaPipe errors (e.g., model loading issues).

-   Verify results.multiFaceLandmarks is populated in onResults.

-   No Video Feed:

-   Confirm browser supports navigator.mediaDevices.getUserMedia.

-   Test with localhost if running from file://.

-   Feedback Not Showing:

-   Ensure analysisInterval is running (check startAnalysis execution).

* * * * *

Privacy and Security
--------------------

-   Data Handling: All video and audio processing occurs locally in the browser.

-   Storage: Session history is stored in localStorage, accessible only on the user's device.

-   No Network Calls: Beyond initial CDN script loading, no data is transmitted.
