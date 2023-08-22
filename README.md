# Make-Your-Own-MPU6050-Gyro-Sensor

![Screenshot (1769)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/05dda4db-856d-4cd1-bf2b-d2fe7db407da)

The MPU6050 is a very popular accelerometer gyroscope chip that has six axes sense with a 16-bit measurement resolution. This high accuracy in sense and the cheap cost make it very popular among the DIY community. Even many commercial products are equipped with the MPU6050. The combination of gyroscope and accelerometers is commonly referred to as an Inertial Measurement Unit or IMU.

IMU sensors are used in a wide variety of applications such as mobile phones, tablets, satellites, spacecraft, drones, UAVs, robotics, and many more. They are used for motion tracking, orientation and position detection, flight control, etc.

The MPU6050 module consists of an MPU6050 IMU chip from TDK InvenSense. It comes in a 24-pin QFN package with a dimension of 4mm x 4mm x 0.9mm. The module has a very low component count, including an AP2112K 3.3V regulator, I2C pull-up resistors, and bypass capacitors. There is also a power led which indicates the power status of the module.

The module also has two auxiliary pins which can be used to interface external IIC modules like a magnetometer, however, it is optional. Since the IIC address of the module is configurable more than one MPU6050 sensor can be interfaced to a Microcontroller using the AD0 pin. This module also has well documented and revised libraries available hence it’s very easy to use with famous platforms like Arduino. So, if you are looking for a sensor to control motion for your RC Car, Drone, Self-balancing Robot, Humanoid, Biped or something like that, then this sensor might be the right choice for you.

![Screenshot (1767)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/c374e195-f2e1-4084-8b3e-6d97f1822775)

How Does MEMS Accelerometer Work?
MEMS accelerometers are used wherever there is a need to measure linear motion, either movement, shock, or vibration but without a fixed reference. They measure the linear acceleration of whatever they are attached to. All accelerometers work on the principle of a mass on a spring, when the thing they are attached to accelerates, then the mass wants to remain stationary due to its inertia and therefore the spring is stretched or compressed, creating a force which is detected and corresponds to the applied acceleration.

In MEMS accelerometer, precise linear acceleration detection in two orthogonal axes is achieved by a pair of silicon MEMS detectors formed by spring ‘proof’ masses. Each mass provides the moving plate of a variable capacitance formed by an array of interlaced finger loke structures. When the sensor is subjected to a linear acceleration along its sensitive axis, the proof mass tends to resist motion due to its inertia, therefore the mass and its fingers become displaced concerning the fixed electrode fingers. The gas between the fingers provides a damping effect. This displacement induces a differential capacitance between the moving and fixed silicon fingers which is proportional to the applied acceleration. This change in capacitance is measured with a high-resolution ADC and then the acceleration is calculated from the rate of change in capacitance. In MPU6050 this is then converted into readable value and then it’s transferred to the I2C master device.

![Screenshot (1772)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/e1fd55d6-dbb6-4dc1-b02c-6a578e6945ff)
![Screenshot (1770)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/9d47e536-a080-471b-8f0b-d38d2e13f8fa)


How Does MEMS Gyroscope Work?
The MEMS Gyroscope working is based on the Coriolis Effect. The Coriolis Effect states that when a mass moves in a particular direction with velocity and an external angular motion is applied to it, a force is generated and that causes a perpendicular displacement of the mass. The force that is generated is called the Coriolis Force and this phenomenon is known as the Coriolis Effect. The rate of displacement will be directly related to the angular motion applied.

The MEMS Gyroscope contains a set of four proof mass and is kept in a continuous oscillating movement. When an angular motion is applied, the Coriolis Effect causes a change in capacitance between the masses depending on the axis of the angular movement. This change in capacitance is sensed and then converted into a reading. Here is a small animation showing the movement of these proof masses on the application of an angular movement for different axis.

#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>
#include <Wire.h>
Adafruit_MPU6050 mpu;
void setup(void) {
  Serial.begin(115200);
  // Try to initialize!
  if (!mpu.begin()) {
    Serial.println("Failed to find MPU6050 chip");
    while (1) {
      delay(10);
    }
  }
  Serial.println("MPU6050 Found!");
  // set accelerometer range to +-8G
  mpu.setAccelerometerRange(MPU6050_RANGE_8_G);
  // set gyro range to +- 500 deg/s
  mpu.setGyroRange(MPU6050_RANGE_500_DEG);
  // set filter bandwidth to 21 Hz
  mpu.setFilterBandwidth(MPU6050_BAND_21_HZ);
  delay(100);
}
void loop() {
  /* Get new sensor events with the readings */
  sensors_event_t a, g, temp;
  mpu.getEvent(&a, &g, &temp);
  /* Print out the readings */
  Serial.print("Acceleration X: ");
  Serial.print(a.acceleration.x);
  Serial.print(", Y: ");
  Serial.print(a.acceleration.y);
  Serial.print(", Z: ");
  Serial.print(a.acceleration.z);
  Serial.println(" m/s^2");
  Serial.print("Rotation X: ");
  Serial.print(g.gyro.x);
  Serial.print(", Y: ");
  Serial.print(g.gyro.y);
  Serial.print(", Z: ");
  Serial.print(g.gyro.z);
  Serial.println(" rad/s");
  Serial.print("Temperature: ");
  Serial.print(temp.temperature);
  Serial.println(" degC");
  Serial.println("");
  delay(1000);
}

![Screenshot (1765)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/fe5ed130-2be3-45c6-b5db-d77038e113e9)

