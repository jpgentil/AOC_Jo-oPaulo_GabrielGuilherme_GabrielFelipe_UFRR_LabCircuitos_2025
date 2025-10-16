# Somador Completo de 8 bits

Este circuito implementa um somador binário completo de 8 bits capaz de adicionar dois números de 8 bits e um carry de entrada, produzindo uma soma de 8 bits e um carry de saída. O circuito utiliza 8 somadores completos de 1 bit conectados em cascata, onde o carry out de cada estágio alimenta o carry in do próximo, permitindo a propagação do carry através de todos os bits.

<p align="center">
  <img src="./Imagens/Somador8bits.png" alt="Somador 8 bits"><br>
  <a href="./Somador_Completo.circ">Link do Somador</a>
</p>

**Aplicações comuns:** CPUs (unidades aritméticas), calculadoras digitais, sistemas de processamento numérico, contadores, acumuladores, e qualquer circuito que necessite realizar adição binária.

---

## 1. Objetivo do Circuito

Dados dois números de 8 bits (Entrada 1 e Entrada 2) e um carry inicial (Cin = 0 para soma simples), o circuito calcula a soma binária completa, produzindo um resultado de 8 bits e um sinal de carry out (Cout) que indica se houve overflow. O circuito é fundamental para operações aritméticas em sistemas digitais, servindo como base para somadores de maior capacidade e para operações mais complexas como subtração e multiplicação.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **Entrada 1:** primeiro número de 8 bits a ser somado.
- **Entrada 2:** segundo número de 8 bits a ser somado.
- **Cin (implícito):** carry de entrada conectado ao ground (0) no primeiro somador.

**Saídas:**
- **Saída:** resultado da soma (8 bits).
- **Cout:** carry de saída que indica overflow (quando a soma excede 255).

### 2.2 Componentes Principais

- 8 somadores completos de 1 bit (full adders) conectados em cascata.
- Splitters para dividir e combinar os sinais de 8 bits em bits individuais.
- Subcircuito XOR implementado com portas AND e NOR para operações dentro do somador.

### **Funções dos componentes:**
- Cada somador completo de 1 bit adiciona dois bits (A e B) mais um carry de entrada (Cin), produzindo um bit de soma (S) e um carry de saída (Cout).
- Os splitters na entrada separam os 8 bits de cada número em sinais individuais que alimentam cada somador.
- O splitter na saída combina os 8 bits de soma em um único barramento de 8 bits.
- O carry out de cada somador é conectado ao carry in do somador seguinte, criando uma cadeia de propagação de carry da direita para a esquerda (do bit menos significativo para o mais significativo).

---

## 3. Funcionamento do Circuito

<p align="center">
  Somador Completo de 1 bit (Full Adder).<br>
  <img src="./Imagens/FullAdder.png" alt="Full Adder"><br>
  <a href="./somador_completo.circ">Link do Somador Completo</a>
</p>

### 3.1 Operação Básica

**A) Separação das entradas:**  
Os splitters dividem Entrada 1 e Entrada 2 em 8 bits individuais cada (bit0 a bit7), permitindo que cada par de bits seja processado por um somador completo dedicado.

**B) Soma bit a bit com propagação de carry:**  
Começando pelo bit menos significativo (bit0), cada somador completo calcula S = A ⊕ B ⊕ Cin e Cout = (A ∧ B) ∨ (Cin ∧ (A ⊕ B)). O carry out gerado é passado como carry in para o próximo somador.

**C) Propagação em cascata:**  
O carry se propaga sequencialmente através de todos os 8 somadores, do bit0 ao bit7. Se houver carry no último estágio (bit7), isso indica que a soma excedeu 8 bits (maior que 255).

**D) Combinação da saída:**  
Os 8 bits de soma (S0 a S7) são combinados pelo splitter de saída em um único valor de 8 bits que representa o resultado final.

### 3.2 Propagação dos sinais

**Equações do Somador Completo de 1 bit:**
- **S (Soma) = A ⊕ B ⊕ Cin:** OU-exclusivo triplo que gera o bit de soma.
- **Cout (Carry Out) = (A ∧ B) ∨ (Cin ∧ (A ⊕ B)):** gera carry quando pelo menos dois dos três bits de entrada são 1.

**Exemplo de propagação de carry:**
```
Entrada 1:  10011101 (157 decimal)
Entrada 2:  01100111 (103 decimal)
Cin:        00000000 (ground)

Bit 0: 1+1+0 = 0, Cout=1
Bit 1: 0+1+1 = 0, Cout=1
Bit 2: 1+1+1 = 1, Cout=1
Bit 3: 1+0+1 = 0, Cout=1
Bit 4: 1+0+1 = 0, Cout=1
Bit 5: 0+1+1 = 0, Cout=1
Bit 6: 0+1+1 = 0, Cout=1
Bit 7: 1+0+1 = 0, Cout=1

Resultado:  00000100 (4 decimal, com Cout=1 indicando 260 = 256+4)
```

### Fluxo das Operações:

| Entrada 1 | Entrada 2 | Cin | Resultado | Cout | Explicação |
|-----------|-----------|-----|-----------|------|------------|
| 00000101 (5) | 00000011 (3) | 0 | 00001000 (8) | 0 | Soma simples sem overflow |
| 11111111 (255) | 00000001 (1) | 0 | 00000000 (0) | 1 | Overflow: 256 mod 256 = 0 |
| 10101010 (170) | 01010101 (85) | 0 | 11111111 (255) | 0 | Soma máxima sem overflow |
| 11111111 (255) | 11111111 (255) | 0 | 11111110 (254) | 1 | 510 mod 256 = 254 |
| 00001111 (15) | 00001111 (15) | 0 | 00011110 (30) | 0 | Carry propagado em 4 bits |

**Tempo de propagação:**
O carry deve se propagar sequencialmente através de todos os 8 somadores. No pior caso (quando todos os bits geram carry), o atraso total é proporcional ao número de bits, sendo este um somador do tipo "ripple carry" (carry propagado). Esse é o tipo mais simples, mas existem arquiteturas mais rápidas como "carry lookahead" para aplicações que exigem maior velocidade.