# Arduino
#include <Enes100.h>

#include <dfr_tank.h>
//#include <SoftwareSerial.h>
//#include <Enes100.h>
//#include <Enums.h>

DFRTank tank;

Enes100 enes("Vulcan", FIRE, 3, 8, 9);
int signal;
int pastLocationy = 0;
int pastLocationx = 0;
int OSV_SPEED_FORWARD = 255;
int OSV_SPEED_REVERSE = -255;

void setup () {
    //SoftwareSerial mySerial(8, 9);
  tank.init();
  pinMode(5, OUTPUT);
  //Serial.begin(9600);
  enes.retrieveDestination();
  
  
}




void turnToAngle(double n) {
  n = n * (PI/180);
  if ( enes.location.theta < PI + n) {
     while (enes.location.theta != n) {
      tank.setLeftMotorPWM(OSV_SPEED_FORWARD);
      tank.setRightMotorPWM(OSV_SPEED_REVERSE);
      properUpdateLocation();
     } 
  }
   if ( enes.location.theta > PI + n) {
     while (enes.location.theta != n) {
      tank.setLeftMotorPWM(OSV_SPEED_REVERSE);
      tank.setRightMotorPWM(OSV_SPEED_FORWARD);
      properUpdateLocation();

     }
   }
}


void moveUpward(double n) {
  properUpdateLocation();
  while (
}

void properUpdateLocation() {
  while (!enes.updateLocation());
}

void loop(){

  properUpdateLocation();

  
  if (enes.location.y < enes.destination.y) {
    turnToAngle(90)

   }

  
  if (enes.location.theta != 0) {
   while (enes.location.theta !=  0) {
    tank.setLeftMotorPWM(-255);
    tank.setRightMotorPWM(255);
    enes.updateLocation();
   }
  }
    signal = analogRead(5); 
  if (enes.location.y == enes.destination.y && enes.location.theta == 0 && signal < 300) {
    while (signal < 300 ) {
     signal = analogRead(5); 
     tank.setLeftMotorPWM(255);
     tank.setRightMotorPWM(255);
     enes.updateLocation(); 
    }
  }
  
  if (signal >= 300) {
   enes.updateLocation();
   pastLocationy = enes.location.y;
   pastLocationx = enes.location.x;
    while (enes.location.theta !=  90) {
    tank.setLeftMotorPWM(-255);
    tank.setRightMotorPWM(255);
    enes.updateLocation();
   }
   while ( enes.location.y < pastLocationy + 0.5) {           //replace 0.5 with length of OSV  
    tank.setLeftMotorPWM(-255);
    tank.setRightMotorPWM(255);
    enes.updateLocation();
   }
    while (enes.location.theta != 0) {
    tank.setLeftMotorPWM(-255);
    tank.setRightMotorPWM(255);
    enes.updateLocation();
    }
    while (enes.location.x < pastLocationx + 1) {        // replace 1 with proper number
    tank.setLeftMotorPWM(255);
    tank.setRightMotorPWM(255);
    enes.updateLocation();
    }
    while (enes.location.theta != 270) {
    tank.setLeftMotorPWM(-255);
    tank.setRightMotorPWM(255);
    enes.updateLocation();
    }
    while (enes.location.y < enes.destination.y) {
    tank.setLeftMotorPWM(255);
    tank.setRightMotorPWM(255);
    enes.updateLocation();
    }
  
  }
  
}

/*
  signal = analogRead(1);
  
//Serial.println(signal);
  if (signal >= 300) {

    
    tank.setLeftMotorPWM(-255);
    tank.setRightMotorPWM(-255);
    for (int i =0; i<4;i++) {
    tank.setLeftMotorPWM(255);
    tank.setRightMotorPWM(-255);
    }
  }
  if (signal < 300) {

    tank.setLeftMotorPWM(255);
    tank.setRightMotorPWM(255);
  }
}

*/

