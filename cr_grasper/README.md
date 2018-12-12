### __README: Robot Graspit! Project__  
__Carlyn Dougherty (ccd2134@columbia.edu)__


Main Idea: replace [Graspit!](https://graspit-simulator.github.io/) with a similar simulator using Pybullet (https://pybullet.org/wordpress/) to 


### Method #1: List Planner: 
The goal here is to get the most simple form of a grasp planner working first, then move on to more sophisticated methods

####Setup/Useful foundations:
* __scene/things__
    - "fixed base" for a loaded object means it cant be moved
        - not great for dynamics - idea: find grasps *then* evaluate
    - configureDebugVisualizer changes GUI preferences
    - resetDebugVisualizerCamera changes camera position (your view of GUI)
    - globalScaling in any object changes its size 
* __load a hand__ 
    - Barrett URDF currently working - just make sure path is ok
* __grip an object/close gripper__ 
   - All joints in any hand have a min/max degree of bend (defined in urdf)
        - access this information using function "getJointInfo"
   - funciton "relax" makes all of those go to their most open
   - from there, currently just randomly applies force within a  certain constraint to close the joints (move towards most closed positions)
* __get contact points__ 
    - "getContactPoints"
    - can feed it two objects + works as step planner does
* __get random grasp__ 
    - quaternions suck. TODO: figure this out
* __evaluate grasp__ 
    - put grasps found in earlier stages into gravity environment + w/objects w/o fixed bases
    - shake them to see if the object shakes out
    - TODO: make this neater/completed
  


####Current Work/To Do:
* __load object__
* __get grasps in a circle of angles around the object (from 0 to 360 in a circle)__
* __spit back finger joints, position, orientation__ 
* __write a planner to check when grip is impossible__
* __rotate the hand around the base axis to get new grasps__ 
* __hand distance - detect the object collision between the hand and then then move a slight bit away to find optimal distance to object__ 
* __remember to change everything back to scaling of 1 before you give back numbers that make no sense__ 
    * __Initial__: loop around a 2D circle, facing the object
    * __Next__: sphere not circle
* __move towared a more flexible setup:__
    * __load in new objects:__ work on getting them into the scene programmatically 
    * __figure out another hand:__ get which joints to move, etc
* __then work on simulated annealing__ 


##Notes: 

##### Notes on URDFs: (based on needs for  barrett hand)

__If you have a depreciated .xacro:__ update/clean up using this script in all folders with .xacro files (dependancies and all) - specifically the robots + urdf files for barrett_hand
```sh
$ find . -iname "*.xacro" | xargs sed -i 's#<\([/]\?\)\(if\|unless\|include\|arg\|property\|macro\|insert_block\)#<\1xacro:\2#g'
```
__In the robots folder (because all the ones in the urdf folder depend on this one):__ convert the urdf.xacro to urdf 
```sh
$ source ~/catkin_ws/devel/setup.bash
$ rosrun xacro xacro --inorder name.urdf.xacro > name.urdf
```
