#define echoPin 7 // Echo Pin
#define trigPin 8 // Trigger Pin
#define LEDPin 13 // Onboard LED
#define Led2 12 //LED 1
#define Led3 11 //LED 2
#define Led4 10 //LED 3

int maximumRange = 200; // Maximum range needed
int minimumRange = 0; // Minimum range needed
double duration, distance; // Duration used to calculate distance

void setup() {
 Serial.begin (9600); //Set up serial
 pinMode(trigPin, OUTPUT); //Set trigger to output
 pinMode(echoPin, INPUT); //Set echo to output
 pinMode(LEDPin, OUTPUT); // Use LED indicator (if required)
}

void loop() {
/* The following trigPin/echoPin cycle is used to determine the
 distance of the nearest object by bouncing soundwaves off of it. */ 
 digitalWrite(trigPin, LOW); 
 delayMicroseconds(2); 

 digitalWrite(trigPin, HIGH);
 delayMicroseconds(10); 
 
 digitalWrite(trigPin, LOW);
 duration = pulseIn(echoPin, HIGH);
 
 distance = duration/58.2; //Calculate the distance in centimeters
 distance = distance*0.393701; //Convert distance to inches
 if (distance >= maximumRange || distance <= minimumRange){ //if it is out of range
 /* Send error message to computer and Turn onboard LED ON 
 to indicate "out of range" */
 Serial.println("ERROR");
 digitalWrite(LEDPin, HIGH); 
 }
 else {
 /* Send the distance to the computer using Serial protocol, and
 turn LED OFF to indicate successful reading. */
 for (int i = 0, i <distance, i++){
 Serial.println("#");
 }
 digitalWrite(LEDPin, LOW);
 
 //Turn off LED1-3
 digitalWrite(Led2, LOW);
 digitalWrite(Led3, LOW);
 digitalWrite(Led4, LOW);
if (distance > 3){ //When the distance is more than 3 in,
  digitalWrite(Led2, HIGH); //Turn LED1 on
}
 if (distance > 6){ //When the distance is more than 6 in,
   digitalWrite(Led3, HIGH); //Turn LED2 on
 }
 if (distance > 9){ //When the distance is more than 9 in,
   digitalWrite(Led4, HIGH); //Turn LED3 on
 }
 }
 //Delay 70 ms before next reading
 delay(70);
}
