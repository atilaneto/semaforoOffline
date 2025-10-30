# Simulação de Semáforo (offline) com Ponteiros – Arduino

## Descrição do Projeto

Este projeto simula o funcionamento de um **semáforo veicular**, controlando três LEDs (vermelho, amarelo e verde) conectados a um **Arduino UNO**.
A lógica de temporização segue o funcionamento real de um cruzamento, alternando entre as luzes com tempos definidos para cada fase.

O diferencial deste código é o uso de **ponteiros em C++**, permitindo uma estrutura mais flexível e profissional — ideal para quem quer compreender melhor manipulação de memória e modularidade em microcontroladores.

---

## Objetivos da Atividade

* Montar fisicamente um semáforo com LEDs e resistores.
* Aplicar conhecimentos de **C/C++** e **ponteiros** em um projeto embarcado.
* Controlar as fases do semáforo com tempos específicos:

  * Vermelho: **6 segundos**
  * Verde: **4 segundos**
  * Amarelo: **2 segundos**
* Demonstrar o funcionamento físico por meio de um vídeo autoral.

---

## Componentes Utilizados

| Componente             | Quantidade | Especificação / Observação           |
| ---------------------- | ---------- | ------------------------------------ |
| Arduino Uno            | 1          | Placa microcontroladora              |
| LED vermelho           | 1          | 5 mm                                 |
| LED amarelo            | 1          | 5 mm                                 |
| LED verde              | 1          | 5 mm                                 |
| Resistores             | 3          | 220 Ω (ou 330 Ω) – um por LED        |
| Protoboard             | 1          | 400 ou 830 pontos                    |
| Jumpers macho-macho    | 6 a 8      | Para interligar Arduino e protoboard |
| Cabo USB               | 1          | Para alimentação e upload do código  |
| (Opcional) Suporte MDF | 1          | Estrutura visual do semáforo         |

---

## Esquema de Ligação

Cada LED é conectado **em série** com um resistor, conforme o diagrama abaixo:

```
[D13] --- Resistor ---|> LED Vermelho --- GND
[D12] --- Resistor ---|> LED Amarelo --- GND
[D11] --- Resistor ---|> LED Verde   --- GND
[GND Arduino] --------------------------- GND comum
```

Legenda:

* `|>` = LED (perna longa = positivo, perna curta = negativo)
* Sempre usar resistor no terminal positivo para limitar a corrente.

---

## Código-Fonte (semáforo com ponteiros)

```cpp
/*
  simulação de semáforo com 3 LEDs usando ponteiros.
  LEDs: vermelho (pino 13), amarelo (pino 12), verde (pino 11)
*/

#define ledVermelho 13
#define ledAmarelo  12
#define ledVerde    11

//criação de um array com os pinos
int leds[] = { ledVermelho, ledAmarelo, ledVerde };

//criação de um array com os tempos de cada LED (em milissegundos)
unsigned long tempos[] = { 6000, 2000, 4000 }; // vermelho, amarelo, verde

//ponteiros para os arrays
int *pLeds = leds;
unsigned long *pTempos = tempos;

void setup() {
  //inicializa cada LED como saída, usando o ponteiro
  for (int i = 0; i < 3; i++) {
    pinMode(*(pLeds + i), OUTPUT);
    digitalWrite(*(pLeds + i), LOW);
  }
}

//função genérica para acender qualquer LED
void acenderLed(int *pino, unsigned long *tempo) {
  digitalWrite(*pino, HIGH);
  delay(*tempo);
  digitalWrite(*pino, LOW);
}

void loop() {
  acenderLed(&leds[0], &tempos[0]); // Vermelho
  acenderLed(&leds[2], &tempos[2]); // Verde
  acenderLed(&leds[1], &tempos[1]); // Amarelo
}
```

---

## Explicação Técnica – Uso de Ponteiros

O código utiliza **ponteiros** para manipular arrays dinamicamente:

