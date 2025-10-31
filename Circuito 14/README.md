# Detector de Paridade Ímpar

Este circuito implementa um detector de paridade ímpar usando portas lógicas XOR: dada uma entrada de 8 bits, ele verifica se o número de bits '1' na entrada é ímpar. A saída será '1' se houver quantidade ímpar de bits '1', e '0' se houver quantidade par.

<p align="center">
  <img src="./Imagens/14_Paridade-Impar.png" alt="Detector de Paridade Ímpar" width="550"><br>
  <a href="./Circuitos%20Logisim/14_Paridade-Impar.circ">Link do Circuito</a>
</p>

**Aplicações comuns:** Detecção de erros em transmissão de dados, verificação de integridade em sistemas de comunicação, protocolos de rede, sistemas de armazenamento digital e memórias.

---

## 1. Objetivo do Circuito

Dado um conjunto de bits (8 bits neste exemplo), o circuito determina se a quantidade total de bits '1' é ímpar. Esta verificação é fundamental para sistemas de detecção de erros, onde um bit adicional de paridade pode ser usado para identificar se houve alteração nos dados durante transmissão ou armazenamento.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **Barramento de 8 bits:** conjunto de valores binários (b₇, b₆, b₅, b₄, b₃, b₂, b₁, b₀) a serem analisados.

**Saídas:**
- **S (Saída de Paridade):** 
  - `1` quando há quantidade ímpar de bits '1' na entrada
  - `0` quando há quantidade par de bits '1' na entrada

### 2.2 Componentes Principais

- **Splitter:** separa o barramento de 8 bits em linhas individuais.
- **Portas XOR (7 unidades):** implementam a lógica de detecção de paridade.
- **Pino de saída:** exibe o resultado da verificação.

### **Funções dos componentes:**
- O **splitter** divide o barramento de entrada em 8 bits individuais para processamento.
- As **portas XOR** são conectadas em cascata (árvore) para calcular a paridade. A propriedade fundamental da porta XOR é que sua saída é '1' quando há quantidade ímpar de entradas em '1'.
- A configuração em árvore reduz a latência e otimiza o processamento paralelo.

---

## 3. Funcionamento do Circuito

<p align="center">
  <img src="./Imagens/14_Porta-XOR.png" alt="Porta XOR" width="200"><br>
  Representação de uma porta XOR
</p>

### 3.1 Operação Básica

**A) Propriedade fundamental da porta XOR:**
A porta XOR (ou exclusivo) possui uma característica essencial: sua saída é '1' quando há quantidade ímpar de entradas em '1'. Esta propriedade é a base para detecção de paridade.

**Tabela verdade da porta XOR (2 entradas):**

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 | 0       |
| 0 | 1 | 1       |
| 1 | 0 | 1       |
| 1 | 1 | 0       |

**B) Cascateamento de portas XOR:**
Quando múltiplas portas XOR são conectadas em cascata, a propriedade de paridade ímpar é preservada. O resultado final indica se o total de bits '1' na entrada original é ímpar.

**C) Processamento em árvore:**
As portas XOR são organizadas em níveis hierárquicos:
- **Nível 1:** 4 portas XOR processam os 8 bits de entrada em pares (b₀⊕b₁, b₂⊕b₃, b₄⊕b₅, b₆⊕b₇)
- **Nível 2:** 2 portas XOR processam os resultados do nível anterior
- **Nível 3:** 1 porta XOR final produz o resultado da paridade

### 3.2 Propagação dos Sinais

A verificação ocorre em três estágios:

**Estágio 1 (4 portas XOR paralelas):**
- XOR₁ = b₀ ⊕ b₁
- XOR₂ = b₂ ⊕ b₃
- XOR₃ = b₄ ⊕ b₅
- XOR₄ = b₆ ⊕ b₇

**Estágio 2 (2 portas XOR paralelas):**
- XOR₅ = XOR₁ ⊕ XOR₂
- XOR₆ = XOR₃ ⊕ XOR₄

**Estágio 3 (1 porta XOR final):**
- S = XOR₅ ⊕ XOR₆

### Exemplos de Funcionamento:

| Entrada (8 bits) | Qtd de '1's | Paridade | Saída S | Comentário |
|------------------|-------------|----------|---------|------------|
| 00000000         | 0           | Par      | 0       | Nenhum bit ativo |
| 00000001         | 1           | Ímpar    | 1       | Um único bit ativo |
| 00000011         | 2           | Par      | 0       | Dois bits ativos |
| 00000111         | 3           | Ímpar    | 1       | Três bits ativos |
| 01010101         | 4           | Par      | 0       | Padrão alternado |
| 01110111         | 6           | Par      | 0       | Seis bits ativos |
| 11111111         | 8           | Par      | 0       | Todos os bits ativos |
| 10101010         | 4           | Par      | 0       | Padrão alternado invertido |
| 11110001         | 5           | Ímpar    | 1       | Cinco bits ativos |
