Autor: Hugo Campos [link](https://github.com/HugocamposL3)

# Bluetooth HC-05 com Placa NUCLEO STM32 F103RB

## Objetivo:

 Verificar o funcionamento de um bluetooth HC-05 recebendo e enviando dados para a placa NUCLEO-F103RB e usando um celular android.
 
 ## Introdução:
 
Este tutorial tem o intuito de ensinar o leitor de uma forma clara e simples a usar um Bluetooth HC-05. A primeira prática é utilizar o HC-05 como transmissor de dados, ele basicamente irá enviar um valor lido na porta analógica da NUCLEO e o usuário receberá esse valor pelo o leitor serial do bluetooth. A segunda prática será ligar e desligar três leds comandados por dados enviados via bluetooth por um aplicativo android para um módulo 
HC-05, onde a placa NUCLEO-F103RB fará a interface entre os led e os dados recebidos pelo módulo.

## Prototipação:

### Prática 1:

Essa prática consiste em ler valores na porta analógica do microcontrolador de acordo com a iluminação do ambiente e esses valores serão lidos pelo o celular do usuário via bluetooth, para isso foi utilizado um componete chamado LDR. 
### Mas o que é um LDR?
É um resistor dependente de luz ou fotorresistência, conhecido pela sigla inglesa LDR (Light Dependent Resistor), é um componente eletrônico passivo do tipo resistor variável, mais especificamente, é um resistor cuja resistência varia conforme a intensidade da luz (iluminamento) que incide sobre ele.

<a href="https://imgur.com/tghZ1u3"><img src="https://imgur.com/tghZ1u3.jpg" title="source: imgur.com" /></a>

Materiais Utilizados:
- 1 - LDR.
- 1 - RESISTOR de 10Kohm.
- 1 - Bluetooth HC-05.
- 1 - Protoboard.
- 1 - NUCLEO-F103RB.

### Esquemático da prática:

<a href="https://imgur.com/09kxomA"><img src="https://imgur.com/09kxomA.jpg" title="source: imgur.com" /></a>

### Por que o LDR está ligado em série com um resistor ?
Para que o microcontrolador possa ler essa variação de resistência do LDR de acordo com a luminosidade é necessário usar um divisor de tensão.
### O que é um divisor de tensão?
Um divisor de tensão é um circuito simples de resistores em série. A tensão de saída é uma fração fixa da tensão de entrada. O fator de divisão é determinado por dois resistores.

<a href="https://imgur.com/T5d8Lpv"><img src="https://imgur.com/T5d8Lpv.jpg" title="source: imgur.com" /></a>

- Pino RX do Bluetooth HC-05 será ligado no pino PB_10 da NUCLEO-F103RB.
- Pino TX do Bluetooth HC-05 será ligado no pino PB_11 da NUCLEO-F103RB.
- Como tensão de entrada do Bluetooth HC-05 foi utilizado a tensão 3.3V direto da NUCLEO-F103RB.
- A tensão de saida do Divisor de tensão entre o resistor de 10Kohm e o LDR foi conectada no Pino A0 da NUCLEO-F103RB.
- O GND é o mesmo para todo circuito, foi utilizado o GND da NUCLEO-F103RB.

### Programando com mBed:

Para programar a placa NUCLEO, será utilizado o compilador online chamado mBed, é necessário fazer o login no site do fornecedor [link](https://os.mbed.com/) e logo após abrir o compilador online para começar o seu código, nesse tutorial o autor entende que o leitor já tem o básico para programar na plataforma online mBed, então por isso ele não vai colocar o passo a passo para isso.

### Escrevendo o Código:

Nesse código não é utilizado funções complexas e só com a biblioteca padrão do mBed é possivel compilar e simular esse projeto.

- 1º Passo: Import a biblioteca padrão do mBed

```javascript
#include "mbed.h"
```
- 2º Passo: Declare que será utilizado a comunicação serial da NUCLEO-F103

```javascript
Serial pc(USBTX, USBRX);
```

- 3º Passo: Declare os pinos que serão utilizados para comunicação serial entre o módulo Bluetooth HC-05  e a placa NUCLEO-F103RB

```javascript
Serial bt (PB_10, PB_11);
```

- 4º Passo: Declare o pino analógico de entrada da NUCLEO-F103

```javascript
AnalogIn LDR (A0);
```

- 5º Passo: Declare uma variavel tipo INT para que o valor lido no pino analógico do microcontrolador seja gravado nela

```javascript
int ldrlido;
```




