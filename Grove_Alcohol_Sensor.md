# Grove - Alcohol Sensor
***
![](https://raw.githubusercontent.com/SeeedDocument/Grove_Alcohol_Sensor/master/image/Alcohol_sensor_01.jpg)
## Introduction
Grove - Alcohol Sensor is a complete alcohol sensor module for Arduino or Seeeduino. It is built with [MQ303A](https://github.com/SeeedDocument/Grove_Alcohol_Sensor/raw/master/resource/MQ303A.pdf) semiconductor alcohol sensor. It has good sensitivity and fast response to alcohol. It is suitable for making Breathalyzer. This Grove implements all the necessary circuitry for MQ303A like power conditioning and heater power supply. This sensor outputs a voltage inversely proportional to the alcohol concentration in air.

Model:[SEN21723P](https://www.seeedstudio.com/item_detail.html?p_id=764)

!!!note
Note that The sensor value only reflects the approximated trend of gas concentration in a permissible error range, it DOES NOT represent the exact gas concentration. The detection of certain components in the air usually requires a more precise and costly instrument, which cannot be done with a single gas sensor. If your project is aimed at obtaining the gas concentration at a very precise level, then we do not recommend this gas sensor.

[![](https://raw.githubusercontent.com/SeeedDocument/Grove_Dust_Sensor/master/image/150px-Get_One_Now_Banner.png)](https://www.seeedstudio.com/item_detail.html?p_id=764)
## Features
- Input Voltage: 5V
- Working Current: 120mA
- Detectable Concentration: 20-1000ppm
- Grove Compatible connector
- Highly sensitive to alcohol.
- Fast response and resumes quickly after alcohol exposure.
- Long life.
- Compact form factor.

## Usage
### Hardware Installation
Grove products have a eco system and all have a same connector which can plug onto the [Base Shield](http://www.seeedstudio.com/wiki/index.php?title=Base_shield_v2&uselang=en). Connect this module to the A0 port of Base Shield, however, you can also connect Gas sensor to Arduino without Base Shield by jumper wires.
Arduino UNO	|Alcohol Sensor
---|---
5V|	VCC
GND|	GND
Analog A1|	SCL
Analog A0|	DAT

You can gain the present voltage through the DAT pin of sensor. Sensitivity can be regulated by rotating the potentiometer. 

!!!note
Please note the best preheat time of the sensor is above 48 hours. 

For the detailed information about the Alcohol sensor please refer to the datasheet.

![](https://raw.githubusercontent.com/SeeedDocument/Grove_Alcohol_Sensor/master/image/Twig_Alcohol_Sensor_Connected_To_Seeeduino_via_BaseStem.jpg)

### Download Code and Upload
There're two steps you need to do before getting the concentration of gas.
First, connect the module with Grove Shield using A0 like the picture above. And put the sensor in a clear air and use the program below.

``` c
#define heaterSelPin 15
 
void setup() {
  Serial.begin(9600);
  pinMode(heaterSelPin,OUTPUT);   // set the heaterSelPin as digital output.
  digitalWrite(heaterSelPin,LOW); // Start to heat the sensor
}
 
void loop() {
  float sensor_volt; 
  float RS_air; //  Get the value of RS via in a clear air
  float sensorValue;
 
/*--- Get a average data by testing 100 times ---*/   
    for(int x = 0 ; x < 100 ; x++)
  {
    sensorValue = sensorValue + analogRead(A0);
  }
  sensorValue = sensorValue/100.0;
/*-----------------------------------------------*/
 
  sensor_volt = sensorValue/1024*5.0;
  RS_air = sensor_volt/(5.0-sensor_volt); // omit *R16
  Serial.print("sensor_volt = ");
  Serial.print(sensor_volt);
  Serial.println("V");
  Serial.print("RS_air = ");
  Serial.println(RS_air);
  delay(1000);
 
}
```

Then, open the monitor of Arduino IDE, you can see some data are printed, write down the value of RS_air and you need to use it in the following program. During this step, you may pay a while time to test the value of RS_air. 

```c
#define heaterSelPin 15
 
void setup() {
  Serial.begin(9600);
  pinMode(heaterSelPin,OUTPUT);   // set the heaterSelPin as digital output.
  digitalWrite(heaterSelPin,LOW); // Start to heat the sensor  
}
 
void loop() {
 
  float sensor_volt;
  float RS_gas; // Get value of RS in a GAS
  float ratio; // Get ratio RS_GAS/RS_air
  int sensorValue = analogRead(A0);
  sensor_volt=(float)sensorValue/1024*5.0;
  RS_gas = sensor_volt/5.0-sensor_volt; // omit *R16
 
  /*-Replace the name "R0" with the value of R0 in the demo of First Test -*/
  ratio = RS_gas/RS_air;  // ratio = RS/R0 
  /*-----------------------------------------------------------------------*/
 
  Serial.print("sensor_volt = ");
  Serial.println(sensor_volt);
  Serial.print("RS_ratio = ");
  Serial.println(RS_gas);
  Serial.print("Rs/R0 = ");
  Serial.println(ratio);
 
  Serial.print("\n\n");
 
  delay(1000);
 
}
```
Now, we can get the concentration of gas from the below figure 

![](https://raw.githubusercontent.com/SeeedDocument/Grove_Alcohol_Sensor/master/image/Gas_Sensor_5.png)

According to the figure, we can see that the minimum concentration we can test is 20ppm and the maximum is 10000ppm, in a other word, we can get a concentration of gas between 0.002% and 1%. However, we can't provide a formula because the relation between ratio and concentration is nonlinear. 

### Notification
- The value varies between 500 - 905. Hence any value above 650 indicates alcohol vapor in the vicinity.
- Once exposed to alcohol vapor, it takes some time for the sensor value to decrease completely.
- Yet, any new exposure will show instant increase in sensor value.

!!!Warning
### Cautions
- Alcohol sensor is very sensitive semiconductor device. Handle with care.
- Do not expose to organic silicon steam, alkali or corrosive gases.
- Do not use freeze or spill water.
- Maintain proper working voltage.

## Resources
- [Grove-Alcohol Sensor Eagle File](https://github.com/SeeedDocument/Grove_Alcohol_Sensor/raw/master/resource/Twig_-_Alcohol_Sensor_Eagle_Files.zip)
- [Grove-Alcohol Sensor v1.2 Eagle File](http://www.seeedstudio.com/wiki/File:Grove_-_Alcohol_Sensor_sch_pcbv1.2.zip)
- [Schematics in PDF Format](https://github.com/SeeedDocument/Grove_Alcohol_Sensor/raw/master/resource/Twig_Alcohol_Sensor_v0.9b_scehmatic.pdf)
- [How to Choose A Gas Sensor](http://www.seeedstudio.com/wiki/How_to_choose_A_Gas_Sensor)
- [MQ303A.pdf](https://github.com/SeeedDocument/Grove_Alcohol_Sensor/blob/master/resource/MQ303A.pdf)

## Help us to make it better
<iframe style="height: 600px; width: 500px; margin: 10px 0 10px;" allowTransparency="true" src="https://www.surveymonkey.com/r/5PK893F" frameborder="0"></iframe>
