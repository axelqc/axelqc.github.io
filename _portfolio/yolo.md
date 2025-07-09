---
title: "Vehicle Count Using AI"
excerpt: "Imagine having many camares all over the city, thousands of GB of information, but don't have the necesarry time to view and count the vehicles, here is where AI comes in handy. <br/><img src='https://www.javeriana.edu.co/pesquisa/wp-content/uploads/2011/06/deteccion-y-conteo-vehicular.jpg'>"
collection: portfolio
---
This project purpose was to analyze many videos and extract the vehicle count in order to indentify denser areas and propose solutions when a natural disaster comes.

# Problem statement
------------------------------------
Currently, there is a significant amount of video footage available from fixed cameras monitoring a specific region. However, this data remains largely untapped. The absence of an automated system to analyze this footage and quantify vehicle types—such as cars, motorcycles, and trucks—limits the ability to make informed decisions during emergencies. There is a need for a scalable solution that can process this video data, count different types of vehicles accurately, and display the results in an interactive and user-friendly dashboard to support disaster response and urban planning efforts.

# Introduction
-----------------------
Natural disasters and emergency situations demand swift and informed responses, often relying on accurate traffic and mobility data. Traditional manual analysis of traffic footage is time-consuming and inefficient, especially when dealing with a large volume of video files from multiple surveillance cameras. To address this challenge, this project proposes an automated system that utilizes computer vision techniques to process video footage from a defined area, classify and count vehicles—including motorcycles, cars, and trucks—and present the results on an interactive web platform.

This system will not only aid in understanding traffic patterns under normal and crisis conditions but also support decision-making for evacuation planning, resource allocation, and infrastructure improvements. By transforming raw video data into actionable insights, the solution aims to enhance situational awareness and disaster preparedness for both public authorities and urban planners.

# Solution
-------------------
## 1. Vehicle Detection and Counting using YOLO
At the core of the solution is the YOLO (You Only Look Once) object detection algorithm, implemented in Python. YOLO was selected for its speed and accuracy in real-time object detection tasks. The model was fine-tuned or configured to recognize and count three specific vehicle types: cars, motorcycles, and trucks.

For each video:

- The YOLO model was applied frame-by-frame to detect vehicles.

- Object tracking methods were used to prevent double-counting.

- Detected vehicles were classified and stored along with their timestamps and frame location.

## 2. Automated Batch Processing with os and Temporary File Handling
Given the large number of videos, the system was designed to process zipped video archives automatically:

- Python's os and zipfile modules were used to locate and extract ZIP files temporarily.

- Once extracted, videos were processed one by one.

- After analysis, both the temporary video files and their folders were automatically deleted to conserve disk space and maintain system performance.

## 3. Aggregation and Reporting
After processing each video:

- Results (vehicle counts, types, camera ID, timestamp, etc.) were stored in structured format using Pandas.

- Data from all processed videos was appended to a master Excel file, which served as a centralized report for further analysis.

- The file included summaries and raw detection logs, ready to be used by analysts or urban planners.

## 4. Interactive Web Dashboard
To visualize the results and support decision-making:

- A web application was developed using technologies such as HTML, CSS, JavaScript, and Leaflet.js.

- A map interface showed the geographic locations of each surveillance camera.

- Users could click on camera markers to view specific statistics from that location.

- The dashboard featured interactive graphs (e.g., bar charts, time series) to display:

    * Vehicle type distribution

    * Peak traffic hours

   * Vehicle counts per location and time

This interface provided an intuitive way to explore traffic patterns and identify potential bottlenecks or critical routes—vital information in emergency planning and disaster response.


# Results

## Vehicle Counting in YOLO
Here's a little video:
\url{https://youtu.be/YMBiUZrCN34}

## Web dashboard
Here's the demo:
\url{https://youtu.be/lKX7TLb4vC0}
