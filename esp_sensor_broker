#include <WiFi.h>
#include <PubSubClient.h>

int value;
char str_value[8];

const char *SSID = "DIGI-X_LOUNGE";
const char *PASSWORD = "IT Certified";

const char* mqttServer= "soldier.cloudmqtt.com";
const int mqttPort = 12503;
const char* mqttUser = "uwywtlse";
const char* mqttPassword = "fyd2p65gmE1-";
#define trigPin 23
#define echoPin 19 

WiFiClient espClient;
PubSubClient client(espClient);

float duration,distance;

float cek_sensor(int out, int in){
    digitalWrite(out,LOW);
    delayMicroseconds(2);
    digitalWrite(out,HIGH);
    delayMicroseconds(10);
    digitalWrite(out,LOW);
    duration = pulseIn(in,HIGH);
    distance = (duration/29)/2;
    Serial.print("Jarak : ");
    Serial.print(distance);
    Serial.println(" cm");
    return distance;
  }

void connectToMqttCloud() {
  client.setServer(mqttServer, mqttPort);
  while ( !client.connected() ) {
    Serial.println("Mencoba koneksi dengan Broker MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword )) {
      Serial.println("tersambung dengan MQTT ");
    } else {
      Serial.print("Error ");
      delay(5000); // coba 5 detik kemudian
    }
  }
}

void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200);
  delay(1000);
  WiFi.disconnect(true);
  delay(2000);
  pinMode(trigPin,OUTPUT);
  pinMode(echoPin,INPUT);
  connectToHotSpot();
}

void loop() {
  // put your main code here, to run repeatedly:
  float jarak = cek_sensor(trigPin, echoPin);
 
  value = jarak;
  connectToMqttCloud();
  sprintf(str_value,"%d", value); // konversi integer ke string
  client.publish("INIX-Sensor", str_value); // publish dengan topic="INIX"
  delay(5000);
}

void connectToHotSpot() {
  WiFi.begin( SSID, PASSWORD );
  Serial.print("Trying to establish connection to WiFi Router");
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println();
  Serial.println("Connected! IP: ");
  Serial.println(WiFi.localIP());
}
