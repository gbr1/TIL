# How add a spherical joint (ball joint) in ROS

It is possible to define links and joints in URDF file.
ROS, in my case Noetic, works on *fixed* and *continuous* joints.

It is possible to define `<axis>` tag to set the rotational axis.
In the past *floating* type was supported and it allows to generate spherical joints or planar joints (multi axis movements).

I found a solution to use a *continuous joint* as spherical.

Let us consider a simple robot with 2 links, we can apply a fictitious continuous joint. If we don't put this joint we'll obtain an error on parsing the URDF due more links are considered as root.

The following urdf is an example of a simple robot with just 2 links and a joint.

```xml
<?xml version="1.0"?>
<robot name="nalug_robot">
    <link name="base_link">
        <visual>
            <origin rpy="0 0 0" xyz="0 0 0.15" />
            <geometry>
                <cylinder  length="0.3" radius="0.1" />
            </geometry>
            <material name="blue">
                <color rgba="0 0 1 1" />
            </material>
        </visual>
    </link>
    <link name="link1">
        <visual>
            <origin rpy="0 0 0" xyz="0 0 0.25" />
            <geometry>
                <cylinder  length="0.5" radius="0.05" />
            </geometry>
            <material name="yellow">
                <color rgba="1 1 0 1" />
            </material>
        </visual>
    </link>
    <joint name="joint1" type="continuous">
        <parent link="base_link"/>
        <child link="link1" />
    </joint>
</robot>
```

When this file is loaded all links are located in 0,0,0 and visual seems corrupted. No warning or Error will be shown in the terminal.

To obtain the perfect functioning we must add a TF broadcaster.

Here an example in python.

```python
#!/usr/bin/env python3

import rospy
import tf2_ros
import geometry_msgs.msg
import math
from tf.transformations import quaternion_from_euler

if __name__ == '__main__':
    rospy.init_node('dynamic_tf2_broadcaster')
    br = tf2_ros.TransformBroadcaster()
    t = geometry_msgs.msg.TransformStamped()

    t.header.frame_id = "base_link"
    t.child_frame_id = "link1"

    rate = rospy.Rate(10.0)
    while not rospy.is_shutdown():
        x = rospy.Time.now().to_sec() * math.pi

        t.header.stamp = rospy.Time.now()
        t.transform.translation.x = 0
        t.transform.translation.y = 0
        t.transform.translation.z = 0.3

        q = quaternion_from_euler(1.57, math.sin(x), math.cos(x))

        t.transform.rotation.x = q[0]
        t.transform.rotation.y = q[1]
        t.transform.rotation.z = q[2]
        t.transform.rotation.w = q[3]

        br.sendTransform(t)
        rate.sleep()
```

So you will obtain an oscillatition on pitch and yaw and the visual is loaded correctly.

Here what you can see:

![](/Users/giovanni/Documents/git/TIL/_assets/spherical-joint.gif)
