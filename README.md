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

🔁 Loop Principal (resumo do pseudocódigo):
Verifica conexão MQTT
Se desconectado, tenta reconectar.
Lê a distância do sensor ultrassônico
Dispara o pulso e mede o tempo de resposta.
Se leitura inválida (sem resposta)
Desliga todos os LEDs e o buzzer.
Envia limpeza do alerta via MQTT (se necessário).
Se leitura válida
Publica a distância via MQTT.
Verifica a faixa de distância:
Até 9 cm: Liga LED verde (nível cheio), desliga alerta.
10 a 19 cm: Liga LED amarelo (nível médio), desliga alerta.
20 cm ou mais: Liga LED vermelho e buzzer (nível crítico), envia alerta via MQTT (se ainda não enviado).
Espera 500 ms e repete o ciclo.
            
