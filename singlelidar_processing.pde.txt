import processing.serial.*;
import java.awt.event.KeyEvent; // imports library for reading the data from the serial port
import java.io.IOException;
Serial myPort;
float x,y,z;
float distance,thetha, phi;
String data="";
String dist="";
String angle1="";
String angle2="";
int index1=0;
int index2=0;
void setup() {
 size(800,600,P3D);
 background(255);
 printArray(Serial.list());
 smooth();
 myPort = new Serial(this,"/dev/ttyACM0", 9600); // starts the serial communication
 myPort.bufferUntil('\n'); //reads the data from the serial port up to the character '.'. 
}

void serialEvent  (Serial myPort) {
  data = myPort.readStringUntil(';');
  data = data.substring(0,data.length()-1);
  
  index1 = data.indexOf(" ");
  index2 = data.indexOf(",");
  dist = data.substring(0, index1);
  angle1 = data.substring(index1 + 1, index2);
  angle2 = data.substring(index2 + 1, data.length());
  distance = float(dist);
  thetha = int(angle2);
  phi = int(angle1);
  
} 

void draw() {
//background(255);
stroke(0);
translate(230,230);
x = (-1) * distance * cos(thetha) * cos(phi) * 10;
y = (-1) * distance * sin(thetha) * 10;
z = (-1) * distance * cos(thetha) * sin(phi) * 10;
point(x, y, z);
print("Data ="+data+"\t");
print("x ="+x +"\t");
print("y ="+y +"\t");
print("z="+z +"\n");
}
