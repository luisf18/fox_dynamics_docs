# ESCs brushed duplos

*ESCs para dois motores escovados, multiprotocolo, configuráveis e com música.*

## Características Técnicas

| Modelo | Tensão | Bateria | Corrente | BEC |
|----|-----|----|----|----|
| ESC Brushed 2 x 1.5A  | 6V até 10V | 2S | até 1.5A por motor | 5V 500mA |
| ESC Brushed 2 x 3A    | 6V até 15V | 2 a 3S | até 3A por motor | 5V 200mA |
| ESC Brushed 2 x 5A    | 6V até 14V | 2 a 3S | até 5A por motor | 5V 500mA |

## ESC 2x5A Brushed

![esc_2x5](esc_2x5.png)

## ESC 2x3A Brushed

![esc_2x3](esc_2x3.png)

## ESC 2x1.5A Brushed

![esc_2x1p5](esc_2x1p5.png)

## Protocolos e compatibilidade

- Sinal PWM de servo
- PPM
- IBUS
- FoxWire
- UART (incluindo módulos Bluetooth)

## Sinal PWM de servo

**Canal 1:** Sinal de servo PWM 1  
**Canal 2:** Sinal de servo PWM 2  

![servo](servo.jpeg)

## Sinal IBUS

**Canal 1:** Sinal IBUS  
**Canal 2:** Telemetria UART TX com baudrate de 115200 (se não quiser não precisa usar)  

![ibus](IBUS.jpeg)

## Sinal PPM

**Canal 1:** Sinal PPM  
**Canal 2:** Telemetria UART TX com baudrate de 115200 (se não quiser não precisa usar)  

<!--![ppm](sinal_ppm.png)-->

## Controle via UART

**Baudrate:** 115200  
**Canal 1:** RX / TX  
**Canal 2:** TX (Telemetria ou respostas, se não quiser não precisa usar)  

![arduino_1fio](arduino_1fio.png)

## Com HC-05 e outros módulos Bluetooth

**Baudrate:** 115200  
**Canal 1:** RX / TX  
**Canal 2:** TX (Telemetria ou respostas, se não quiser não precisa usar)  

![bluetooth](bluetooth.jpeg)

## Comandos shell

O modo shell permite ler e alterar parâmetros e controlar a placa via comandos de texto enviados por UART com baudrate de 115200. Ideal para integração com projetos usando Arduino, ESP32 ou Raspberry Pi.  

**Funcionamento:** Cada vez que um comando de movimento é recebido, o ESC executa o movimento e reinicia o contador de tempo do mecanismo de failsafe. Caso o tempo determinado pelo parâmetro "uart_failsafe" seja ultrapassado, o ESC desliga os motores.

**uart_failsafe:** O parâmetro "uart_failsafe" é diferente do failsafe geral usado pelos outros protocolos. Ele pode ser ajustado de 0 a 2000 ms. Caso seja igual a zero, o mecanismo é desativado, ou seja, os motores não serão desligados caso o ESC pare de receber comandos.

**Sintaxe:** Existem duas formas de usar o modo UART.  
- **Acionando o modo Shell:** toda vez que reiniciar a placa, para iniciar a comunicação, envie "FOX-SHELL". O ESC deve responder com "FOX-SHELL-INIT", indicando que o modo Shell foi iniciado. Depois, basta enviar qualquer comando da tabela.
- **Modo Bluetooth (por prefixo)**: envie "FX-" seguido pelo comando, por exemplo: "FX-pwm -2000 2000" ou "FX-stop". Dessa forma, dispensa-se a inicialização com "FOX-SHELL". No entanto, é necessário que a opção Bluetooth esteja habilitada no campo Protocolos da configuração. Existe ainda a variação "FX0-...". O 0 indica que a resposta ao comando deve ser enviada pelo mesmo fio, enquanto com "FX-" a resposta é enviada pelo outro pino de canal.

|Comandos| uart_failsafe | Função |
|---|--|--|
| FOX-SHELL | não muda | Inicia a comunicação em modo shell |
| move `v1` `v2` `t` | não muda | Movimenta os motores com PWM `v1` e `v2` (de -2000 a 2000) durante `t` milissegundos depois para os motores. |
| move_ch `ch1` `ch2` `t` | não muda | Movimenta os motores usando `ch1` e `ch2` (de 1000 a 2000) como entradas de canais durante `t` milissegundos depois para os motores. |
| off | reinicia | Desliga os motores |
| stop | reinicia | Freia os motores |
| fall | encerra | Encerra a conexão |
| ch `ch1` `ch2` ... | reinicia | Envia o valor de cada canal |
| pwm `v1` `v2` | reinicia | Envia o valor PWMde cada motor de -2000 a 2000 |
| uart_failsafe `t` | configura | Lê ou escreve o valor do failsafe da UART de 0 a 2000 ms |
| sound `n` | não muda | Toca a música de índice `n`<BR>**0:** Init<BR>**1:** Connect<BR>**2:** Disconnect<BR>**3:** Beacon<BR>**4:** Extra |
| beep `f_Hz` `count` `periodo` `motor` | não muda | Toca um beep de frequência (`f_Hz`) durante `periodo` milissegundos depois desliga durante o mesmo intervalo e repete isso `count` vezes no `motor` (se quiser tocar os dois deixe sem esse parâmetro).<BR>**Exemplo:** "pi... pi... pi..." seria: *beep 2000 3 200*  |
| load | não muda | Carrega as configurações. Use após alterá-las ou reinicie o ESC. |
| uart1 `texto` | não muda | Imprime `texto` no pino de telemetria se ele estiver disponível. |
| [*comandos padrão*](../../communication/Shell) | - | Mais informações em [Shell](../../communication/Shell) |


