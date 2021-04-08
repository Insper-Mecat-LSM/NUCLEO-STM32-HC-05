Autor: Hugo Campos [link](https://github.com/HugocamposL3)

# Bluetooth HC-05 com Placa NUCLEO STM32 F103RB

## Objetivo:

 Verificar o funcionamento de um bluetooth HC-05 recebendo e enviando dados para a placa NUCLEO-F103RB e usando um celular android.
 
 ## Introdução:
 
Este tutorial tem o intuito de ensinar o leitor de uma forma clara e simples a usar um Bluetooth HC-05. A primeira prática é utilizar o HC-05 como transmissor de dados, ele basicamente irá enviar um valor lido na porta analógica da NUCLEO e o usuário receberá esse valor pelo o leitor serial do bluetooth. A segunda prática será ligar e desligar três leds comandados por dados enviados via bluetooth por um aplicativo android para um módulo 
HC-05, onde a placa NUCLEO-F103RB fará a interface entre os led e os dados recebidos pelo módulo.

## Prototipação:

### Prática 1:

Materiais Utilizados:
- 1 - LDR.
- 1 - RESISTOR de 10Kohm.
- 1 - Bluetooth HC-05.
- 1 - Protoboard.
- 1 - NUCLEO-F103RB.

### O que é um LDR:
É um resistor dependente de luz ou fotorresistência, conhecido pela sigla inglesa LDR (Light Dependent Resistor), é um componente eletrônico passivo do tipo resistor variável, mais especificamente, é um resistor cuja resistência varia conforme a intensidade da luz (iluminamento) que incide sobre ele.

<a href="https://imgur.com/ozot3Xb"><img src="https://imgur.com/ozot3Xb.jpg" title="source: imgur.com" /></a>

