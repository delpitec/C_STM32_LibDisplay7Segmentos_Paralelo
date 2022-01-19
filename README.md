# Biblioteca para displays de 7 segmentos com STM32

**Descrição da biblioteca:**<br>
A biblioteca permite a fácil utilização de displays de 7 segmentos conectados de modo "paralelo" (isso significa que para cada pino de cada display de 7 segmentos, deverá ser utilizado um pino do microcontrolador). Possui limitação da configuração máxima de 4 displays de 7 segmentos (devido à quantidade de pinos disponível no STM32F103C8T6). 
Existem outros tipos de maneiras de utilizar os displays de 7 segmentos com o STM32, porém esta é uma solução diferenciada no quesito de facilidade e versatilidade, uma vez que o projetista precisa de poucas linhas de código para exibir um valor numérico ou uma mensagem (conforme padrão de codigicação alfanumérico para o display de 7 segmentos) no display.

 &nbsp;<br>

**Status do desenvolvimento:**<br>
✅ Concluído

 &nbsp;<br> 
 
**Modo de utilização da biblioteca (passo-a-passo):**

1 - Defina a quantidade de displays que irá utilizar (1 até 4 displays);&nbsp;<br>
2 - Atribua identificadores a cada um dos displays (ID #1 até ID #4);&nbsp;<br>
3 - Realize o cabeamento ponto-a-ponto entre o STM32F103C8T6 e cada um dos displays utilizando displays de 7 segmentos do tipo cátodo comum e com resistores limitadores de corrente de no mínimo 330Ω; 


![WhatsApp Image 2021-08-12 at 15 35 35](https://user-images.githubusercontent.com/58537514/129255545-6fe489da-7bc1-4a77-a6ea-4b32e0eada2c.jpeg)

**OBS:** A figura acima, que é a montagem base para este passo-a-passo, respeita a seguinte sequência de ligação:

| From<br>STM32 Pin | To<br>Display ID / seg|
| :---:   | :-: |
| B0 | 1 / a |
| B1 | 1 / b |
| B14 | 1 / c |
| B12 | 1 / d |
| B13 | 1 / e |
| B10 | 1 / f |
| B11 | 1 / g |
| B3 | 2 / a |
| A15 | 2 / b |
| A9 | 2 / c |
| A8 | 2 / d |
| B15 | 2 / e |
| A7 | 2 / f |
| A6 | 2 / g |
| A0 | 3 / a |
| A1 | 3 / b |
| A12 | 3 / c |
| A11 | 3 / d |
| A10 | 3 / e |
| A5 | 3 / f |
| A4 | 3 / g |

&nbsp;<br>
4 - No Device Configuration Tool, atribuir o nome dos pinos de acordo com a tabela acima, com a seguinte nomenclatura: <br>
&nbsp;&nbsp;&nbsp; DISPLAYX_LED_Y &nbsp; **Onde {X = #ID do display | Y = segmento do display}** conforme figura a seguir:
&nbsp;<br>

![WhatsApp Image 2021-08-12 at 16 46 36](https://user-images.githubusercontent.com/58537514/129260752-6f99b494-ab9b-45f1-8516-b368e167353d.jpeg)
**OBS:** Caso tenha dúvidas na criação do projeto, verifique aqui [como criar um projeto desde o início com a ferramenta STM32CubeIDE](https://www.youtube.com/watch?v=0UmISQhm_8k&t=338s)

&nbsp;<br>
5 - Após criar o projeto, insira os [arquivos .h e .c deste repositório](https://github.com/delpitec/C_STM32_STM32F103C8T6_BluePill_MyLibraries-main/tree/main/LibDisplay7Segmentos_Paralelo)  nas seguintes pastas do projeto a seguir **(1)** e realize a inserção da seguinte linha de código no local mostrado a seguir **(2)**:
```ruby
#include "7SegParallel_v1.2.h"
```
![ttttt](https://user-images.githubusercontent.com/58537514/129263685-3a79c01d-2cb2-4730-b981-737faac0176c.png)

&nbsp;<br>
6 - Usando a biblioteca:
&nbsp;<br>
&nbsp;<br>
Para inicializar a biblioteca, escrever o seguinte código:
```ruby
DisplaySeteSegmentos Valor_1 = IniDisplay(3,1,2,3); // IniDisplay(Qtd de displays , Caractere mais significativo,...,Cacactere menos significativo)
```
Acima, estamos inicializando a variável Valor_1 que possui 3 displays, sendo que o display 1 é o caractere mais significativo, e o display 3 é o menos significativo.
&nbsp;<br>
&nbsp;<br>
&nbsp;<br>
Caso queira inverter a ordem dos caracteres, basta inverter a ordem da declaração dos IDs dos caracteres conforme a seguir:
```ruby
DisplaySeteSegmentos Valor_1 = IniDisplay(3,3,2,1);
```
Acima, estamos inicializando a variável Valor_1 que possui 3 displays, sendo que o display 3 é o caractere mais significativo, e o display 1 é o menos significativo.
&nbsp;<br>
&nbsp;<br>
&nbsp;<br>
É possível também atribuir valores diferentes, assim permitindo que cada um exiba um dado diferente:
```ruby
DisplaySeteSegmentos Valor_1 = IniDisplay(1,1);   // Valor_1 exibe dado com uma unidade no display ID #1
DisplaySeteSegmentos Valor_2 = IniDisplay(2,2,3); // Valor_2 exibe dado com duas unidades nos displays ID #2 e ID #3
```
&nbsp;<br>
&nbsp;<br>
6.1 - Escrevendo valores numéricos:
Para escrever valores numéricos (positivos, 0 ~ 9 para 1 display, 00 até 99 para 2 displays...), basta utilizar a função a seguir:
```ruby
void EscreveNumero(DisplaySeteSegmentos* DisplayConfigurado , uint16_t Valor);
```
A seguir, exemplo de código que escreve o número 16 em uma representação com 3 displays de 7 segmentos:
```ruby
DisplaySeteSegmentos Valor_1 = IniDisplay(3,1,2,3); // Inicializa valor com 3 dígitos
EscreveNumero(&Valor_1, 16);   // Escreve 016 nos 3 dígitos do display
```
&nbsp;<br>
&nbsp;<br>
Conforme comentado anteriormente, podemos ter dois displays diferentes conforme a seguir, onde o Valor_1 possui 1 dígito e escreve o número 8 e o Valor_2 possui 2 dígitos e escreve o número 24:
```ruby
DisplaySeteSegmentos Valor_1 = IniDisplay(3,1,2);
DisplaySeteSegmentos Valor_2 = IniDisplay(1,3);

EscreveNumero(&Valor_1, 24);   // Escreve 24 nos 2 dígitos que estão usando os displays ID #1 e ID #2
EscreveNumero(&Valor_2, 6);    // Escreve  6 no dígito que está usando o display ID #3
```

6.2 - Escrevendo mensagens:
Para escrever mensagens de texto no display, ele se comporta da seguinte forma:
&nbsp;<br>
&nbsp;<br>
- Caso a quantidade de caracteres do texto seja igual a quantidade de displays, exibe mensagem estática
```ruby
DisplaySeteSegmentos Valor_1 = IniDisplay(3,1,2,3);
EscreveString(&Valor_1, "Ola");   // Escreve Ola de forma estática nos 3 displays de 7 segmentos
```

&nbsp;<br>
&nbsp;<br>
- Caso a quantidade de caracteres do texto seja maior que a quantidade de displays, a cada execução da função *EscreveString()* a mensagem é deslocada no display.
```ruby
DisplaySeteSegmentos Valor_1 = IniDisplay(3,1,2,3);

while(1){
   EscreveString(&Valor_1, "Delpitec");   // Escreve Delpitec de forma dinâmica nos 3 displays de 7 segmentos a cada execução da função
   HAL_Delay(300); // Desloca a msg no display a cada 300ms
}
```
&nbsp;<br><br>

**Possíveis melhorias:**
&nbsp;<br>
🎯 Possibilitar a escolha de display catodo comum ou anodo comum

 &nbsp;<br><br> 

## Contact me:
💼[LinkedIn](https://br.linkedin.com/in/rafaeldelpino)&nbsp;&nbsp;&nbsp;
📹[Youtube](https://www.youtube.com/delpitec)&nbsp;&nbsp;&nbsp;
📸[Instagram](https://www.instagram.com/delpitec_/)&nbsp;&nbsp;&nbsp;
📧[E-mail](delpitec@gmail.com)&nbsp;&nbsp;&nbsp;
