#define echoPin 7 // Echo Pin
#define trigPin 8 // Trigger Pin
#define LEDPin 13 // Onboard LED
#define Led2 12 //LED 1 port
#define Led3 11 //LED 2 port
#define Led4 10 //LED 3 port
#define SpkrPin 9 //Speaker Pin port

int maximumRange = 1000; // Maximum range needed
int minimumRange = 0; // Minimum range needed
double duration, distance; // Duration used to calculate distance

void setup() {
 Serial.begin (9600); //Set up serial
 pinMode(trigPin, OUTPUT); //Set trigger to output
 pinMode(echoPin, INPUT); //Set echo to output
 pinMode(LEDPin, OUTPUT); // Use LED indicator (if required)
 pinMode(Led2, OUTPUT); //Set LED 1 to output
 pinMode(Led3, OUTPUT); //Set LED 2 to output
 pinMode(Led4, OUTPUT); //Set LED 3 to output
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
 pinMode(LEDPin, LOW);
 /* Send the distance to the computer using Serial protocol, and
 turn LED OFF to indicate successful reading. */
 if (distance >= 60){
   Serial.println("Out of range -- too far");
   digitalWrite(LEDPin, LOW);
 }
 else if (distance <= 0){
   Serial.println("Out of range -- too close");
   digitalWrite(LEDPin, HIGH);
 }
 else{
 for (int i = 0; i < distance; i++){
 Serial.print("###");
 }
 Serial.println(" ");
 }
 digitalWrite(LEDPin, LOW);
 
 //Turn off LED1-3
 digitalWrite(Led2, LOW);
 digitalWrite(Led3, LOW);
 digitalWrite(Led4, LOW);
if (distance > 2){ //When the distance is more than 3 in,
  digitalWrite(Led2, HIGH); //Turn LED1 on
  tone (SpkrPin, 261.5); //Speaker plays C
}
 if (distance > 8){ //When the distance is more than 6 in,
   digitalWrite(Led3, HIGH); //Turn LED2 on
   tone (SpkrPin, 329.5); //Speaker plays E
 }
 if (distance > 14){ //When the distance is more than 9 in,
   digitalWrite(Led4, HIGH); //Turn LED3 on
   tone (SpkrPin, 392); //Speaker plays G
 }
 //Delay 1/10 s before next reading
 delay(10);
}
