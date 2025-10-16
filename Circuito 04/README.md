# Somador de 8 bits + 4

Este circuito implementa um somador binário de 8 bits que adiciona a constante 4 a qualquer valor de entrada. O circuito utiliza somadores de 1 bit encadeados (ripple-carry adder) para realizar a operação aritmética, propagando o carry (vai-um) através de cada estágio.

<p align="center">
  <img src="./Imagens/Somador8bits+4.png" alt="Somador 8 bits + 4"><br>
  <a href="./Circuito_Logisim/Somador 8 bits(int) +4.circ">Link do Somador 8 bits + 4</a>
</p>

**Aplicações comuns:** incrementadores em contadores de programa (PC), cálculo de endereços sequenciais em memória, unidades aritméticas e lógicas (ALU), e operações de deslocamento aritmético.

---

## 1. Objetivo do Circuito

Dado um número binário de 8 bits na entrada, o circuito adiciona o valor 4 (representado como 00000100 em binário) e produz o resultado de 8 bits na saída, tratando automaticamente o overflow através do carry propagado.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **Input:** barramento de 8 bits contendo o valor a ser incrementado.

**Saídas:**
- **Output:** barramento de 8 bits contendo o resultado da soma (Input + 4).

### 2.2 Componentes Principais

- 8 somadores de 1 bit (Full Adders) interconectados.
- 2 Splitters (um para separar os bits de entrada e outro para reunir os bits de saída).
- 1 porta NOT (para inverter o bit 2, implementando o valor binário 4).
- 1 Ground (terra lógico, representando 0).

### **Funções dos componentes:**
- Cada somador de 1 bit realiza a adição de três entradas: um bit do número de entrada, um bit da constante 4, e o carry do estágio anterior.
- O Splitter de entrada divide o barramento de 8 bits em linhas individuais para processamento.
- O Splitter de saída reconstrói o resultado em um barramento de 8 bits.
- A porta NOT e o Ground configuram os bits da constante 4 (00000100).

---

## 3. Funcionamento do Circuito

<p align="center">
  Representação de um Somador de 1 bit (Full Adder) no Logisim.<br>
  <img src="./Imagens/Somador1bit.png" alt="Somador 1 bit"><br>
  <a href="./Circuito_Logisim/Somador 8 bits(int) +4.circ">Link do Somador 1 bit</a>
</p>

### 3.1 Somador de 1 Bit (Full Adder)

O componente básico do circuito é o somador de 1 bit, que possui:

**Entradas:**
- **A:** primeiro bit a ser somado.
- **B:** segundo bit a ser somado.
- **CarryIN:** carry (vai-um) proveniente do estágio anterior.

**Saídas:**
- **S:** bit de soma resultante.
- **CarryOUT:** carry (vai-um) para o próximo estágio.

**Lógica interna:**
- **S = A ⊕ B ⊕ CarryIN** (soma usando portas XOR)
- **CarryOUT = (A AND B) OR (A AND CarryIN) OR (B AND CarryIN)** (detecção de carry)

### 3.2 Operação em Cascata

O circuito conecta 8 somadores de 1 bit em série, onde:

1. **Bit 0 (LSB):** Recebe o bit menos significativo da entrada e 0 (da constante 4). Não há carry de entrada inicial.
2. **Bit 1:** Recebe o segundo bit da entrada e 0 (da constante 4), mais o carry do Bit 0.
3. **Bit 2:** Recebe o terceiro bit da entrada e 1 (através da porta NOT, representando o bit 2 de 4 = 00000100), mais o carry do Bit 1.
4. **Bits 3-7:** Recebem os bits restantes da entrada e 0 (da constante 4), propagando o carry sequencialmente.

### 3.3 Configuração da Constante 4

O valor 4 em binário de 8 bits é **00000100**. Isso significa:
- Bits 0, 1, 3, 4, 5, 6, 7 = 0 (conectados ao Ground)
- Bit 2 = 1 (obtido através da inversão NOT de um sinal 0)

### 3.4 Fluxo das Operações

| Estágio | Bit Entrada | Bit Constante (4) | CarryIN | Soma (S) | CarryOUT |
|:-------:|:-----------:|:-----------------:|:-------:|:--------:|:--------:|
| Bit 0   | A₀          | 0                 | 0       | A₀       | 0        |
| Bit 1   | A₁          | 0                 | C₀      | A₁ ⊕ C₀  | C₁       |
| Bit 2   | A₂          | 1                 | C₁      | A₂ ⊕ 1 ⊕ C₁ | C₂    |
| Bit 3   | A₃          | 0                 | C₂      | A₃ ⊕ C₂  | C₃       |
| Bit 4   | A₄          | 0                 | C₃      | A₄ ⊕ C₃  | C₄       |
| Bit 5   | A₅          | 0                 | C₄      | A₅ ⊕ C₄  | C₅       |
| Bit 6   | A₆          | 0                 | C₅      | A₆ ⊕ C₅  | C₆       |
| Bit 7   | A₇          | 0                 | C₆      | A₇ ⊕ C₆  | C₇       |

**Nota:** C representa o carry propagado entre os estágios.

---

## 4. Exemplo Prático

### Exemplo: Somar 4 ao valor 00001010 (10 em decimal)

**Entrada:** 00001010 (10₁₀)  
**Constante:** 00000100 (4₁₀)

**Processo bit a bit:**

| Bit | Entrada | Constante | CarryIN | Soma | CarryOUT |
|:---:|:-------:|:---------:|:-------:|:----:|:--------:|
| 0   | 0       | 0         | 0       | 0    | 0        |
| 1   | 1       | 0         | 0       | 1    | 0        |
| 2   | 0       | 1         | 0       | 1    | 0        |
| 3   | 1       | 0         | 0       | 1    | 0        |
| 4   | 0       | 0         | 0       | 0    | 0        |
| 5   | 0       | 0         | 0       | 0    | 0        |
| 6   | 0       | 0         | 0       | 0    | 0        |
| 7   | 0       | 0         | 0       | 0    | 0        |

**Saída:** 00001110 (14₁₀)  
**Verificação:** 10 + 4 = 14 ✓

---

## 5. Características do Ripple-Carry Adder

### 5.1 Vantagens
- **Simplicidade:** estrutura simples e repetitiva, fácil de implementar e entender.
- **Economia de componentes:** requer apenas somadores de 1 bit interconectados.
- **Escalabilidade:** pode ser facilmente expandido para qualquer número de bits.

### 5.2 Desvantagens
- **Propagação de carry:** o atraso aumenta proporcionalmente ao número de bits, pois cada estágio deve esperar o carry do anterior.
- **Velocidade limitada:** não é ideal para aplicações de alta velocidade devido ao atraso acumulado.

### 5.3 Tratamento de Overflow
Quando a soma excede o limite de 8 bits (255), ocorre overflow. O último carry (C₇) é descartado, resultando em um valor modulado por 256.

**Exemplo de overflow:**
- Entrada: 11111111 (255₁₀)
- Constante: 00000100 (4₁₀)
- Resultado: 00000011 (3₁₀) — overflow ocorreu, 259 mod 256 = 3

---

## 6. Aplicações Práticas

### 6.1 Contador de Programa (PC)
Em processadores, o PC é incrementado em 4 bytes após cada instrução para apontar para a próxima instrução na memória.

### 6.2 Endereçamento de Memória
Cálculo de endereços sequenciais em operações de leitura/escrita de blocos de dados.

### 6.3 ALU (Unidade Aritmética e Lógica)
Operações de incremento são fundamentais em loops, contadores e manipulação de índices.
