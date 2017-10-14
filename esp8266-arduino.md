//http://projectpi.pythonanywhere.com/get_data
//https://www.pythonanywhere.com/user/projectPI/files/var/log/projectpi.pythonanywhere.com.access.log
//https://www.pythonanywhere.com/user/projectPI/files/var/log/projectpi.pythonanywhere.com.server.log

#include <ESP8266WiFi.h>

String odebraneDane = ""; //pusty ciąg odebranych danych z Arduino

const char* ssid     = "***";
const char* password = "***";

const char* host = "http://projectpi.pythonanywhere.com";

float start = 1555623531678;
float timer;
float x;
float y;
float z;


void setup() {
  Serial.begin(115200);
  delay(100);

  // We start by connecting to a WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

 while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected - POU");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP()); 
}

void loop() {

  
//if (Serial.available()) {

  odebraneDane = random(2, 8); //Serial.readStringUntil('\n');

  WiFiClient client;
  const int httpPort = 80;
  if (!client.connect(host, httpPort)) {
    Serial.println("connection failed");
    return;
  }
   

   Serial.print("Requesting POST: ");
  
   client.println("POST /get_data?x=" + odebraneDane + " HTTP/1.1");
   client.println("Host: projectpi.pythonanywhere.com");
   client.println("Accept: */*");
   client.println("Content-Type: text/plain");
   client.print("Content-Length: ");
   client.println(odebraneDane.length());
   client.println();
   client.print(odebraneDane);

   delay(500); // Can be changed
   
  
  if (client.connected()) { 
    client.stop();  // DISCONNECT FROM THE SERVER
  }
  Serial.println();
  Serial.println("closing connection");
  delay(5000);
  //}
}
