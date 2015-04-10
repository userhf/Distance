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
 if (distance > 1){//When the distance to the sensor is 1 in,
 tone (SpkrPin, 261.63) //Speaker plays Middle C
 digitalWrite(Led2, HIGH); //Turn on first LED
 }
 if (distance > 6){ //When the distance to the sensor is 6 in,
 tone (SpkrPin, 293.66); //Speaker plays D
 }
 if (distance > 11){ //When the distance to the sensor is 11 in,
 tone (SpkrPin, 329.63); //Speaker plays E
 digitalWrite(Led3, HIGH); //Turn on second LED
 }
 if (distance > 16){ //When the distance to the sensor is 16 in,
 tone (SpkrPin, 349.23); //Speaker plays F
 }
 if (distance > 21){ //When the distance to the sensor is 21 in,
 tone (SpkrPin, 783.99); //Speaker plays G
 }
 if (distance > 26){ //When the distance to the sensor is 26 in,
 tone (SpkrPin, 440); //Speaker plays A
 digitalWrite(Led3, HIGH); // Turn on third LED
 }
 if (distance > 31){ //When the distance to the sensor is 31 in,
 tone (SpkrPin, 493.88); //Speaker plays B
 }
 if (distance > 36){ //When the distance to the sensor is 36 in,
 tone (SpkrPin, 523.25); //Speaker plays C
 }
 //Delay 1/10 s before next reading
 delay(10);
}
