---
layout: page
title: RoboMaster
---

## Introduction of [RoboMaster](https://www.robomaster.com/en-US)
The RoboMaster University Championship focuses on the comprehensive application and engineering practice abilities of the participants in science and engineering disciplines, fully integrating many robot-related technical disciplines such as “machine vision”, “embedded system design”, “mechanical control”, “inertial navigation”, “human-computer interaction”. At the same time, the innovative combination of e-sports presentation and robotic competition makes the robot confrontation more intuitive and intense, attracting the attention of many technology enthusiasts and the public.

<img src="https://zuozuojia.github.io/zuojia/images/rules.png">

---


## My Team Position
- 2022 ROBOMASTER : <font color='#00ff00'> <b>Leader</b> </font>, <b>Embedded Engineer</b>
- 2021 ROBOMASTER : <b>Vice Leader</b>, <b>Embedded Engineer</b>, <b>Algorithm Engineer</b>

---

## Leadership Experience

Personal Assignment During the Competition:
- Manage the research and development cycle of robots
- Determine the technical indicators and design scheme of robots
- Manage and assign tasks to team members
- Lead team members to carry out promotional activities and get sponsorship
- Hold intramural competitions to select freshmen members

Some pictures of me in the contest:
<table>
    <tr>
        <td ><center><img src="https://zuozuojia.github.io/zuojia/images/rm备赛1.png" width="200" ></center></td>
        <td ><center><img src="https://zuozuojia.github.io/zuojia/images/rm备赛3.png" width="200" ></center></td>
        <td ><center><img src="https://zuozuojia.github.io/zuojia/images/rm备赛2.png" width="200" ></center></td>
        <td ><center><img src="https://zuozuojia.github.io/zuojia/images/rm备赛4.jpeg" width="200" ></center></td>
    </tr>
</table>

<!-- <style>
.img-wrap{
border: 1px 
}
img{
float: left;
width: 30%;
}
</style> -->

Some pictures of the robots in our team:
<table>
    <tr>
        <td ><center><img src="https://zuozuojia.github.io/zuojia/images/步兵.png"  >Figure 1.Standard Robot</center></td>
        <td ><center><img src="https://zuozuojia.github.io/zuojia/images/英雄.png"  >Figure 2.Hero Robot</center></td>
        <td ><center><img src="https://zuozuojia.github.io/zuojia/images/哨兵.png"  >Figure 3.Sentry Robot</center></td>
    </tr>
</table>

A group photo of my team and our robots:
<img src="https://zuozuojia.github.io/zuojia/images/rm合照.png">

