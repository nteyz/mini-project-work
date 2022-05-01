# mini-project


# TURNING ON A MOTOR WITH A FINGER PRINT SENSOR


# 1.ABSTRACT:
This project entitled" Designe and implementation of turning ON motor by using finger print sensor"  is designed for switching  a motor that Will raising and increasing the excess security of system and this system is reliable, this project is composed by three main parts which are sensing part, control part, and indicating part. Sensing part is for fingerprintsensor that Will be able to know the fingerprint codes to operate the system Is a type of electronic security system that uses fingerprints for biometric authentication to grant a user access to information or to approve transaction Fingerprint scanner work by capturing the pattern of the ridges and valley on finger. The information is then processed by the device’s pattern analysis / matching software, which compares it to the list of registered fingerprint on file. Asuccesful matching means an identification has been verified, thereby gradient access. while in control part includes processing unity ( microcontroller) that Will process the operation of the action to b excecuted, indicating part will show the user how on and off actions are taking place. There is also Arduino contains microchip used with finger print, the system it can only be opened when an authorized user is present. In this project, we used also fingerprint Module and Arduino to take and keep the data and records. By using fingerprint sensor, the system will become more secure for the users.
Where if the owner of a motor put a finger on the fingerprint screen,the fingerprint will identify and cognize the fingercodes that Will changed into electrical signal to turns on a relay that Will operate the motor.
While if the other person puts a finger in the fingerprint screen, it will be able to understand that the fingerprint codes is not authorized, that means Will take that finger's codes as an error so our fingerprintsensor will not command the the processor to turn on a relay because the finger codes is not authorized.













# 2. PROBLEM STATEMENTS
After noticing that there is a problem of security along system and luck of confidence to start system (turn on) most people and industries need to turn on their system, the use of  keys, password and other switches introduced, but their meet with the issues or challenges that are appear in such situation.
Firstly, same people need to secure their system which cannot be easy and not confidential using password, keys and other switching system.
Secondary, according to the lost of security pattern or password and keys of starting system especialsystemincludesmotor. After seeing those problems, I through about how to solve those problems by using fingerprint system. 


# 3.BLOCK DIAGRAM AND IT'S DESCRIPTION
 


Fingerprint sensor module captures finger’s print image and then converts it into the equivalent template and saves them into its memory as per selected ID by Arduino. All the process is commanded by Arduino like taking an image of finger’s print, convert it into templates and storing as ID etc.
Arduino is the main component of this system it is responsible for control of the whole system.







  
# 4.CIRCUIT DIAGRAM
 

Fingerprint module’s Rx and Tx directly connected at Serial pin D2 and D3 (Software Serial) of Arduino. 5v supply is used for powering finger print module  and relay that are taken from Arduino board. Battery of 12v is used to supply the motor as our output (actuator).

# COMPILED SOURCE CODES IN ARDUINO IDE

 
 
 

 
# Source code:

#include <Adafruit_Fingerprint.h>
#include <SoftwareSerial.h>
SoftwareSerial mySerial(2, 3);

Adafruit_Fingerprint finger = Adafruit_Fingerprint(&mySerial);

void setup()  
{
  
  pinMode(9,OUTPUT);
  Serial.begin(9600);
  while (!Serial);  
  delay(100);
  Serial.println("\n\nAdafruit finger detect test");
  finger.begin(57600);
  
  if (finger.verifyPassword()) {
    Serial.println("Found fingerprint sensor!");
  } else {
    Serial.println("Did not find fingerprint sensor :(");
    while (1) { delay(1); }
  }

  finger.getTemplateCount();
  Serial.print("Sensor contains "); 
  Serial.print(finger.templateCount); 
  Serial.println(" templates");
  Serial.println("Waiting for valid finger...");
}

void loop()                    
{
  getFingerprintIDez();
  delay(5);           
digitalWrite(9,HIGH);
}
uint8_t getFingerprintID() {
  uint8_t p = finger.getImage();
  switch (p) {
    case FINGERPRINT_OK:
      Serial.println("Image taken");
      break;
    case FINGERPRINT_NOFINGER:
      Serial.println("No finger detected");
      return p;
    case FINGERPRINT_PACKETRECIEVEERR:
      Serial.println("Communication error");
      return p;
    case FINGERPRINT_IMAGEFAIL:
      Serial.println("Imaging error");
      return p;
    default:
      Serial.println("Unknown error");
      return p;
  }
  p = finger.image2Tz();
  switch (p) {
    case FINGERPRINT_OK:
      Serial.println("Image converted");
      break;
    case FINGERPRINT_IMAGEMESS:
      Serial.println("Image too messy");
      return p;
    case FINGERPRINT_PACKETRECIEVEERR:
      Serial.println("Communication error");
      return p;
    case FINGERPRINT_FEATUREFAIL:
      Serial.println("Could not find fingerprint features");
      return p;
    case FINGERPRINT_INVALIDIMAGE:
      Serial.println("Could not find fingerprint features");
      return p;
    default:
      Serial.println("Unknown error");
      return p;
  }
  
  p = finger.fingerFastSearch();
  if (p == FINGERPRINT_OK) {
    Serial.println("Found a print match!");
  } else if (p == FINGERPRINT_PACKETRECIEVEERR) {
    Serial.println("Communication error");
    return p;
  } else if (p == FINGERPRINT_NOTFOUND) {
    Serial.println("Did not find a match");
    return p;
  } else {
    Serial.println("Unknown error");
    return p;
  }   
  
  Serial.print("Found ID #"); Serial.print(finger.fingerID); 
  Serial.print(" with confidence of "); Serial.println(finger.confidence); 

  return finger.fingerID;
}

int getFingerprintIDez() {
  uint8_t p = finger.getImage();
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.image2Tz();
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.fingerFastSearch();
  if (p != FINGERPRINT_OK)  return -1;
digitalWrite(9,LOW);
  delay(20000);
  Serial.print("Found ID #"); Serial.print(finger.fingerID); 
  Serial.print(" with confidence of "); Serial.println(finger.confidence);
  return finger.fingerID; 
}

