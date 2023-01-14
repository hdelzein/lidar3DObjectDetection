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

### Summary for Project Step Three. it is divided into two section. 

Task preparations:

In file loop_over_dataset.py, set the attributes for code execution in the following way:

- data_filename = 'training_segment-1005081002024129653_5313_150_5333_150_with_camera_labels.tfrecord
- show_only_frames = [50, 51]
- exec_detection = ['bev_from_pcl', 'detect_objects']
- exec_tracking = []
- exec_visualization = ['show_objects_in_bev_labels_in_camera']
- configs_det = det.load_configs(model_name="fpn_resnet")

## Section One. Add a second model from a GitHub repo (ID_S3_EX1)

In addition to Complex YOLO, extract the code for output decoding and post-processing from the GitHub repo.

Modify Code in object_detect.py within three functions:

Funtion One: 'load_configs_model'. Add configuration paramters

- Step 1: Line 61 - 103

Function Two: 'Create_model'. Instantiate a model

- Step 1: Line 157 - 165

Function Three: 'detect_objects'. Perform inference

- Step 1: Line 206 - 222

The outcome is the output of this poece of code:
- outputs['hm_cen'] = _sigmoid(outputs['hm_cen'])
- outputs['cen_offset'] = _sigmoid(outputs['cen_offset'])
- detections = decode(outputs['hm_cen'], outputs['cen_offset'], outputs['direction'], outputs['z_coor'],outputs['dim'], K=configs.K)
- detections = detections.cpu().numpy().astype(np.float32)
- detections = post_processing(detections, configs)
- detections = detections[0][1]  # only first batch
- print(detections)


## Section Two. Extract 3D bounding boxes from model response (ID_S3_EX2)

- Transform BEV coordinates in [pixels] into vehicle coordinates in [m]
- Convert model output to expected bounding box format [class-id, x, y, z, h, w, l, yaw]

Modify Code in object_detect.py within functions 'detect_objects':

- Step 1: Line 235
- Step 2: Line 237
- Step 3: Line 241

The outcome is a visualization of the bounding boxes:


<img src="img/Frame50_boundingBox.jpg"/>


### Summary for Project Step Fouir. it is divided into three section. 


Task preparations for section one and two:

In file loop_over_dataset.py, set the attributes for code execution in the following way:

- data_filename = 'training_segment-1005081002024129653_5313_150_5333_150_with_camera_labels.tfrecord
- show_only_frames = [50, 51]
- exec_detection = ['bev_from_pcl', 'detect_objects', 'validate_object_labels', 'measure_detection_performance']
- exec_tracking = []
- exec_visualization = ['show_detection_performance']
- configs_det = det.load_configs(model_name="darknet")

## Section One. Compute intersection-over-union between labels and detections (ID_S4_EX1)

This task involves writing code in the file student/objdet_eval.py for function 'measure_detection_performance'
It will perform the following:

- For all pairings of ground-truth labels and detected objects, compute the degree of geometrical overlap
- The function tools.compute_box_corners returns the four corners of a bounding box which can be used with the Polygon structure of the Shapely toolbox
- Assign each detected object to a label only if the IOU exceeds a given threshold
- In case of multiple matches, keep the object/label pair with max. IOU
- Count all object/label-pairs and store them as “true positives”

- Step 1: Line 51
- Step 2: Line 59
- Step 3: Line 63
- Step 4: Line 66
- Step 5: Line 71
- Step 6: Line 77


## Section Two. Compute false-negatives and false-positives (ID_S4_EX2)


This task involves writing code in the file student/objdet_eval.py for function 'measure_detection_performance'

- Step 1: Line 103
- Step 2: Line 107
- Step 3: Line 109
- Line 115 assemble the performance data into a list

## Section Three. Compute precision and recall (ID_S4_EX3)

Task preparations
In file loop_over_dataset.py, set the attributes for code execution in the following way:

data_filename = 'training_segment-1005081002024129653_5313_150_5333_150_with_camera_labels.tfrecord
show_only_frames = [50, 150]
exec_detection = ['bev_from_pcl', 'detect_objects', 'validate_object_labels', 'measure_detection_performance']
exec_tracking = []
exec_visualization = ['show_detection_performance']
configs_det = det.load_configs(model_name="darknet")

This task involves writing code in the file student/objdet_eval.py for function 'compute_performance_stats'

- Compute “precision” over all evaluated frames using true-positives and false-positives
- Compute “recall” over all evaluated frames using true-positives and false-negatives

- Step 1: Line 139
- Step 2: Line 141
- Step 3: Line 144

## Output for all of these three sections is shown in the image below:

<img src="img/histogram_one.png"/>

The outcome of the preciosn recall when configs_det.use_labels_as_objects = True is shown below:


<img src="img/histogram_two.png"/>