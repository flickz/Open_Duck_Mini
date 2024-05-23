# Preparing a robot for simulation in MuJoCo

# If you designed your robot in OnShape

## Make sure to design you robot according to onshape-to-robot constraints
https://onshape-to-robot.readthedocs.io/en/latest/design.html

## Get get robot urdf from onshape


```bash
$ onshape-to-robot robots/bd1/
```

# Convert URDF to MJCF (MuJoCo)

## Get MuJoCo binaries

Download mujoco binaries somewhere https://github.com/google-deepmind/mujoco/releases

unpack and run

```bash
$ ./compile robot.urdf robot.xml
```

## In robot.xml, add actuators : 
Example : 
```xml
	<actuator>
		<position name="left_hip_yaw"    joint="left_hip_yaw"    inheritrange="1"/>
		<position name="left_hip_roll"   joint="left_hip_roll"   inheritrange="1"/>
		<position name="left_hip_pitch"  joint="left_hip_pitch"  inheritrange="1"/>
		<position name="left_knee"       joint="left_knee"       inheritrange="1"/>
		<position name="left_ankle"      joint="left_ankle"      inheritrange="1"/>
		<position name="right_hip_roll"  joint="right_hip_roll"  inheritrange="1"/>
		<position name="right_hip_yaw"   joint="right_hip_yaw"   inheritrange="1"/>
		<position name="right_hip_pitch" joint="right_hip_pitch" inheritrange="1"/>
		<position name="right_knee"      joint="right_knee"      inheritrange="1"/>
		<position name="right_ankle"     joint="right_ankle"     inheritrange="1"/>
		<position name="head_pitch1"     joint="head_pitch1"     inheritrange="1"/>
		<position name="head_pitch2"     joint="head_pitch2"     inheritrange="1"/>
		<position name="head_yaw"        joint="head_yaw"        inheritrange="1"/>
	</actuator>
```

## Add a freejoint 

encapsulate the body in a freejoint
```xml
<worldbody>
	<body>
		<freejoint />
		...
		...
	</body>
</worldbody>
```

## Setup collision groups, damping and friction
/!\ remove actuatorfrcrange in joints
Put that inside the <mujoco> bracket
```xml
<mujoco>
  <default>
    <geom contype="1" conaffinity="1" solref=".004 1" />
    <joint damping="0.09" frictionloss="0.1"/>
    <position kp="10" forcerange="-5.0 5.0"/>
  </default>
	...
	...
</mujoco>
```

still need to add : 
- change frames to sites