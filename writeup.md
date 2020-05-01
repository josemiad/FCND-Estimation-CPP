## Project: Building an Estimator
![Quad Image](./misc/enroute.png)

---



## [Rubric](https://review.udacity.com/#!/rubrics/1807/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  





#### Step 1: Sensor Noise

In this section we only need to change the values for MeasuredStdDev_GPSPosXY and MeasuredStdDev_AccelXY.

To do this, We run scenario 6 and we see the values between the GPS sensor are. After that, we stimate the values of the two variables to capture approx 68% of the respective measurements. 

I use the values 0.725 for MeasuredStdDev_GPSPosXY and 0.5 for MeasuredStdDev_AccelXY. After change that we obtain the next results.

![Quad Image](./images/step1.png)


We don't want that the home position was the started position of the dron, the drone needs to be able to star planning from anywhere.

Fist, we read the firt line of the csv file to set the global drone home position. After read that, I set it to home position with self.set_home_possition()

You can find it in [line 124](motion_planning.py#L124) of `motion_planning.py` and [line 161 to 168](planning_utils.py#L161-L168) of `planning_utils.py`.

#### 2. Set your current local position

the drone need to be able to takeoff from anywhere but the starter code assumed the drone takes off from map center.

With the drone's current position in geodetic coordinates from self.global_position, and the global home position set from last step from self.global_home, We use the global_to_local() function to convert the current global position to local position.

I did this in [line 130](motion_planning.py#L130) of `motion_planning.py`.


#### 3. Set grid start position from local position

The starter code hardcoded the map center as the start point for planning. To further enhance the flexibility to the start location, I changed this to be the current local position in line 142 to 144 of motion_planning.py.

I change the code to take the current local position of the drone as the start point. I did it in [line 141 to 143](motion_planning.py#L141-143) of `motion_planning.py`.

#### 4. Set grid goal position from geodetic coords

In this step, like before, I take a latitude and longitude that the user chose, and then I use the global_to_local function to get the local position in the grid.

I did it in [line 145 to 153](motion_planning.py#L145-153) of `motion_planning.py`.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)

To modify this I needed change tow things.

* First, the cost inside the class Action, to add the cost of the diagonal movements. You can find this in [line 58 to 61](planning_utils.py#L58-L61) of `planning_utils.py`.

* Second, in the valid_actions function, I added 4 more if conditions to check the new states. That's in [line 91 to 98](planning_utils.py#L91-L98) of `planning_utils.py`.

Here's you can see in the first image the old function with rectangular movements and the seconds image with the new function with diagonal movements 
![Top Down View](./images/rect_path.png)

![Top Down View](./images/diag_path.png)

#### 6. Cull waypoints 

Finally, we want to eliminate the points in the same line to reduce the number of waypoint. To do in we use the collinearity os three consecutive point and eliminate it if is unnecessary. 

This function is implemented in [line 171 to 192] (planning_utils.py#L171-L192) of `planning_utils.py`.

We call this function after calculate the path in [line 162](motion_planning.py#L162) of `motion_planning.py`.

Here you can see the path after prune it!!

![Top Down View](./images/prune_path.png)


### Execute the flight

Here is a photo while the drone execute the flight!!

![Top Down View](./images/flight.png)

