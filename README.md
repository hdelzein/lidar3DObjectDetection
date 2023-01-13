### Lidar 3D Object Detetction Project

This is a summary of the implementation of the results of Lidar 3D object detection project

### Summary for Project Step One. it is divided into two section. Below is the result for each section

## Section One. Visualize range image channels (ID_S1_EX1)

I followed the instructions below:

In file loop_over_dataset.py, set the attributes for code execution in the following way:

- 1- data_filename = 'training_segment-1005081002024129653_5313_150_5333_150_with_camera_labels.tfrecord
- 2- show_only_frames = [0, 1]
- 3- exec_detection = []
- 4- exec_visualization = ['show_range_image']



I implemented the show_range_image function located in the file student/objdet_pcl.py.

- Convert range image “range” channel to 8bit 
- Convert range image “intensity” channel to 8bit 
- Crop range image to +/- 90 deg. left and right of the forward-facing x-axis
- Stack cropped range and intensity image vertically and visualize the result using OpenCV

Implmented these requirements based on the steps in the function 'show_range_image'

- Step 1 Line 76
- Step 2 Line 84
- Step 3 Line 89
- Step 4 Line 94
- Step 5 Line 104
- Step 6 Line 114

The outcome is a visualization of the range image

<img src="img/range_img.jpg"/>

## Section Two. Visualize Point Cloud ((ID_S1_EX2)

Task preparations
In file loop_over_dataset.py, set the attributes for code execution in the following way:

- data_filename = 'training_segment-10963653239323173269_1924_000_1944_000_with_camera_labels.tfrecord'
- show_only_frames = [0, 200]
- exec_detection = []
- exec_tracking = []
- exec_visualization = ['show_pcl']

I implemented the show_pcl function located in the file student/objdet_pcl.py

- Step 1 Line 46
- Step 2 Line 52
- Step 3 Line 56
- Step 4 Line 59 


The outcome is a visualization is 


### Summary for Project Step Two. it is divided into three section. 

Task preparations

In file loop_over_dataset.py, set the attributes for code execution in the following way:

- data_filename = 'training_segment-1005081002024129653_5313_150_5333_150_with_camera_labels.tfrecord
- show_only_frames = [0, 1]
- exec_detection = ['bev_from_pcl']
- exec_tracking = []
- exec_visualization = []

The outcome of this task involves writing code within the function 'bev_from_pcl' located in the file student/objdet_pcl.py.

## Section one. Create BEV map

Convert coordinates in x,y [m] into x,y [pixel] based on width and height of the bev map


Convert sensor coordinates to BEV-map coordinates (ID_S2_EX1)

- Step 1 - Line 149
- Step 2 - Line 152
- Step 3 - Line 157
- Step 4 - Line 164



The outcome is a visualization of the intensity image

<img src="img/bev.jpg"/>

## Section two. Compute intensity layer of the BEV map (ID_S2_EX2)

- Assign lidar intensity values to the cells of the bird-eye view map
- Adjust the intensity in such a way that objects of interest (e.g. vehicles) are clearly visible

Compute intensity layer of the BEV map (ID_S2_EX2) function 'bev_from_pcl':

- Step 1 - Line 182
- Step 2 - Line 184
- Step 3 - Line 198
- Step 4 - Line 195
- Step 5 - Line 201


The outcome is a visualization of the intensity image

<img src="img/intensity_img.jpg"/>

## Section three. Compute height layer of the BEV map (ID_S2_EX3)

- Make use of the sorted and pruned point-cloud lidar_pcl_top from the previous task
- Normalize the height in each BEV map pixel by the difference between max. and min. height
- Fill the "height" channel of the BEV map with data from the point-cloud

Compute height layer of the BEV map (ID_S2_EX3) in function 'bev_from_pcl':

- Step 1 - Line 218
- Step 2 - Line 220
- Step 3 - Line 227

The outcome is a visualization of the height image

<img src="img/height_img.jpg"/>





