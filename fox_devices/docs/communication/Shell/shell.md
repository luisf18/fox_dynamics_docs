# Shell

Os comandos shell utilizam comandos de texto legiveis por humanos que podem ser facilmente digitados. A lista de comandos depende de cada dispositivo. No entanto, existem comandos comuns a todos.

## Inicialização do Shell

Digite `FOX-SHELL` para iniciar o shell. Em seguida digite os comandos que desejar.

### Inicialização condicinal

Como o shell é executado em um linha que podem possuir diversos dispositivos ligados em paralelo, é preciso especificar com qual dispositivo deseja se comunicar. O comando `FOX-SHELL` inicia todos na linha, se houver mais de um dispositivo isso irá causar erros. A `inicialização condiciona` é um recurso que ajuda a distinguir o dispositivo destino. A seguir os comando de inicialização e suas diferenças.

* `FOX-SHELL` inicia todos os dispositivos em modo shell (só pode ser feito quando só tem um dispositivo!)
* `FOX-SHELL-<ADDR>`  inicia o dispositivo com o endereço correspondente
* `FOX-SHELL-IF` inicia apenas o dispositivo que atende a uma condição especifica. Por exemplo, no sensor FXS50, a condição é ter um obstaculo na frente. Portanto, para acionar apenas um dispositivo com endereço indefinido coloque um obstaculo na frente de um unico sensor e execute este comando.
* `FOX-SHELL-UNLOCK` Os dispositivos na linha que não são iniciados entram em modo de espera. Para iniciar comunicação posteriormente com outro dispositivo é preciso sair e executar esse comando.

## Comandos comuns do Shell

* `help` lista os comandos disponiveis
* `exit` encerra o shell
* `register` lê ou escreve um registrador de 8bits
* `dump` lista as principais configurações
* `vcc` mede a tensão aplicada no microcontrolador

## Exemplo 1: Iniciando Shell em uma rede com 1 dispositivo

basta executar apenas o comando `FOX-SHELL` 

## Exemplo 2:

Iniciando Shell no dispositivo 0, saindo e depois iniciando o 1 em uma rede com 2 ou mais dispositivos:

1. `FOX-SHELL-0` (inicia 0 e bloqueia os outros)
2. "comandos..."
3. `exit` (encerra 0)
4. `FOX-SHELL-UNLOCK` (desbloqueia todos)
5. `FOX-SHELL-1` (inicia 1 e bloqueia os outros)
6. "comandos..."
7. `exit`