Code Explanation
In the starting, we have included the Adafruit MPU6050 library, Adafruit Sensor library, and the wire library, which are necessary to communicate with the MPU6050 and get the reading. Then we have created a new instance called mpu which will be used to get the readings from the MPU6050 IMU.

#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>
#include <Wire.h>
Adafruit_MPU6050 mpu;
In the setup function, we have initialized the serial communication and the MPU6050 IMU. Then the accelerometer range, gyroscope range, and filter bandwidth parameters are set. The range parameters will affect the accuracy of the reading. So, if needed these can be changed as per the library values. The setFilterBandwidth parameter will change the low pass filter bandwidth.

void setup(void) {
  Serial.begin(115200);
  // Initializethe MPU6050 IMU
  if (!mpu.begin()) {
    Serial.println("Failed to find MPU6050 chip");
    while (1) {
      delay(10);
    }
  }
  
  Serial.println("MPU6050 Found!");
  // set accelerometer range to +-8G
  mpu.setAccelerometerRange(MPU6050_RANGE_8_G);
  // set gyro range to +- 500 deg/s
  mpu.setGyroRange(MPU6050_RANGE_500_DEG);
  // set filter bandwidth to 21 Hz
  mpu.setFilterBandwidth(MPU6050_BAND_21_HZ);
  delay(100);
}
In the loop function, the values are read from the MPU6050 with the help of Adafruit library and then printed to the serial monitor. This will repeat every second.

void loop() {
  /* Get new sensor events with the readings */
  sensors_event_t a, g, temp;
  mpu.getEvent(&a, &g, &temp);
  /* Print out the readings */
  Serial.print("Acceleration X: ");
  Serial.print(a.acceleration.x);
  Serial.print(", Y: ");
  Serial.print(a.acceleration.y);
  Serial.print(", Z: ");
  Serial.print(a.acceleration.z);
  Serial.println(" m/s^2");
  Serial.print("Rotation X: ");
  Serial.print(g.gyro.x);
  Serial.print(", Y: ");
  Serial.print(g.gyro.y);
  Serial.print(", Z: ");
  Serial.print(g.gyro.z);
  Serial.println(" rad/s");
  Serial.print("Temperature: ");
  Serial.print(temp.temperature);
  Serial.println(" degC");
  Serial.println("");
  delay(1000);
}

![Screenshot (1766)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/978baca9-916f-4fc5-85c9-e2db1637ff0e)


This project contains PCB and which was made with the help of JLCPCB

![FWII4IJLL9GI260](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/d242b4b8-e2ed-48db-8289-a44e3fc6cc8a)

JLCPCB has upgraded the via-in-pad process of all 6-20 layer PCBs for free and provides free ENIG to make PCB products more stable and reliable. It is worth mentioning that due to large-scale production capabilities, JLCPCB is able to reduce the cost, allowing everyone to truly enjoy the benefits of the JLCPCB advanced fabrication. Here at JLCPCB, you can also get 1-8 layer PCBs at only $2

![FEP2OKBLL9GI263](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/1f36e3bc-1bf0-49fd-9105-4cd8299106b6)

Go to JLCPCB website and create a free account.  

Register and Login using Google Account is also possible.

![FWBKDVKLL9GI262](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/af51e862-4775-47df-a5e3-d0012b71cfea)

Step 2 – Upload Gerber File
Once you have successfully created an account, Click on “Quote Now” and upload your Gerber File.

Gerber File contains information about your PCB such as PCB layout information, Layer information, spacing information, tracks to name a few.

![FTC3JQDLL9GI261](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/f0de552f-22a1-47b1-a844-01ee0f6fa77b)


Step 3 – Preview the File
Once the Gerber file is uploaded, it will show you a preview of your circuit board.

Make sure this is the PCB Layout of the board you want.

Step 4 – Choose Necessary PCB Options

Below the PCB preview, you will see so many options such as PCB Quantity, Texture, Thickness, Color etc. Choose all that are necessary for you

The MPU-6050 is the world’s first and only 6-axis motion tracking devices designed for the low power, low cost, and high performance requirements of smartphones, tablets and wearable sensors.

Experimental Procedures

Step 1: Connect the circuit

Step 2:Add library

Open Arduino IDE, then click Sketch -> Include Library -> Add ZIP Library, and select MPU6050.zip to include.

After including successfully, you can see the example in File -> Examples -> MPU6050 as shown.

Step 3: Get Drift Compensation Values

The MPU6050 sensor’s reading (raw data) is transformed in space attitude angle. To get more stable and accurate reading, we should set the drift compensation first. The drift differs for different sensors (also affected by environments), thus it’s necessary to configure it before using every time.

![Screenshot (1770)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/11bcc5a8-25a5-4c3f-badc-714db39fc3aa)

Download mpu6050_calibration in folder examples, select the board type and COM. Open the serial monitor after program uploading, set the baud rate to 115200, and place the MPU6050 horizontally, do not move to avoid any vibration disturbance. Then press any key and click enter. So here you can see the compensation values are 1721, -1885, 1368, 38, -25, and 18, corresponding to six parameters including acceleration (AcceX, AcceY, and AcceZ) and gyroscope (GyroX, GyroY, and GyroZ).

Step 4: Reading Real-time Data

Then upload the program, press any key and click Enter in the serial monitor. Move the MPU6050 and observe the real-time data (as shown in figure below).

![Screenshot (1773)](https://github.com/No-Need-Loi/Make-Your-Own-MPU6050-Gyro-Sensor/assets/142481076/283b90eb-31c7-43f6-bce5-bcb0a72aaea4)


what is mpu6050 :

The MPU-6050 is the world’s first and only 6-axis motion tracking devices designed for the low power, low cost, and high performance requirements of smartphones, tablets and wearable sensors.

