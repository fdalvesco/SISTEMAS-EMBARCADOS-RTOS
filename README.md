# MQTT (18/06/2024)
<p align="justify">
O MQTT (Message Queuing Telemetry Transport) é um protocolo de mensagens leve ideal para conectar dispositivos IoT de maneira eficiente em redes com largura de banda limitada.\
Além de ser simples de implementar e usar, o MQTT oferece diferentes níveis de entrega de mensagens, garantindo confiabilidade aos dispositivos IoT conectados.\
Amplamente utilizado, o MQTT conecta e controla dispositivos inteligentes, coleta dados de sensores distribuídos para monitoramento e integra dispositivos em automação residencial ou industrial, facilitando a comunicação entre dispositivos IoT de maneira escalável através do uso de tópicos para direcionar mensagens aos destinatários adequados. </p>

## MQTT -Na Prática
* Publish/Publicação: Dispositivos enviam mensagens para um tópico específico.
* Subscribe/Assinatura: Dispositivos interessados se inscrevem nos tópicos para receber as mensagens.
* Broker MQTT: Serve como intermediário que recebe mensagens publicadas e as encaminha aos dispositivos assinantes dos tópicos correspondentes.

## Código Comentado

  #include <Arduino.h>
  #include <ESP8266WiFi.h>
  #include <PubSubClient.h>
  
  const char* ssid = "GABRIEL";  // Nome da rede WiFi
  const char* password = "12345678";  // Senha da rede WiFi
  const char* mqtt_server = "192.168.10.85";  // Endereço IP do servidor MQTT
  
  WiFiClient espClient;  // Cliente WiFi
  PubSubClient client(espClient);  // Cliente MQTT usando o cliente WiFi
  const int Relay = D1;  // Pino ao qual está conectado o relé
  
  void setup_wifi() {
    delay(10);
    Serial.println();
    Serial.print("Connecting to ");
    Serial.println(ssid);
    WiFi.begin(ssid, password);  // Conecta à rede WiFi especificada
    while (WiFi.status() != WL_CONNECTED) {  // Aguarda até que a conexão seja estabelecida
      delay(500);
      Serial.print(".");
    }
    Serial.println("");
    Serial.println("WiFi connected");
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());  // Imprime o endereço IP atribuído ao ESP8266
  }
  
  void callback(char* topic, byte* payload, unsigned int length) {
    Serial.print("Message arrived [");
    Serial.print(topic);
    Serial.print("] ");
    String message;
    for (int i = 0; i < length; i++) { 
      Serial.print((char)payload[i]);  // Imprime cada byte da mensagem recebida
      message += (char)payload[i];  // Constrói a string da mensagem recebida
    }
    Serial.println(message);  // Imprime a mensagem completa no Serial Monitor
  
    if (String(topic) == "ControleRelay") {  // Verifica se o tópico da mensagem é "ControleRelay"
      if (message == "ON") {
        digitalWrite(Relay, LOW);  // Liga o relé se a mensagem for "ON"
      } else if (message == "OFF") {
        digitalWrite(Relay, HIGH);  // Desliga o relé se a mensagem for "OFF"
      }
    }
  }
  
  void setup() {
    pinMode(Relay, OUTPUT);  // Configura o pino do relé como saída
    digitalWrite(Relay, HIGH);  // Inicialmente desliga o relé
    Serial.begin(9600);  // Inicializa o Serial Monitor com baud rate 9600
    setup_wifi();  // Inicia a conexão WiFi
    client.setServer(mqtt_server, 1883);  // Configura o servidor MQTT e porta
    client.setCallback(callback);  // Define a função de callback para mensagens MQTT
  }
  
  void reconnect() {
    while (!client.connected()) {  // Enquanto não estiver conectado ao servidor MQTT
      Serial.print("Attempting MQTT connection...");
      if (client.connect("ESP8266Client")) {  // Tenta se reconectar ao servidor MQTT
        Serial.println("connected");
        client.subscribe("ControleRelay");  // Se conectado, inscreve-se no tópico "ControleRelay"
      } else {
        Serial.print("failed, rc=");
        Serial.print(client.state());  // Exibe o estado da conexão MQTT
        Serial.println(" try again in 5 seconds");
        delay(5000);  // Espera 5 segundos antes de tentar novamente
      }
    }
  }
  
  void loop() {
    if (!client.connected()) {
      reconnect();  // Se não estiver conectado, tenta reconectar
    }
    client.loop();  // Mantém a comunicação com o servidor MQTT
  }


# SISTEMAS-EMBARCADOS-RTOS (04/06/2024)

## Esquemático
![Untitfed 1](https://github.com/fdalvesco/SISTEMAS-EMBARCADOS-RTOS/assets/101358513/c240dab4-dfb1-4910-b845-6ec7dcf2b1a8)

## PCB
![Untitlged 2](https://github.com/fdalvesco/SISTEMAS-EMBARCADOS-RTOS/assets/101358513/b8b208f4-6b86-47a4-b9e2-03081d31f263)



## 3D
![Untitled 1](https://github.com/fdalvesco/SISTEMAS-EMBARCADOS-RTOS/assets/101358513/e8956ccc-bbfe-4036-94bc-ca469ca947cf)

![Untitled 2](https://github.com/fdalvesco/SISTEMAS-EMBARCADOS-RTOS/assets/101358513/bcdba762-3b62-4ec6-b635-0c7860891522)
