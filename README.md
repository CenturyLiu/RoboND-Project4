# RoboND-Project4
Implementation of the mapping project of the Udacity's Robotics Software Engineering Nanodegree

## Installation

- Follow instruction on [rtabmap_ros](https://github.com/introlab/rtabmap_ros) to install rtabmap_ros package.

- Then download the codes in this repository.

## Parameter tuning notes


I encountered the problem of "Large map size" as stated in [/rtabmap_ros/issues/354](https://github.com/introlab/rtabmap_ros/issues/354). The problem is solved after I confines the "Grid/RangeMax" parameter in [my_rtab_mapping/launch/mapping.launch](https://github.com/CenturyLiu/RoboND-Project4/blob/master/my_rtab_mapping/launch/mapping.launch) (line 40-41), as suggested in the [/rtabmap_ros/issues/354](https://github.com/introlab/rtabmap_ros/issues/354). 


      <!-- Limit laser range-->
      <param name="Grid/RangeMax" type="string" value="30"/>
