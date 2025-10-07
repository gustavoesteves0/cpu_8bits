# CPU de 8 bits

## Introdução
Neste projeto, foi desenvolvida uma CPU simplificada de 8 bits utilizando o simulador **Digital**. O principal objetivo foi implementar uma **ULA (Unidade Lógica e Aritmética)** abrangente e integrar um **circuito de controle** robusto, capaz de orquestrar os ciclos de *fetch* e *execute*, conforme as especificações da atividade e o desafio extra. A arquitetura foi inspirada nos princípios de processadores elementares, como o SAP-I, para demonstrar o funcionamento sequencial de um processador.

## Funcionalidades da ULA
A ULA foi projetada para suportar as seguintes operações, com base nos requisitos da atividade:

* **Soma (8 bits):** Implementada para operações de adição de 8 bits.
* **Subtração (5 e 8 bits):** A subtração foi realizada utilizando a representação por **complemento de dois**, garantindo a correção para números negativos.
* **Multiplicação (operandos de 4 bits):** Um `array multiplier` foi desenvolvido especificamente para operar com operandos de 4 bits, resultando em saídas de 8 bits, como especificado.
* **Incremento (8 bits):** Circuito dedicado para incrementar um valor em 1.
* **Decremento (8 bits):** Circuito dedicado para decrementar um valor em 1.
* **Divisão (operandos de 4 bits):** A lógica de divisão foi implementada para operar com operandos de 4 bits.
* **Bit Shift (deslocamento à esquerda de 8 bits):** Realiza o deslocamento lógico de bits para a esquerda, sem *wrap-around*.
* **Comparação:** Circuito com entradas de dois valores de 8 bits e três saídas que indicam `A == B`, `A < B` e `A > B`.

Além das operações aritméticas e lógicas, o circuito de controle da CPU suporta as seguintes instruções:
* **LOAD** (carregamento de valor no acumulador).
* **Jump incondicional** (salto para um novo endereço de programa).
* **Jump condicional** (executa salto quando o acumulador é igual a 0, utilizando a **Flag Zero** da ULA).

Toda a CPU é sincronizada por um **sinal de clock unificado** e a saída principal é exibida em **dois displays de sete segmentos**, que mostram o valor armazenado no acumulador, suportando números positivos e negativos.

## Ciclos de Operação e Controle
A CPU opera em ciclos bem definidos, gerenciados por uma Unidade de Controle que incorpora um **Sequenciador** e um **Contador de Programa (PC)**:

* **Ciclo de Fetch:** Responsável por buscar as instruções da memória ROM. Nos tempos T1, T2 e T3 do sequenciador, o PC envia o endereço da instrução para o MAR (Memory Address Register), que então acessa a memória ROM, carregando a instrução no IR (Instruction Register). Simultaneamente, o PC é incrementado para apontar para a próxima instrução.
* **Ciclo de Execute:** Nesta fase, que ocorre nos tempos T4, T5 e T6, a instrução carregada no IR é decodificada. O opcode extraído da instrução e os sinais de tempo do sequenciador ativam os caminhos de dados e as operações da ULA ou as instruções de controle (LOAD, Jump), culminando na produção de um resultado ou na alteração do fluxo do programa.

## Estrutura de Arquivos
Durante o desenvolvimento, diversos circuitos auxiliares, organizados em arquivos `.dig`, foram criados para compor a CPU final:

* **5bitSubtractor.dig / 8bitSubtractor.dig:** Circuitos de subtração com lógica de complemento de dois.
* **8bitAdder.dig / adder5bit.dig / fullAdder.dig / halfAdder.dig:** Componentes fundamentais para operações de soma.
* **8bitdecrementor.dig / 8bitIncrementor.dig:** Circuitos dedicados para operações de incremento e decremento.
* **ControlUnit.dig / sequenciator.dig:** Implementação da unidade de controle e da lógica de sequenciamento de 6 tempos (T1 a T6).
* **cpu.dig / newCPU.dig:** Arquivos principais que contêm a implementação completa da CPU de 8 bits.
* **DataPath.dig:** Organização do caminho de dados e barramentos entre os principais componentes.
* **divideStep.dig / divisor.dig:** Lógica para a operação de divisão de operandos de 4 bits.
* **multiplier.dig:** Circuito de multiplicação de operandos de 4 bits.
* **memory.dig:** Implementação da memória ROM para armazenamento de instruções e dados.
* **PC.dig / newPC.dig / ResettableRegister.dig:** Contador de Programa com funcionalidades de auto-incremento e carregamento paralelo (`LOAD` para Jumps).
* **shifter_left_8bit.dig:** Deslocador lógico à esquerda de 8 bits.
* **ULA.dig:** Unidade Lógica e Aritmética principal, integrando todas as operações especificadas e o circuito de seleção por opcodes.
* **MANIFEST.TXT:** Arquivo auxiliar para organização do projeto.

## Demonstração em Vídeo
Para complementar a entrega, foi gravado um vídeo demonstrativo explicando o funcionamento da CPU e os principais desafios de implementação, que está [aqui.](https://www.youtube.com/watch?v=spU5cvcks4c)

## Conclusão
O desenvolvimento desta CPU de 8 bits proporcionou uma aplicação prática e aprofundada de conceitos fundamentais da arquitetura de computadores. A construção de componentes em nível lógico, como somadores, multiplicadores e divisores, juntamente com a estruturação dos ciclos de *fetch* e *execute*, permitiu consolidar o aprendizado em **operações aritméticas e lógicas, controle sequencial, gerenciamento de memória e barramentos** dentro de uma arquitetura funcional. A implementação do ciclo de busca, como desafio extra, demonstrou a capacidade de criar um sistema programável completo, reforçando a compreensão do fluxo de execução de um programa em nível de hardware.
