# Real Time People Counter
People Counting in Real-Time using live video stream. SSD Detector and Centroid Tracker are being used.

<div align="center">
<img src=https://imgur.com/SaF1kk3.gif" width=550>
<p>Live demo</p>
</div>

- Use case: The main use case of this project is to count the number of people in the stores/buildings/shopping malls etc., in real-time.

--- 

## Table of Contents

* [Simple Theory](#simple-theory)
    - [SSD detector](#ssd-detector)
    - [Centroid tracker](#centroid-tracker)
* [Running Inference](#running-inference)
    - [Install the dependencies](#install-the-dependencies)
    - [Test video file](#test-video-file)
    - [Webcam](#webcam)
    - [IP camera](#ip-camera)
* [Features](#features)
    - [Real-Time alert](#real-time-alert)
    - [Threading](#threading)
    - [Scheduler](#scheduler)
    - [Timer](#timer)
    - [Simple log](#simple-log)
* [References](#references)

---

## Simple Theory

### SSD detector

- I used a SSD ```Single Shot Detector``` with a MobileNet architecture. 

### Centroid tracker

- Centroid tracker is one of the most reliable trackers out there.
- Centroid tracker computes the ```centroid``` of the bounding boxes.
- Once the co-ordinates are obtained by our SSD, the tracker computes the centroid (center) of the box. In other words, the center of an object.
- Then an ```unique ID``` is assigned to every particular object deteced, for tracking over the sequence of frames.

---

## Running Inference

### Install the dependencies

First up, install all the required Python dependencies by running: ```
pip install -r requirements.txt ```

### Test video file

To run inference on a test video file, head into the root directory and run the command: 

```
python people_counter.py --prototxt detector/MobileNetSSD_deploy.prototxt --model detector/MobileNetSSD_deploy.caffemodel --input utils/data/tests/test_1.mp4
```

### Webcam

To run on a webcam, set ```"url": 0``` in ```utils/config.json``` and run the command:

```
python people_counter.py --prototxt detector/MobileNetSSD_deploy.prototxt --model detector/MobileNetSSD_deploy.caffemodel
```

### IP camera

To run on an IP camera, setup your camera url in ```utils/config.json```, e.g., ```"url": 'http://191.138.0.100:8040/video'```.

Then run the command:
```
python people_counter.py --prototxt detector/MobileNetSSD_deploy.prototxt --model detector/MobileNetSSD_deploy.caffemodel
```

---

## Features

The following features can be easily enabled/disabled in ```utils/config.json```:

```json
{
    "Email_Send": "",
    "Email_Receive": "",
    "Email_Password": "",
    "url": "",
    "ALERT": false,
    "Threshold": 10,
    "Thread": false,
    "Log": false,
    "Scheduler": false,
    "Timer": false
}
```

### Real-Time alert

If selected, we send an email alert in real-time. Example use case: If the total number of people (say 10 or 30) are exceeded in a store/building, we simply alert the staff. 

- You can set the max. people limit in config, e.g., ```"Threshold": 10```.
- This is quite useful considering scenarios similar to COVID-19. Below is an example:
<img src="https://imgur.com/35Yf1SR.png" width=350>

> ***1. Setup your emails:***

In the config, setup your sender email ```"Email_Send": ""``` to send the alerts and your receiver email ```"Email_Receive": ""``` to receive the alerts.

> ***2. Setup your password:***

Similarly, setup the sender email password ```"Email_Password": ""```.

### Threading

- Multi-Threading is implemented in ```utils/thread.py```. If you ever see a lag/delay in your real-time stream, consider using it.
- It is most suitable to get solid performance on complex real-time applications. To use threading: set ```"Thread": true,``` in config.

### Scheduler

- Automatic scheduler to start the software. Configure to run at every second, minute, day, or workdays e.g., Monday to Friday.

```python
# runs at every day (09:00 am)
schedule.every().day.at("9:00").do(run)
```

### Timer

- Configure stopping the software execution after a certain time, e.g., 30 min or 8 hours (currently set) from now.
- All you have to do is set your desired time and run the script.

```python
# automatic timer to stop the live stream (set to 8 hours/28800s)
end_time = time.time()
num_seconds = (end_time - start_time)
if num_seconds > 28800:
    break
```

### Simple log

- Logs the counting data at end of the day.
- Useful for footfall analysis. Below is an example:
<img src="https://imgur.com/CV2nCjx.png" width=400>

---

## References

***Main:***

- SSD paper: https://arxiv.org/abs/1512.02325
- MobileNets paper: https://arxiv.org/abs/1704.04861
- Centroid tracker: https://www.pyimagesearch.com/2018/07/23/simple-object-tracking-with-opencv/

***Optional:***

- Object detection with SSD/MobileNets: https://pyimagesearch.com/2017/09/11/object-detection-with-deep-learning-and-opencv/
- Schedule: https://pypi.org/project/schedule/

---
