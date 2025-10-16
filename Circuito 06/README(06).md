# Memória RAM (Random Access Memory)

Este circuito implementa uma memória RAM de 8 posições, cada uma armazenando 8 bits de dados. A RAM permite tanto operações de leitura quanto de escrita, utilizando um decoder (demultiplexer) para selecionar o endereço de escrita e um multiplexer para selecionar o endereço de leitura.

<p align="center">
  <img src="./Imagens/MemoriaRAM.png" alt="Memória RAM"><br>
  <a href="./Circuitos_Logisim/Memoria Ram.circ">Link da Memória RAM</a>
</p>

**Aplicações comuns:** armazenamento temporário de dados em processadores, memória principal de computadores, cache de dados, buffers de comunicação, e armazenamento de variáveis em programas.

---

## 1. Objetivo do Circuito

Dado um endereço de 3 bits (Key), o circuito permite armazenar dados de 8 bits em posições específicas (operação de escrita) ou recuperar dados previamente armazenados (operação de leitura), funcionando como uma memória volátil de acesso aleatório.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **Key:** barramento de 3 bits que determina qual endereço será acessado (0 a 7).
- **Entrada:** barramento de 8 bits contendo os dados a serem escritos na memória.
- **Clock:** sinal de sincronização para operações de escrita.
- **Escrever:** sinal de controle que habilita a operação de escrita quando ativado.
- **Read:** sinal de controle que habilita a operação de leitura quando ativado.
- **Reset:** sinal que limpa todos os registradores, resetando a memória.

**Saídas:**
- **Saída:** barramento de 8 bits contendo os dados lidos do endereço selecionado.

### 2.2 Componentes Principais

- 8 registradores de 8 bits (células de memória).
- 1 Demultiplexer (decoder para operações de escrita).
- 1 Multiplexer 8x1 de 8 bits (seletor para operações de leitura).
- 2 portas AND (para controle de escrita e leitura).
- Splitters (para manipulação de barramentos).

### **Funções dos componentes:**
- Cada registrador de 8 bits armazena uma palavra de dados em uma posição de memória.
- O Demultiplexer decodifica o endereço e envia o sinal de clock apenas para o registrador selecionado durante operações de escrita.
- O Multiplexer 8x1 seleciona qual registrador terá seus dados apresentados na saída durante operações de leitura.
- As portas AND controlam quando as operações de escrita e leitura podem ser executadas.

---

## 3. Funcionamento do Circuito

<p align="center">
  Representação de um Registrador de 8 bits no Logisim.<br>
  <img src="./Imagens/Registrador8bits.png" alt="Registrador 8 bits"><br>
  <a href="./Circuitos_Logisim/Memoria Ram.circ">Link do Registrador 8 bits</a>
</p>

### 3.1 Registrador de 8 Bits

O componente básico da RAM é o registrador de 8 bits, que consiste em:

**Entradas:**
- **Entrada:** barramento de 8 bits com os dados a serem armazenados.
- **Clock:** pulso de sincronização que ativa a escrita.
- **Reset:** sinal que zera todos os flip-flops.

**Saídas:**
- **Saída:** barramento de 8 bits com os dados armazenados.

**Estrutura interna:**
- 8 flip-flops D (um para cada bit) que armazenam os dados.
- Cada flip-flop captura seu respectivo bit de entrada na borda de subida do clock.

### 3.2 Operação de Escrita

**A) Configuração:**  
O sinal "Escrever" é ativado (nível lógico 1), permitindo que o clock seja propagado através da porta AND.

**B) Seleção de endereço:**  
Os 3 bits de Key são aplicados ao Demultiplexer, que decodifica o endereço e direciona o clock apenas para o registrador selecionado.

**C) Splitter de entrada:**  
Os 8 bits de dados são separados e distribuídos simultaneamente para todos os 8 registradores.

**D) Escrita síncrona:**  
Na borda de subida do clock, apenas o registrador selecionado captura os dados de entrada. Os outros registradores não recebem o clock e mantêm seus valores anteriores.

### 3.3 Operação de Leitura

**A) Configuração:**  
O sinal "Read" é ativado (nível lógico 1), habilitando o Multiplexer através da porta AND.

**B) Seleção de endereço:**  
Os 3 bits de Key são aplicados ao Multiplexer 8x1, que seleciona qual dos 8 registradores terá seus dados encaminhados para a saída.

**C) Leitura assíncrona:**  
Os dados do registrador selecionado são imediatamente apresentados na saída, sem necessidade de pulso de clock. A operação é puramente combinacional.

**D) Saída contínua:**  
Enquanto "Read" estiver ativo, a saída reflete o conteúdo do endereço selecionado em tempo real.

### 3.4 Tabela de Endereçamento