### Waymo Open Dataset Files
This project makes use of three different sequences to illustrate the concepts of object detection and tracking. These are: 
- Sequence 1 : `training_segment-1005081002024129653_5313_150_5333_150_with_camera_labels.tfrecord`
- Sequence 2 : `training_segment-10072231702153043603_5725_000_5745_000_with_camera_labels.tfrecord`
- Sequence 3 : `training_segment-10963653239323173269_1924_000_1944_000_with_camera_labels.tfrecord`

To download these files, you will have to register with Waymo Open Dataset first: [Open Dataset – Waymo](https://waymo.com/open/terms), if you have not already, making sure to note "Udacity" as your institution.

Once you have done so, please [click here](https://console.cloud.google.com/storage/browser/waymo_open_dataset_v_1_2_0_individual_files) to access the Google Cloud Container that holds all the sequences. Once you have been cleared for access by Waymo (which might take up to 48 hours), you can download the individual sequences. 

The sequences listed above can be found in the folder "training". Please download them and put the `tfrecord`-files into the `dataset` folder of this project.


### Pre-Trained Models
The object detection methods used in this project use pre-trained models which have been provided by the original authors. They can be downloaded [here](https://drive.google.com/file/d/1Pqx7sShlqKSGmvshTYbNDcUEYyZwfn3A/view?usp=sharing) (darknet) and [here](https://drive.google.com/file/d/1RcEfUIF1pzDZco8PJkZ10OL-wLL2usEj/view?usp=sharing) (fpn_resnet). Once downloaded, please copy the model files into the paths `/tools/objdet_models/darknet/pretrained` and `/tools/objdet_models/fpn_resnet/pretrained` respectively.

### Using Pre-Computed Results

In the main file `loop_over_dataset.py`, you can choose which steps of the algorithm should be executed. If you want to call a specific function, you simply need to add the corresponding string literal to one of the following lists: 

- `exec_data` : controls the execution of steps related to sensor data. 
  - `pcl_from_rangeimage` transforms the Waymo Open Data range image into a 3D point-cloud
  - `load_image` returns the image of the front camera

- `exec_detection` : controls which steps of model-based 3D object detection are performed
  - `bev_from_pcl` transforms the point-cloud into a fixed-size birds-eye view perspective
  - `detect_objects` executes the actual detection and returns a set of objects (only vehicles) 
  - `validate_object_labels` decides which ground-truth labels should be considered (e.g. based on difficulty or visibility)
  - `measure_detection_performance` contains methods to evaluate detection performance for a single frame

In case you do not include a specific step into the list, pre-computed binary files will be loaded instead. This enables you to run the algorithm and look at the results even without having implemented anything yet. The pre-computed results for the mid-term project need to be loaded using [this](https://drive.google.com/drive/folders/1-s46dKSrtx8rrNwnObGbly2nO3i4D7r7?usp=sharing) link. Please use the folder `darknet` first. Unzip the file within and put its content into the folder `results`.

- `exec_tracking` : controls the execution of the object tracking algorithm

- `exec_visualization` : controls the visualization of results
  - `show_range_image` displays two LiDAR range image channels (range and intensity)
  - `show_labels_in_image` projects ground-truth boxes into the front camera image
  - `show_objects_and_labels_in_bev` projects detected objects and label boxes into the birds-eye view
  - `show_objects_in_bev_labels_in_camera` displays a stacked view with labels inside the camera image on top and the birds-eye view with detected objects on the bottom
  - `show_tracks` displays the tracking results
  - `show_detection_performance` displays the performance evaluation based on all detected 
  - `make_tracking_movie` renders an output movie of the object tracking results

Even without solving any of the tasks, the project code can be executed. 

The final project uses pre-computed lidar detections in order for all students to have the same input data. If you use the workspace, the data is prepared there already. Otherwise, [download the pre-computed lidar detections](https://drive.google.com/drive/folders/1IkqFGYTF6Fh_d8J3UjQOSNJ2V42UDZpO?usp=sharing) (~1 GB), unzip them and put them in the folder `results`.

