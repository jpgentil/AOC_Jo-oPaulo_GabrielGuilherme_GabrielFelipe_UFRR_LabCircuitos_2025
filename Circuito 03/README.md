# Porta XOR (Exclusive OR)

Este circuito implementa uma porta lógica XOR (OU Exclusivo) utilizando portas lógicas básicas (AND, OR e NOT). A porta XOR retorna 1 (verdadeiro) apenas quando as duas entradas têm valores diferentes, e retorna 0 quando ambas são iguais.

<p align="center">
  <img src="./Imagens/PortaXOR.png" alt="Porta XOR"><br>
  <a href="./Circuitos_Logisim/Porta XOR.circ">Link da Porta XOR</a>
</p>

**Aplicações comuns:** somadores binários (half-adder e full-adder), circuitos de paridade, detecção de diferenças, criptografia, e comparadores digitais.

---

## 1. Objetivo do Circuito

Dado duas entradas binárias (A e B), o circuito produz uma saída (C) que é 1 apenas quando as entradas são diferentes entre si, implementando a operação lógica de OU Exclusivo.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **A:** primeira entrada binária.
- **B:** segunda entrada binária.

**Saídas:**
- **C:** saída da porta XOR, resultado da operação A ⊕ B.

### 2.2 Componentes Principais

- 2 portas AND de 2 entradas cada (uma com entrada invertida).
- 1 porta OR de 2 entradas.
- Inversores integrados nas portas AND (representados por círculos nas entradas).

### **Funções dos componentes:**
- A primeira porta AND implementa a condição ¬A AND B (quando A é 0 e B é 1).
- A segunda porta AND implementa a condição A AND ¬B (quando A é 1 e B é 0).
- A porta OR combina ambas as condições, produzindo saída 1 quando qualquer uma delas é verdadeira.

---

## 3. Funcionamento do Circuito

### 3.1 Operação Básica

**A) Análise das entradas:**  
O circuito avalia simultaneamente duas condições mutuamente exclusivas através das portas AND.

**B) Primeira condição (¬A AND B):**  
A primeira porta AND verifica se A = 0 e B = 1. Quando essa condição é satisfeita, sua saída é 1.

**C) Segunda condição (A AND ¬B):**  
A segunda porta AND verifica se A = 1 e B = 0. Quando essa condição é satisfeita, sua saída é 1.

**D) Combinação final:**  
A porta OR recebe as saídas das duas portas AND. Se qualquer uma delas for 1 (ou seja, se as entradas forem diferentes), a saída C será 1.

### 3.2 Expressão Lógica

A porta XOR pode ser expressa algebricamente como:

**C = A ⊕ B = (¬A AND B) OR (A AND ¬B)**

Ou de forma equivalente:

**C = (A OR B) AND ¬(A AND B)**

O circuito implementado utiliza a primeira forma, que é mais direta e intuitiva.

### 3.3 Tabela Verdade

| A | B | ¬A | ¬B | ¬A AND B | A AND ¬B | C (Saída) |
|:-:|:-:|:--:|:--:|:--------:|:--------:|:---------:|
| 0 | 0 | 1  | 1  | 0        | 0        | **0**     |
| 0 | 1 | 1  | 0  | 1        | 0        | **1**     |
| 1 | 0 | 0  | 1  | 0        | 1        | **1**     |
| 1 | 1 | 0  | 0  | 0        | 0        | **0**     |

### 3.4 Fluxo dos Sinais

1. As entradas A e B são aplicadas simultaneamente ao circuito.
2. A primeira porta AND (com entrada A invertida) avalia se ¬A AND B é verdadeiro.
3. A segunda porta AND (com entrada B invertida) avalia se A AND ¬B é verdadeiro.
4. A porta OR combina os resultados: se pelo menos uma das condições for verdadeira, C = 1.
5. O resultado é que C = 1 apenas quando A ≠ B (valores diferentes).

---

## 4. Aplicações da Porta XOR

### 4.1 Somador Binário (Half-Adder)
A porta XOR é o componente central do half-adder, gerando o bit de soma de dois bits de entrada.

### 4.2 Detector de Paridade
Em sistemas de comunicação, a XOR verifica se o número de bits 1 em uma sequência é par ou ímpar.

### 4.3 Comparador
Detecta se dois bits são iguais (saída 0) ou diferentes (saída 1).

### 4.4 Criptografia
A operação XOR é fundamental em diversos algoritmos de criptografia simétrica, como a cifra de Vernam (One-Time Pad).

---

## 5. Comparativo com Outras Portas Lógicas

| Porta | Saída = 1 quando | Aplicação Principal |
|:-----:|:----------------:|:-------------------:|
| **OR** | Pelo menos uma entrada é 1 | Circuitos de seleção |
| **AND** | Todas as entradas são 1 | Circuitos de habilitação |
| **XOR** | Entradas são diferentes | Somadores e comparadores |
| **XNOR** | Entradas são iguais | Detectores de igualdade |