| Key (Endereço) | Binário | Registrador | Operação de Escrita | Operação de Leitura |
|:--------------:|:-------:|:-----------:|:-------------------:|:-------------------:|
| 0              | 000     | Reg 0       | Clock → Reg 0       | Saída ← Reg 0       |
| 1              | 001     | Reg 1       | Clock → Reg 1       | Saída ← Reg 1       |
| 2              | 010     | Reg 2       | Clock → Reg 2       | Saída ← Reg 2       |
| 3              | 011     | Reg 3       | Clock → Reg 3       | Saída ← Reg 3       |
| 4              | 100     | Reg 4       | Clock → Reg 4       | Saída ← Reg 4       |
| 5              | 101     | Reg 5       | Clock → Reg 5       | Saída ← Reg 5       |
| 6              | 110     | Reg 6       | Clock → Reg 6       | Saída ← Reg 6       |
| 7              | 111     | Reg 7       | Clock → Reg 7       | Saída ← Reg 7       |

---

## 4. Exemplos Práticos

### Exemplo 1: Escrita de Dados

**Objetivo:** Escrever o valor 11010110 (214₁₀) no endereço 3.

**Processo:**
1. **Configuração das entradas:**
   - Key = 011 (endereço 3)
   - Entrada = 11010110
   - Escrever = 1
   - Read = 0

2. **Decodificação:**
   - Demultiplexer direciona o clock para o Registrador 3

3. **Escrita:**
   - Na borda de subida do clock, Registrador 3 armazena 11010110
   - Registradores 0, 1, 2, 4, 5, 6, 7 mantêm seus valores anteriores

**Resultado:** Dados escritos com sucesso no endereço 3.

### Exemplo 2: Leitura de Dados

**Objetivo:** Ler o valor armazenado no endereço 3.

**Processo:**
1. **Configuração das entradas:**
   - Key = 011 (endereço 3)
   - Escrever = 0
   - Read = 1

2. **Seleção:**
   - Multiplexer 8x1 seleciona a saída do Registrador 3

3. **Leitura:**
   - Saída = 11010110 (valor previamente armazenado)

**Resultado:** Dados lidos com sucesso do endereço 3.

---

## 5. Características da Memória RAM

### 5.1 Vantagens
- **Acesso aleatório:** qualquer endereço pode ser acessado diretamente sem necessidade de percorrer endereços anteriores.
- **Leitura e escrita:** permite tanto armazenar quanto recuperar dados dinamicamente.
- **Velocidade:** operações de leitura são instantâneas (combinacionais); escritas são síncronas e rápidas.
- **Flexibilidade:** conteúdo pode ser modificado durante a operação do sistema.

### 5.2 Desvantagens
- **Volatilidade:** os dados são perdidos quando a energia é desligada.
- **Complexidade:** requer circuitos de controle adicionais (clock, sinais de read/write).
- **Consumo de energia:** flip-flops consomem mais energia que circuitos puramente combinacionais.

### 5.3 Diferenças entre RAM e ROM

| Característica | RAM | ROM |
|:--------------:|:---:|:---:|
| **Volatilidade** | Volátil | Não-volátil |
| **Escrita** | Sim (dinâmica) | Não (fabricação) |
| **Leitura** | Sim | Sim |
| **Velocidade de escrita** | Rápida | N/A |
| **Aplicação** | Dados temporários | Dados permanentes |
| **Elementos de memória** | Flip-flops | Portas lógicas |

---

## 6. Aplicações Práticas

### 6.1 Memória Principal de Computadores
A RAM armazena programas e dados em execução, permitindo ao processador acessar informações rapidamente.

### 6.2 Cache de Dados
Memórias RAM de alta velocidade armazenam cópias temporárias de dados frequentemente acessados, acelerando operações.

### 6.3 Buffers de Comunicação
Armazenamento temporário de dados em transmissão entre dispositivos com velocidades diferentes.

### 6.4 Pilha de Execução (Stack)
Armazenamento temporário de variáveis locais, endereços de retorno e contextos de execução em programas.

### 6.5 Registradores de Propósito Geral
Em processadores, conjuntos de registradores RAM armazenam operandos e resultados intermediários de operações aritméticas.

---

## 7. Sinais de Controle

### 7.1 Clock
Sincroniza todas as operações de escrita, garantindo que os dados sejam capturados no momento correto.

### 7.2 Escrever (Write Enable)
Habilita ou desabilita operações de escrita. Quando desativado, protege os dados contra modificações acidentais.

### 7.3 Read (Read Enable)
Habilita a operação de leitura, controlando quando os dados devem ser apresentados na saída.

### 7.4 Reset
Limpa todos os registradores simultaneamente, retornando a memória ao estado inicial (todos os bits zerados).

---

## 8. Expansão da Memória

Para aumentar a capacidade de armazenamento:

- **Mais endereços:** adicionar mais bits ao Key permite endereçar mais posições (4 bits = 16 posições, 5 bits = 32 posições).
- **Palavras maiores:** aumentar o número de bits por registrador permite armazenar dados maiores (16 bits, 32 bits, 64 bits).
- **Bancos de memória:** múltiplos módulos RAM podem ser organizados em bancos paralelos para aumentar a largura de banda.