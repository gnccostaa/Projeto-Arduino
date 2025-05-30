O que é o Projeto IoT e Sistema de Monitoramento de Nível de Água para Reservatórios:
O acesso à água potável e o uso sustentável dos recursos hídricos são fundamentais para o desenvolvimento sustentável, conforme definido pelo ODS 6 (Água Potável e Saneamento). Este projeto propõe um sistema IoT para monitoramento do nível de água em reservatórios domésticos e industriais, utilizando sensores de nível e conectividade Wi-Fi. A implementação desse sistema permite evitar desperdícios, prevenir falta de água e auxiliar na gestão eficiente do recurso, especialmente em cidades como São Paulo, que enfrentam períodos alternados de chuvas intensas e estiagem prolongada.


Principais funções do código:
🔧 setup()
Objetivo: Inicializa o sistema.
Funções principais:
Inicia a comunicação serial.
Configura os pinos (sensor, LEDs, buzzer).
Conecta à rede Wi-Fi.
Define o servidor MQTT.

🔁 loop()
Objetivo: Executa continuamente a lógica principal.
Funções principais:
Verifica e mantém a conexão MQTT.
Faz a leitura da distância usando o sensor ultrassônico.
Controla os LEDs e o buzzer com base na distância lida.
Publica a distância no tópico MQTT.
Envia alerta por MQTT se o nível estiver muito baixo.

🔄 reconnectMQTT()
Objetivo: Garante que o ESP32 esteja sempre conectado ao servidor MQTT.
Funções principais:
Tenta conectar ao broker MQTT.
Caso falhe, aguarda 3 segundos e tenta novamente até conectar com sucesso.


Pseudocódigo:
INICIAR bibliotecas WiFi e MQTT

CONFIGURAR:
    - Nome e senha da rede Wi-Fi
    - Servidor MQTT (broker)
    - Tópicos MQTT para publicação de nível e alerta

DECLARAR pinos:
    - TRIG e ECHO para sensor ultrassônico
    - LEDs (CHEIO, MÉDIO, VAZIO)
    - BUZZER para alerta sonoro

VARIÁVEIS:
    - duration (tempo do pulso)
    - distance (distância calculada)
    - alertaEnviado (controle para evitar múltiplos envios)

FUNÇÃO SETUP:
    - Iniciar comunicação serial
    - Configurar pinos como entrada ou saída
    - Conectar ao Wi-Fi
    - Configurar conexão com servidor MQTT

FUNÇÃO reconnectMQTT:
    - Enquanto MQTT não estiver conectado:
        - Tentar conectar com ID "ESP32Reservatorio"
        - Se falhar, aguardar 3 segundos e tentar novamente

FUNÇÃO LOOP:
    - Se conexão MQTT caiu, reconectar
    - Executar client.loop() para manter MQTT ativo

    - Disparar o sensor ultrassônico (TRIG → HIGH por 10 microssegundos)
    - Ler tempo do retorno do pulso (pulseIn)

    - Se não houver retorno (timeout):
        - Definir distância como inválida
        - Apagar todos os LEDs
        - Desligar buzzer
        - Limpar alerta no MQTT se já havia sido enviado

    - Caso tenha leitura:
        - Calcular distância com base no tempo do pulso
        - Publicar distância no tópico "reservatorio/nivel"

        - Se distância entre 0 e 9 cm (nível alto):
            - Acender LED VERDE
            - Apagar outros LEDs
            - Desligar buzzer
            - Limpar alerta se já enviado

        - Se distância entre 10 e 19 cm (nível médio):
            - Acender LED AMARELO
            - Apagar outros LEDs
            - Desligar buzzer
            - Limpar alerta se já enviado

        - Se distância maior ou igual a 20 cm (nível baixo):
            - Acender LED VERMELHO
            - Ligar buzzer
            - Se alerta ainda não foi enviado:
                - Publicar "ALERTA: nível baixo!" no tópico "reservatorio/alerta"
                - Marcar alerta como enviado

    - Aguardar 500 milissegundos.