## Configuração

### Parâmetros gerais

| Parâmetros | Descrição |
|--|--|
|**Addr**| Endereço no protocolo FoxWire (de 0 a 31) |
|**Name**| Nome do dispositivo (até 16 caracteres) |
| **Basic config.** | - **Mix:** mixar os canais  <br> - **swap motors:** Troca as entradas dos motores<br> - **Invert M1** inverte o sentido de M1<br> - **Invert M2** inverte o sentido de M2<br> - **M1 Break on stop** Aciona freio de M1<br> - **M2 Break on stop** Aciona freio de M2<br> - **Enable uart log** Aciona log no pino de canal ocioso<br> - **300ms uart log** Limita a 300 ms o intervalo entre logs (recomendado) |
|**Protocolos** | Habilita os protocolos: IBUS, PPM e "Bluetooth" (comandos UART com "FX-" ou "FX0-") |
| **Failsafe delay** | Tempo de failsafe em milissegundos |
| **UART Failsafe** | Tempo de failsafe em milissegundos dos comandos de movimento via UART |
|**Sound Volume**| Volume durante a execução de músicas.<BR>**[Atenção]:** Se colocar um valor muito grande, o motor pode acabar girando ao tocar a música.<BR> - **Sound On**: aciona sons<BR> - **Automatic volume compensation**: Compensação automática do volume em função da frequência  |
|**Sounds config.**| - **Enable start sound**: Habilita música ao ligar o ESC<BR> - **Enable connect sound**: Habilita música ao conectar<BR> - **Enable disconnect sound**: Habilita música ao perder conexão<BR> - **Use default sounds**: Caso habilitado usa os sons padrão; Caso contrário, usa os sons inseridos pelo usuário |
|**Multi Channels**| Nos protocolos que fornecem os valores de vários canais em um único fio, como IBUS, PPM ou UART, o ESC utiliza esta tabela para definir a função de cada canal. Caso um canal não seja utilizado, selecione "--".<BR> - **In1/rotation** entrada 1 ou velocidade de rotação quando está mixado.<BR> - **In2/speed** entrada 2 ou velocidade linear quando está mixado.<BR> - **Flip** canal para inverter o sentido de IN2: útil para caso o robô esteja de cabeça para baixo<BR> - **Max speed** canal que controla a velocidade máxima dos motores<BR> - **Sound** Canal para colocar um switch, quando acionado começa a tocar uma música definida pelo usuário.<BR> - **Move1** Quando acionado o ESC executa o movimento 1 programado pelo usuário.<BR> - **Move2** Quando acionado o ESC executa o movimento 2 programado pelo usuário. |

![config1](config.png)

### Configurações de cada motor

| Parâmetros | Descrição |
|--|--|
|**Config. Flags**|  - **Bidirectional** se o motor é bidirecional ou não.<BR> - **Invert** Inverte o sentido do motor.<BR> - **Set Min. Power** habilita a limitação de potência mínima nos motores usando o parâmetro **Min. Power**<BR> - **Set Min. Power** habilita a limitação de potência máxima nos motores usando o parâmetro **Max. power**<BR> |
|**Min. power**| Valor mínimo de PWM que o motor irá receber. De 0 a 100%.<BR>Recomendado para remover a zona morta onde um PWM muito baixo não é suficiente para mover o motor. |
|**Max. power**| Valor máximo de PWM que o motor irá receber. De 0 a 100%. |
|**Ch min**| Valor mínimo do canal de entrada. |
|**Ch center**| Valor central do canal de entrada. Nele o motor está parado ou em freio. |
|**Ch max**| Valor máximo do canal de entrada. |
|**Ch deadband**| Zona de tolerância em torno do centro e dos limites do canal de entrada.<BR>Por exemplo: para o motor começar a se mover, o canal de entrada precisa ser maior que **Ch center** + **Ch deadband**. |

![config2](config_motor.png)

## Configurando música de início

Configure ou consulte a **música de início do ESC** usando a ferramenta de [upload de músicas](https://luisf18.github.io/FoxLink_web_tool/rtttl.html), que permite utilizar melodias no formato **RTTTL**.

**Passo 1:** Conecte a placa com a ferramenta.  

![music1](music_connect.png)

**Passo 2:** Clique em escanear e selecione o dispositivo.  
**Passo 3:** Toque no computador ou no dispositivo ou salve músicas em RTTTL.  

![music3](music_esc.png)  