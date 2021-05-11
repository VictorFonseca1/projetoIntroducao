String ssid     	= "Simulator Wifi";  	// SSID to connect to
String password 	= ""; 				 	// Our virtual wifi has no password (so dont do your banking stuff on this network)
String host     	= "api.thingspeak.com"; // Open API Requests - Write a Channel Feed
const int httpPort  = 80;					// Port number
String uri 			= "/update?api_key=&field1=";

// Função para criar o shield e conectá-lo
int setupESP8266(void) {
  // Start our ESP8266 Serial Communication
  Serial.begin(115200);   // Serial connection over USB to computer
  Serial.println("AT");   // Serial connection on Tx / Rx port to ESP8266
  delay(10);        // Wait a little for the ESP to respond
  if (!Serial.find("OK")) return 1;
    
  // Connect to 123D Circuits Simulator Wifi
  Serial.println("AT+CWJAP=\"" + ssid + "\",\"" + password + "\"");
  delay(10);        // Wait a little for the ESP to respond
  if (!Serial.find("OK")) return 2;
  
  // Open TCP connection to the host:
  Serial.println("AT+CIPSTART=\"TCP\",\"" + host + "\"," + httpPort);
  delay(50);        // Wait a little for the ESP to respond
  if (!Serial.find("OK")) return 3;
  
  return 0;
}
 
// Função para enviar a ...	
void enviaPotenciometroESP8266(void) {
  int ocupacao;
  
  // Variável para converter o valor de A0 (0 a 100)
  int temp = map(porcentagem,0,250,0,100); 
  
  // Construct our HTTP call
  String httpPacket = "GET " + uri + String(temp) + " HTTP/1.1\r\nHost: " + host + "\r\n\r\n";
  int length = httpPacket.length();
  
  // Send our message length
  Serial.print("AT+CIPSEND=");
  Serial.println(length);
  delay(10); // Wait a little for the ESP to respond

  // Send our http request
  Serial.print(httpPacket);
  delay(10); // Wait a little for the ESP to respond
  if (!Serial.find("SEND OK\r\n")) return;

}
int buttonStateIN = 0;
int buttonStateOUT = 0;
float capacidade = 250;
float ocupacao = 0;

void setup()
{
  pinMode(2, INPUT);
  pinMode(3, INPUT);
  pinMode(13, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(11, OUTPUT);
  // Inicia o shield
  setupESP8266();
}

void loop()
{
  // read the state of the pushbutton
  buttonStateIN = digitalRead(2);
  buttonStateOUT = digitalRead(3);
  // check if pushbutton is pressed. if it is, the
  // button state is HIGH
  if (buttonStateIN == HIGH) {
    ocupacao++;
  } 
  if (buttonStateOUT == HIGH) {
    ocupacao--;
  }
  float porcentagem = (float)ocupacao/capacidade;
  if (porcentagem>=0.8) {
    digitalWrite(13, HIGH);
  }
  else {
    digitalWrite(13, LOW);
  }
  if (porcentagem>=0.9) {
    digitalWrite(12, HIGH);
  }
  else {
    digitalWrite(12, LOW);
  }
  if (porcentagem==1) {
    digitalWrite(11, HIGH);
  }
  else {
    digitalWrite(11, LOW);
  }
  delay(10);
  enviaPotenciometroESP8266();
  Serial.println(ocupacao);
  
  
    
    
  delay(10); // Delay a little bit to improve simulation performance
}
