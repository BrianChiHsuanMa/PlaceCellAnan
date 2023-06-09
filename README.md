# Inscopix_PlaceCell
This code processes data generated by the Inscopix system and DeepLabCut and calculates place maps for place cells.

The code assumes the following configuration has been used during data collection:
- Inscopix Data Acquisition Software configured to initiate calcium activity recording by BNC trigger input.
- Video capturing device configured to send out BNC signal to Inscopix DAQ box at recording start.

The following recording configurations are supported:
![Example recording configurations](/images/Inscopix%20recording.png)

## Installation
1. Clone this repository by running `git clone git@github.com:BrianChiHsuanMa/Inscopix_PlaceCell.git`, then change directory with `cd Inscopix_PlaceCell`.
2. Then install the environment with `conda env create -f isxplc.yml`.

## Getting started
Before you start, you should save the following files in the same folder and name them in the following format:
- **date_GPIO.csv**: The GPIO file generated by Inscopix. This can be from a single session or a time series.
- **date_Traces.csv OR date_Traces_Denoise.csv**: The raw or denoised calcium traces file generated by Inscopix. This can be from a single session or a time series.
- **date_Events.csv**: The event detection (spiking) file generated by Insxopix. This can be from a single session or a time series.
- **xxxshufflexxx.csv**: The motion detection results generated by DeepLabCut. The default naming logic by DeepLabCut is recommended. One or multiple files can be used based on your recording schedule.

Open up 'main.py' and edit the values marked with 'Manual input' to match your settings.
```
'''
Manual input
Modify these to match your settings
'''
data_dir = 'C:\\Users\\owner\\PlaceCell\\SampleData' # Where the data are stored
x_lens = [490, 490, 490, 490, 490] # Length of actual arena on x-axis, for each session
x_bounds = [60, 75, 60, 80, 70] # Left boundary of the actual arena, for each session
y_lens = [480,480,480,480,480] # Length of actual arena on y-axis, for each session
y_bounds = [0, 0, 0, 0, 0] # Upper boundary of the actual arena, for each session
px2cm = 490/30 # Conversion ratio between pixel and cm
x_res, y_res = 640, 480 # X- and y- resolution of the captured video
```
- **x_lens** and **y_lens** refer to the length of the actual arena, in pixels. It is likely that your camera is positioned to capture a larger area than the actual arena. These values could be measured with ImageJ (or something equivalent) using screenshots of the captured videos, or the labeled images generated by DeepLabCut.
- Similarly, **x_bounds** and **y_bounds** refer to the left and upper boundaries of the arena in relation to the area captured in the videos.
- For the values mentioned above, measurements should be taken for each **video recording session** included in the analysis.

## Run
To run the code, first activate the installed conda environment with `conda activate isxplc`, then run `python main.py` to process the data.

The code will generate some output files (.csv files and plots), which will be stored underneath a newly created folder **data_dir\\processed**.
