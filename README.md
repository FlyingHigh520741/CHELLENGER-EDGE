
# Projeto de Votação Interativa para Fórmula E Mahindra
Este projeto simula uma plataforma de votação interativa para os fãs da Fórmula E, permitindo que votem em seu piloto favorito e acompanhem a "corrida" virtual em tempo real. O protótipo foi desenvolvido em Arduino e pode ser facilmente adaptado para uma aplicação web real.

## Equipe
- Bento Rangel - RM559124
- Daniel Vieira - RM556275
- Thamiris Almeida - RM559155
- Werbeth Nunes - RM559067

## Demonstração Online
Experimente o protótipo no simulador Wokwi:

https://wokwi.com/projects/400716155579648001

## Como Funciona
1. Votação: Dois botões representam os pilotos ("Verde" e "Azul"). Cada pressionamento de botão registra um voto para o piloto correspondente.
2. Feedback Visual: LEDs nas cores dos pilotos acendem brevemente para confirmar o voto.
3. Contagem de Votos: O Arduino monitora os votos e os exibe no monitor serial.
4. Vitória: Quando um piloto atinge um número predefinido de votos (10 neste exemplo), ele é declarado vencedor.
5. Comemoração: O LED do piloto vencedor pisca para celebrar a vitória.
6. Reinício: A votação é reiniciada quando qualquer botão é pressionado após o término.

## Hardware Necessário
- Arduino Uno (ou compatível)
- 2 botões
- 2 LEDs (verde e azul)
- Resistores (para os LEDs e botões, se necessário)
- Jumpers

## Esquema de Ligação
- Botão Verde: Pino 2 do Arduino
- Botão Azul: Pino 3 do Arduino
- LED Verde: Pino 7 do Arduino
- LED Azul: Pino 8 do Arduino

## Código-Fonte (sketch.ino)
C++
   
    // Imprime a contagem dos votos na serial
    
    const int numPilotos = 2; // Corrigido para 2 pilotos

    // Pinos dos botões
    const int btgreen = 2;
    const int btdblue = 3;

    // Pinos dos LEDs
    const int ledgreen = 7;
    const int ledblue = 8;

    int votoPilotoG = 0;
    int votoPilotoB = 0;
    const int votosVitoria = 10;

    bool votacaoEncerrada = false; // Variável para controlar o estado da votação

    void setup() {
      Serial.begin(9600); 

      // Configura os pinos dos botões como entrada com pull-up interno
      pinMode(btgreen, INPUT_PULLUP);
      pinMode(btdblue, INPUT_PULLUP);

      // Configura os pinos dos LEDs como saída
      pinMode(ledgreen, OUTPUT);
      pinMode(ledblue, OUTPUT);
    }
    
    // Verifica se a votação está em andamento

    void loop() {
      if (!votacaoEncerrada) { 
        // Verifica se algum botão foi pressionado
        if (digitalRead(btgreen) == LOW) {
          votoPilotoG++;
          digitalWrite(ledgreen, HIGH); // Pisca o LED para indicar o voto
          delay(100);
          digitalWrite(ledgreen, LOW);
        }
        if (digitalRead(btdblue) == LOW) {
          votoPilotoB++;
          digitalWrite(ledblue, HIGH); // Pisca o LED para indicar o voto
          delay(100);
          digitalWrite(ledblue, LOW);
          }

        // Imprime a contagem dos votos na serial
    
        Serial.print("Votos: Verde: ");
        Serial.print(votoPilotoG);
        Serial.print(", Azul: ");
        Serial.println(votoPilotoB);

        // Verifica se algum piloto atingiu a vitória
    
        if (votoPilotoG >= votosVitoria) {
          vencedor(0); // Verde venceu
          Serial.print("Verde venceu!");
          votacaoEncerrada = true;
        } else if (votoPilotoB >= votosVitoria) {
          vencedor(1); // Azul venceu
          Serial.print("Azul venceu!");
          votacaoEncerrada = true;
        }
      } else {
        // A votação está encerrada, espera por um novo voto para reiniciar
        if (digitalRead(btgreen) == LOW || digitalRead(btdblue) == LOW) {
          votoPilotoG = 0;
          votoPilotoB = 0;
          votacaoEncerrada = false;
          digitalWrite(ledgreen, LOW); // Apaga os LEDs ao reiniciar
          digitalWrite(ledblue, LOW);
        }
      }

      delay(100); // Pequeno atraso para evitar leituras duplicadas
    }

    // Função para indicar o vencedor (simplificada)
    void vencedor(int piloto) {
      // Pisca o LED do vencedor rapidamente
      for (int i = 0; i < 10; i++) {
        digitalWrite((piloto == 0) ? ledgreen : ledblue, HIGH);
        delay(100);
        digitalWrite((piloto == 0) ? ledgreen : ledblue, LOW);
        delay(100);
      }

      // Mantém o LED do vencedor aceso
      digitalWrite((piloto == 0) ? ledgreen : ledblue, HIGH); 
    }

Use o código com cuidado.
content_copy
## Explicação do Código
- Variáveis:
  - numPilotos: Número de pilotos na corrida (2 neste caso).
  - btgreen, btdblue: Pinos dos botões.
  - ledgreen, ledblue: Pinos dos LEDs.
  - votoPilotoG, votoPilotoB: Contadores de votos para cada piloto.
  - votosVitoria: Número de votos necessários para vencer.
  - votacaoEncerrada: Flag para controlar o estado da votação.
  - lastDebounceTimeG, lastDebounceTimeB, debounceDelay: Variáveis para o debounce dos botões.
  - lastButtonStateG, lastButtonStateB: Estados anteriores dos botões.
  - btgreenPressed, btdbluePressed: Flags para indicar se os botões foram pressionados.
- setup(): Inicializa a comunicação serial e configura os pinos como entrada (botões) ou saída (LEDs).
- loop():
  - Verifica se a votação está em andamento:
  - Lê os estados dos botões e aplica debounce para evitar leituras falsas.
  - Incrementa os contadores de votos quando os botões são pressionados.
  - Pisca os LEDs para indicar o voto.
  - Imprime a contagem de votos no monitor serial.
  - Verifica se algum piloto atingiu a vitória e chama a função vencedor() se necessário.
  - Verifica se a votação está encerrada:
  - Espera por um novo pressionamento de botão para reiniciar a votação.
  - vencedor(): Pisca o LED do piloto vencedor e o mantém aceso.
## Próximos Passos
- Implementação Web: Adaptar o código Arduino para uma aplicação web utilizando tecnologias como HTML, CSS e JavaScript.
- Interface Gráfica: Criar uma interface visualmente atraente e intuitiva para a página de votação.
- Integração com Dados Reais: Obter dados em tempo real das corridas da Fórmula E para atualizar a "corrida" virtual.
- Recursos Adicionais: Adicionar placares de líderes, gráficos, animações e outras funcionalidades para enriquecer a experiência do usuário.