Video of our competition:
<!-- [![](https://zuozuojia.github.io/zuojia/images/hero_victory.png)](https://player.bilibili.com/player.html?aid=642858681&bvid=BV1TY4y1n74w&cid=756999895&page=1) -->
<!-- <video src="https://www.bilibili.com/video/BV1TY4y1n74w?spm_id_from=333.999.0.0"></video> -->
<!-- <iframe height=498 width=510 src="https://www.bilibili.com/video/BV1TY4y1n74w?spm_id_from=333.999.0.0"> -->

<iframe src="https://www.youtube.com/watch?v=kqdkdlgtjy8" frameborder="0" allowfullscreen></iframe>

<!-- <div class='img-wrap'>
<img src="https://zuozuojia.github.io/zuojia/images/步兵.png" width="200">
<img src="https://zuozuojia.github.io/zuojia/images/英雄.png" width="200">
<img src="https://zuozuojia.github.io/zuojia/images/哨兵.png" width="200">
</div> -->

---

## Embedded Project

### 1. Software Architecture of the Robots in RoboMaster (Based on RT-Thread)

The code framework based on RT-Thread (A kind of RTOS) is designed for all robots in Robomaster, including peripheral drivers based on RT-Thread; communication protocol; general code library; robot master control, PTZ control, automatic aiming, launch logic and other upper layers module.

Software Architecture Diagram:
<img src="https://zuozuojia.github.io/zuojia/images/RM软件架构.png">

Logic block diagram of PTZ control and launch mechanism:
<img src="https://zuozuojia.github.io/zuojia/images/云台逻辑框图.png">

Main features:
- Compatible with gimbal and chassis projects of all robots.
- Decouple the code as much as possible to realize the reuse of the underlying files.
- Use `CMake` & `GCC` to achieve cross-platform development, `Cmakelist` files can be automatically generated according to the scons script.
- Compatible with `IAR` and `KEIL` compilation and debugging.
- No dependencies on specific operating systems and IDEs.


### 2. Contributions to RT-Thread (A kind of RTOS)

Fixed some bugs about STM32 MCU peripheral drivers in RT-Thread.

GitHub link:

- [[bsp/stm32] Fix the bug of can filter conflict.](https://github.com/RT-Thread/rt-thread/pull/5488)

- [[bsp/stm32] Fix some can and pwm bugs in stm32.](https://github.com/RT-Thread/rt-thread/pull/5477)

### 3. Software of Motor Intelligent Control Board (Based on RT-Thread)

Developed a control relay station that automatically controls the motor. The host MCU only needs to call the corresponding API to send a specific command to the slave MCU to make the slave automatically control different types of motors attached to the slave. It supports different data sources of the motors, different reduction ratios, and different control modes ( Closed-loop speed, closed-loop angle, etc.), automatic alignment, automatic error handling and disconnection detection, etc.. The slave machine mounts the device through the linked list, which shares the calculation amount of the host computer, expands the number of motors that the host computer can control which makes development more flexible and convenient.

```c
// MCU 105 Slave and motor initialization
void M105_Init(rt_uint8_t MCUnum);
/* ----------------------- control commands ----------------------- */
// Set the absolute set value of the speed
void SETM105_SPEED_ABS(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float SetSpeed);
// Set the speed setpoint increment
void SETM105_SPEED_ADD(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float SetAddSpeed);
// Set the absolute set value of the angle
void SETM105_ANGLE_ABS(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float SetAngle);
// Set the angle setpoint increment
void SETM105_ANGLE_ADD(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float SetAddAngle);
// Set output offset and gain
void SETM105_BIAS_GAIN(rt_uint8_t MCUNum, rt_uint8_t MotorNum, rt_int16_t SetBias, rt_int16_t SetGain);

/* ----------------------- read commands ----------------------- */
// Read the angle command of the current motor data source encoder
void CMD_ReadEncodeAngle(rt_uint8_t MCUNum, rt_uint8_t MotorNum);
// Read current motor absolute angle command
void CMD_ReadAbsAngle(rt_uint8_t MCUNum, rt_uint8_t MotorNum);
// Read current speed command
void CMD_ReadSpeed(rt_uint8_t MCUNum, rt_uint8_t MotorNum);
// Read current current value command
void CMD_ReadCurrent(rt_uint8_t MCUNum, rt_uint8_t MotorNum);
// Read current angle closed-loop integral value command
void CMD_ReadAngleI(rt_uint8_t MCUNum, rt_uint8_t MotorNum);

/* ----------------------- read functions ----------------------- */
// Read the current motor data source encoder angle
void ReadM105_EncoderAngle(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float *angle);
// Read the current absolute motor angle
void ReadM105_AbsAngle(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float *angle);
// Read the current speed
void ReadM105_Speed(rt_uint8_t MCUNum, rt_uint8_t MotorNum, rt_int16_t *speed);
// Read the current current value
void ReadM105_Current(rt_uint8_t MCUNum, rt_uint8_t MotorNum, rt_int16_t *current);
// Read the current angle closed-loop integral value
void ReadM105_AngleI(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float *i_value);

/* ----------------------- configuration command ----------------------- */
// Configure Motor Control Mode
// parameter:
// SET105_NO_CONTROL - do not control
// SET105_SPEED_CONTROL - speed PID control
// SET105_ANGLE_CONTROL - speed-angle PID control
void SetM105_ControlMode(rt_uint8_t MCUNum, rt_uint8_t MotorNum, ControlMode_e ControlMode);
// Configure motor speed closed-loop PID parameters
void SetM105_SpeedPID_K(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float kp, float ki, float kd, rt_uint8_t SpePeriod);
// Configure motor speed closed-loop PID limit parameters
void SetM105_SpeedPID_L(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float i_limit, float out_limit_up, float out_limit_down);
// Configure motor angle closed-loop PID parameters
void SetM105_AnglePID_K(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float kp, float ki, float kd, rt_uint8_t AngPeriod);
// Configure motor angle closed-loop PID limit parameters
void SetM105_AnglePID_L(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float i_limit, float out_limit_up, float out_limit_down);
// Configuring Coefficients for Feedforward Control
void SetM105_FrontRatio(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float FrontRatio);
// Configure parameters for zero calibration
// Mode - Calibration mode: 0-current angle is zero, 1-calibration through forward stall, 2-calibration through reverse stall
// Speed - Speed setting value during stall calibration (0~255)
// MaxCurrent - Maximum current threshold for identifying stalled rotor (0~65535)
// time - After the locked rotor time, it is confirmed as locked rotor, the unit is ms (0~255)
void SetM105_ZeroAngle(rt_uint8_t MCUNum, rt_uint8_t MotorNum, rt_uint8_t Mode, 
                        rt_uint8_t Speed, rt_uint16_t MaxCurrent, rt_uint8_t time);
// Configure last position calibration
void SetM105_EndAngle(rt_uint8_t MCUNum, rt_uint8_t MotorNum, rt_uint8_t Mode, 
                        rt_uint8_t Speed, rt_uint16_t MaxCurrent, rt_uint8_t time);
// Configuring Stall Threshold Parameters
// LockCurrent - Maximum current threshold for identifying stalled rotor (0~65535)
// LockTime - After the locked rotor time, it is confirmed as locked rotor, the unit is ms (0~65535)
void SetM105_LockParam(rt_uint8_t MCUNum, rt_uint8_t MotorNum, rt_uint16_t LockCurrent, rt_uint16_t LockTime);
// Whether the zero calibration is completed
// Return value: MOTOR_OK - calibration completed, MOTOR_ERR - calibration not completed
MotorErr_e IfZeroAngleOK(rt_uint8_t MCUNum, rt_uint8_t MotorNum);
// Whether the last position calibration is completed, and obtain the last position angle
// Return value: MOTOR_OK - calibration completed, MOTOR_ERR - calibration not completed
// *EndAngle - Used to get the last angle
MotorErr_e IfEndAngleOK(rt_uint8_t MCUNum, rt_uint8_t MotorNum, float *EndAngle);
```
- control commands:
    - Set the absolute set value of the speed
    - Set the speed setpoint increment
    - Set the absolute set value of the angle
    - Set the angle setpoint increment
    - Set output offset and gain
- read commands:
    - Read the angle command of the current motor data source encoder
    - Read current motor absolute angle command
    - Read current speed command
    - Read current current value command
    - Read current angle closed-loop integral value command
- configuration command:
    - Configure Motor Control Mode:do not control / speed PID control / speed-angle PID control
    - Configure motor speed closed-loop PID parameters
    - Configure motor speed closed-loop PID limit parameters
    - Configure motor angle closed-loop PID parameters
    - Configure motor angle closed-loop PID limit parameters
    - Configuring Coefficients for Feedforward Control
    - Configure parameters for zero calibration
    - Configure last position calibration
    - Configuring Stall Threshold Parameters
    - Check whether the zero calibration is completed
    - Check whether the last position calibration is completed, and obtain the last position angle



---

## Visual Algorithm Project

### Adaptive Exposure Algorithm

On the RoboMaster arena, teams can install industrial cameras on the robots to automatically identify enemy robots. Whether it is the traditional opencv method or the convolutional neural network, it is necessary to ensure that the brightness of the robot armor plate (attacking) in the picture remains basic regardless of the lighting conditions. In order to keep the brightness of the target object constant during the shooting of the dataset and during the competition, I designed a camera adaptive exposure algorithm.

Based on the characteristics of these two scenes, I found that there are no very discrete sources of distracting light sources in the field of view (i.e. there are distractions that glow everywhere, such as several computer screens). However, there may be large blocks of external light sources such as windows, doors, etc. If only the grayscale data after image binarization is used as the data source of the adaptive algorithm, these large light sources will seriously affect the accuracy of the adaptive algorithm. So I designed an adaptive exposure algorithm that can automatically cut out large spots.

The effect diagram of removing large light spots:
<img src="https://zuozuojia.github.io/zuojia/images/自适应曝光.png" width="600">

Video of the effect of adaptive exposure algorithm that automatically cuts out large light spots:

<iframe src="https://www.youtube.com/watch?v=bb7qiyvm8rk" frameborder="0" allowfullscreen></iframe>
