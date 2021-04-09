Autor: Hugo Campos [link](https://github.com/HugocamposL3)

# Bluetooth HC-05 com Placa NUCLEO-F103RB

## Objetivo:

 Verificar o funcionamento de um bluetooth HC-05 recebendo e enviando dados para a placa NUCLEO-F103RB e usando um celular android.
 
 ## Introdução:
 
Este tutorial tem o intuito de ensinar o leitor de uma forma clara e simples a usar um Bluetooth HC-05. A primeira prática é utilizar o HC-05 como transmissor de dados, ele basicamente irá enviar um valor lido na porta analógica da NUCLEO e o usuário receberá esse valor pelo o leitor serial do bluetooth. A segunda prática será ligar e desligar três leds comandados por dados enviados via bluetooth por um aplicativo android para um módulo HC-05, onde a placa NUCLEO-F103RB fará a interface entre os led e os dados recebidos pelo módulo.

### O que é um Módulo Bluetooth HC-05?
O módulo Bluetooth HC-05 é uma forma mais limpa de comunicação entre uma parte móvel e um equipamento fixo, esse módulo suporta os modos Mestre e Escravo diferente do módulo
HC-06. Em sua placa existe um regulador de tensão e você poderá alimentar com 3.3 a 5v, bem como um LED que indica se o módulo está pareado com outro dispositivo. Possui alcance de até 10m.
Antes de comunicar com o seu módulo você precisará pareá-lo com o dispositivo que desejas conectar. Isto vai variar dependendo do Sistema Operacional que você estará usando mas em termos gerais:

- Habilite o Bluetooth do seu dispositivo.
- Procure por outros dispositivos Bluetooth.
- Procure por um dispositivo chamado 'linvor' e pareie com ele.
- O código é '1234' ou '0000'.

<a href="https://imgur.com/MC1hwwm"><img src="https://imgur.com/MC1hwwm.jpg" title="source: imgur.com" /></a>

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
Um divisor de tensão é um circuito simples de resistores em série. A tensão de saída é uma fração fixa da tensão de entrada se os dois resistores tiverem valores fixos, mas também pode ser variada que é o caso desse circuito pois o LDR muda a sua resistência de acordo com a luminosidade. O fator de divisão é determinado por dois resistores.

<a href="https://imgur.com/zEuNNei"><img src="https://imgur.com/zEuNNei.jpg" title="source: imgur.com" /></a>

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

- 3º Passo: Declare os pinos que serão utilizados para comunicação serial entre o módulo Bluetooth HC-05 e a placa NUCLEO-F103RB

```javascript
Serial bt (PB_10, PB_11);
```

- 4º Passo: Declare o pino analógico de entrada da NUCLEO-F103

```javascript
AnalogIn LDR (A0);
```

- 5º Passo: Declare uma variável tipo INT para que o valor lido no pino analógico do microcontrolador seja gravado nela

```javascript
int ldrlido;
```

- 6º Passo: Dentro do Código principal será inicializado a comunicação serial do microprocessador e também do Módulo Bluetooth HC-05 com um baud rate de 9600 e uma
mensagem de inicialização será mostrada no leitor serial

```javascript
int main(void)
{
    pc.baud(9600);
    bt.baud(9600);
    
    pc.printf("Codigo Carregado\n\r");
    bt.printf("Codigo Carregado\n\r");
```

- 7º Passo: No loop principal a variável **ldrlido** receberá o valor lido pela o pino analógico do microcontrolador multiplicado por 1000 a cada 200ms, e esse valor será mostrado no leitor serial do bluetooth e também do computador. Nessa configuração quanto mais iluminado for o ambiente em que o usuário está, mas alto será o valor chegando 
até 1000.

```javascript
 while(1)
    {
        ldrlido = LDR.read()*1000;
        pc.printf ("Valor do LDR: %d\n\r", ldrlido);
        bt.printf ("Valor do LDR: %d\n\r", ldrlido);
        wait_ms(200);
    }
}
```

- Feito isso, é só compilar e gravar na NUCLEO-F103RB. Para que o autor pudesse ler os valores na serial do bluetooth em seu celular ele utilizou um aplicativo 
chamado **Serial Bluetooth Terminal** foi baixado pela play Store e também pode ser utilizado o **Bluetooth Terminal** também baixado pela play store. Abaixo
tem os dois aplicativos citados:

<a href="https://imgur.com/i75ngjx"><img src="https://imgur.com/i75ngjx.jpg" title="source: imgur.com" /></a>
<a href="https://imgur.com/eeJZd21"><img src="https://imgur.com/eeJZd21.jpg" title="source: imgur.com" /></a>

- Aplicativos funcionando mostrando os valores lidos:

<a href="https://imgur.com/WqGSBWL"><img src="https://imgur.com/WqGSBWL.jpg" title="source: imgur.com" /></a>
<a href="https://imgur.com/X656s1V"><img src="https://imgur.com/X656s1V.jpg" title="source: imgur.com" /></a>

### Prática 2:
Nessa prática o usuário irá enviar um dado para o módulo Bluetooth pelo o celular, o microcontrolador conectado ao Bluetooth via serial também receberá esse dado e esse microcontrolador vai fazer a interface entre os valores recebidos e os leds conectados a placa NUCLEO-F103RB, dependendo do valor recebido os leds irão acender 
ou apagar. 

Materiais Utilizados:
- 3 Leds.
- 1 Placa NUCLEO-F103RB.
- 1 Módulo Bluetooth HC-05.
- 1 Protoboard.
- 3 Resistores de 270ohm

### Esquemático da Prática:

<a href="https://imgur.com/bJgrJTe"><img src="https://imgur.com/bJgrJTe.jpg" title="source: imgur.com" /></a>

- Pino RX do Bluetooth será ligado no pino PB_10 da NUCLEO.
- Pino TX do Bluetooth será ligado no pino PB_11 da NUCLEO.
- Para alimentar o Bluetooth foi utilizado a saída 3.3V da NUCLEO.
- Os leds serão ligados nos D10, D9 e D8 da NUCLEO
- O GND é o mesmo para o circuito todo, foi utilizado o GND da NUCLEO.

### Escrevendo o Código:

Esse código é diferente da **Prática 1** porque o HC-05 não vai enviar os dados ele vai receber os dados. O programa consiste em dizer para o microcontrolador o que fazer
com determinados dados recebidos, por exemplo se o carácter **"A"** for recebido o led ligado no pino D0 vai acender ou apagar, no decorrer do código isso será mais detalhado.

- 1º Passo: Import a biblioteca padrão do mBed.

```javascript
#include "mbed.h"
```
- 2º Passo: Declare os pinos que serão utilizados para comunicação serial entre o módulo Bluetooth HC-05 e a placa NUCLEO-F103RB.

```javascript
Serial bt (PB_10, PB_11);
```
- 3º Passo: Declare os pinos de saidas que serão ligados nos Leds.

```javascript
DigitalOut led1 (D10);
DigitalOut led2 (D9);
DigitalOut led3 (D8);
```

- 4º Passo: Dentro do código Principal é necessário declarar uma variável tipo carácter **char** nomeada de **ch** nessa variável será gravada o dado enviado para o bluetooth 
e também é inicializado a comunicação serial do bluetooth com um baud rate de 9600 e também uma mensagem será mostrada na serial conectada ao bluetooth.

```javascript
int main(void)
{
    char ch;
    
    bt.baud(9600);

    bt.printf("Codigo Carregado\r\n");
```
- 5º Passo: E por fim no loop principal o código vai verificar se tem algum carácter enviado para a serial do bluetooth, se sim, ele vai ler esse carácter e de acordo do que foi recebido o microcontrolador vai acionar ou desligar um led. 

```javascript
while(1)
    {
        if(bt.readable())
        {
            ch = bt.putc(bt.getc());
            if (ch == 'A')
            {
                led1 = 1;
                wait_ms(200);
            }
            else if (ch == 'B')
            {
                led1 = 0;
                wait_ms(200);
            }
            else if (ch == 'C')
            {
                led2 = 1;
                wait_ms(200);
            }
            else if (ch == 'D')
            {
                led2 = 0;
                wait_ms(200);
            }  
            else if (ch == 'E')
            {
                led3 = 1;
                wait_ms(200);
            }  
            else if (ch == 'F')
            {
                led3 = 0;
                wait_ms(200);
            }     
        }
    }
}
```
- A letra **A** liga o Led 1, a letra **B** desliga o Led 1.
- A letra **C** liga o led 2, a letra **D** desliga o Led 2.
- A letra **E** liga o led 2, a letra **F** desliga o Led 3.

### Mas como eu envio esses carácteres para o Módulo Bluetooth?

Com os mesmos aplicativos que o celular lia os valores da variável da prática 1, só que nesse código o celular não vai ler nada pelo o contrário ele vai enviar. Não se esqueça
que nesse código todas as letras estão em maiúsculos.

<a href="https://imgur.com/Wr4VgZP"><img src="https://imgur.com/Wr4VgZP.jpg" title="source: imgur.com" /></a>
<a href="https://imgur.com/hfEmB43"><img src="https://imgur.com/hfEmB43.jpg" title="source: imgur.com" /></a>

# Considerações Finais:
Fazendo essa duas práticas o leitor vai poder operar o bluetooth em dois modos, como receptor de dados e também como transmissor de dados, isso pode ser muito útil em um projeto
em que o usuário precise monitorar os status da sua linha de produção de uma forma que ele não precise ficar muito próximo da máquina, somente usando sensores, um módulo 
bluetooth e um microcontrolador é possível receber esses dados pelo o seu celular. Em caso de alguma dúvida que o leitor tenha tido no decorrer desse tutorial entre em contato
com o autor. 

