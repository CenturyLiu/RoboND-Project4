# RoboND-Project4
Implementation of the mapping project of the Udacity's Robotics Software Engineering Nanodegree

## Installation

- Follow instruction on [rtabmap_ros](https://github.com/introlab/rtabmap_ros) to install rtabmap_ros package.

- Then download the codes in this repository.

## Usage

Run the following commands in different consoles in series
- start the world file

      roslaunch my_robot world.launch

- start mapping
      
      roslaunch my_rtab_mapping mapping.launch

- teletop

      roslaunch my_rtab_mapping teleop.launch

Then you can teleop the robot and map the world, as shown in the gif below

![mapping_demo](https://github.com/CenturyLiu/RoboND-Project4/blob/master/mapping_demo.gif)


## Bug sorting

I encountered the problem of "Large map size" as stated in [/rtabmap_ros/issues/354](https://github.com/introlab/rtabmap_ros/issues/354). The problem is solved after I confines the "Grid/RangeMax" parameter in [my_rtab_mapping/launch/mapping.launch](https://github.com/CenturyLiu/RoboND-Project4/blob/master/my_rtab_mapping/launch/mapping.launch) (line 40-41), as suggested in the [/rtabmap_ros/issues/354](https://github.com/introlab/rtabmap_ros/issues/354). 


      <!-- Limit laser range-->
      <param name="Grid/RangeMax" type="string" value="30"/>
      
## Parameter tuning and performance

To create the most accurate 2D and 3D map of the environment I created, several sets of parameters are tested. The results are shown below.

|Parameters|Description|Original set|Scan-involved set|Tuned set|
|---|---|---|---|---|
|Rtabmap/DetectionRate|Rate (Hz) at which new nodes are added to map|1|1|**5**|
|Kp/MaxFeatures|Maximum visual words per image (bag-of-words)|400|400|**500**|
|Reg/Strategy|Use visual only for loop closure detection(0), use scan and visual for loop closure detection(1)|0|**1**|0|

Original set result:
Surprisingly, the original set produces the best 2D and 3D maps I can get. Although the average global loop closure number is < 3, which is 
required by the [Udacity project requirement](https://review.udacity.com/#!/rubrics/2352/view)

![original 3D](https://github.com/CenturyLiu/RoboND-Project4/blob/master/3D_map_with_original_param.gif)
> 3D map created by using the original set of parameters. There are few overlapping parts in the 3D map.

Scan-involved set:
The scan involved set produces lots of local loop closure, but lacks no global loop closure, and doesn't help to produce better map.

![Scan-involved set](https://github.com/CenturyLiu/RoboND-Project4/blob/master/icp_constraint_demo.gif)
> The yellow lines in the image are indication of local loop closure detection. The map will change a little bit each time a local loop closure is detected. 

Tuned set:
The tuned set increased the update rate and the number of features the rtabmap package will detect on each image. This helps increasing the  number of global loop closure detection, and should be generally good for mapping.

![tuned_3D](https://github.com/CenturyLiu/RoboND-Project4/blob/master/3D_map_with_tuned_param.gif)
> 3D map created by using the tuned set of parameters. Most of the part are created with acceptable quality, but there exists some overlapping (e.g. the coffee table in the middle)