| Elemento                            | Descrição                                                                                      |
| ----------------------------------- | ---------------------------------------------------------------------------------------------- |
| `int *pLeds = leds;`                | Cria um ponteiro que aponta para o primeiro elemento do array `leds`.                          |
| `*(pLeds + i)`                      | Acessa o valor de cada posição do array usando aritmética de ponteiros.                        |
| `acenderLed(&leds[0], &tempos[0]);` | Passa o **endereço de memória** (referência) para a função, que controla o LED correspondente. |
| `digitalWrite(*pino, HIGH);`        | Usa o valor apontado pelo ponteiro (número do pino) para ligar o LED.                          |

Essa abordagem torna o código:

* Mais **genérico** (fácil adicionar mais LEDs e tempos);
* Mais **otimizado**, sem repetição de comandos;
* Um ótimo exemplo prático de **passagem por referência** em C/C++.

---

## Funcionamento Lógico do Ciclo

| Fase | LED ativo   | Tempo (ms) | Descrição                              |
| ---- | ----------- | ---------- | -------------------------------------- |
| 1    | Vermelho | 6000       | Veículos parados, pedestres atravessam |
| 2    | Verde    | 4000       | Liberação do tráfego de veículos       |
| 3    | Amarelo  | 2000       | Alerta de transição antes do vermelho  |

---

## Demonstração em Vídeo

**[https://drive.google.com/file/d/1yCfjrHD8DrhhVrm9Xi1VlPDybALdXsnm/view?usp=sharing]**

> O vídeo mostra o funcionamento físico da montagem (autor presente), com os LEDs alternando nas cores e tempos corretos.

---

## Testes Realizados

* Teste individual de cada LED com código de diagnóstico (`digitalWrite()` e `delay(1000)`).
* Verificação de continuidade nos fios e resistores.
* Ajuste de tempos com cronômetro externo.
* Validação da ordem correta das fases.
* Verificações no funcionamento da protoboard (sempre que os LEDs não ligavam, o problema era o funcionamento da protoboard)

---

## Autoria

**Aluno:** *[Átila Luís Flores Ramos Neto]*
**Curso:** Ciência da Computação
**Instituição:** *[Instituto de Tecnologia e Liderança]*
**Projeto:** Controle de semáforo com Arduino (atividade ponderada)
**Professor(a):** *[Crishna Irion]*

---

## Avaliação por Pares

| Avaliador               | Curso / Turma | Parecer        |
| ----------------------- | ------------- | -------------- |
| *[Lucas Faria]* | *[Turma 17]*     | *[Projeto completo, Átila. A montagem está correta e organizada (4 pontos), temporização de acordo com os critérios (3 pontos), código bem estruturado e organizado e utiliza ponteiros corretamente (3 pontos). O projeto é simples, mas eficiente e funcional.]* |
| *[Samuel Vono]* | *[Turma 17]*     | O projeto está bem montado, porém não utiliza as cores corretas nos jumpers. Isso significa que não foram aplicadas boas práticas de montagem (por exemplo, o uso do jumper preto para o GND e do vermelho para o VCC). Essa escolha dificulta a compreensão do circuito pelo avaliador, já que foram utilizadas apenas duas cores (azul e branco — Grêmio > Inter).
No código, recomenda-se sempre utilizar a função millis() em vez de delay(), para evitar a paralisação do circuito durante a execução. Além disso, é importante declarar as funções fora do loop() e apenas chamá-las dentro dele, garantindo uma estrutura mais limpa e modular.|

---

## Referências

* **Documentação Arduino:** [https://www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/)
* **C++ Pointer Basics – Arduino Docs:** [https://docs.arduino.cc/learn/programming/pointers](https://docs.arduino.cc/learn/programming/pointers)
* **Resistores para LEDs:** [https://learn.sparkfun.com/tutorials/leds](https://learn.sparkfun.com/tutorials/leds)
