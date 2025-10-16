# Extensor de Sinal 4-8 bits

Este circuito implementa um extensor de sinal que converte números de 4 bits em números de 8 bits, preservando o valor numérico tanto para números positivos quanto negativos. O extensor replica o bit de sinal (bit mais significativo) nos 4 bits superiores adicionados, permitindo representação correta em complemento de 2.

<p align="center">
  <img src="./Imagens/ExtensorSinal.png" alt="Extensor de Sinal"><br>
  <a href="./11-Extensor.circ">Link do Extensor</a>
</p>

**Aplicações comuns:** Processadores (expansão de dados de diferentes tamanhos), conversores de largura de dados, interfaces entre módulos de diferentes resoluções, sistemas que precisam manter compatibilidade entre operandos de tamanhos diferentes.

---

## 1. Objetivo do Circuito

Dado um número de 4 bits na entrada, o circuito produz um número de 8 bits que representa o mesmo valor numérico. Para números positivos (bit mais significativo = 0), os 4 bits superiores são preenchidos com 0. Para números negativos (bit mais significativo = 1), os 4 bits superiores são preenchidos com 1, mantendo a representação em complemento de 2.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **Entrada 4 bits:** número a ser expandido (pode ser positivo ou negativo em complemento de 2).

**Saídas:**
- **Saída 8 bits:** número expandido mantendo o mesmo valor numérico.

### 2.2 Componentes Principais

- Splitters para separar e recombinar bits.
- Conexões diretas que replicam o bit de sinal.
- Multiplexação implícita através da estrutura de fios.

### **Funções dos componentes:**
- O splitter de entrada separa os 4 bits individuais da entrada, permitindo acesso ao bit de sinal (bit 3, o mais significativo).
- Os 4 bits inferiores da saída são conectados diretamente aos 4 bits de entrada, preservando seu valor original.
- Os 4 bits superiores da saída são todos conectados ao bit de sinal da entrada, replicando-o para manter a representação correta em complemento de 2.
- O splitter de saída combina os 8 bits (4 replicados + 4 originais) em um único barramento de 8 bits.

---

## 3. Funcionamento do Circuito

### 3.1 Operação Básica

**A) Separação da entrada:**  
O splitter divide a entrada de 4 bits em 4 sinais individuais: bit0 (menos significativo), bit1, bit2 e bit3 (bit de sinal, mais significativo).

**B) Replicação do bit de sinal:**  
O bit3 (bit de sinal) é conectado às posições bit4, bit5, bit6 e bit7 da saída, criando uma extensão que preserva o sinal do número.

**C) Preservação dos bits originais:**  
Os bits0, bit1, bit2 e bit3 da entrada são conectados diretamente às posições correspondentes (bit0 a bit3) da saída de 8 bits.

**D) Combinação da saída:**  
O splitter de saída combina os 8 bits em um único sinal de 8 bits que representa o número expandido.

### 3.2 Propagação dos sinais

**Exemplo com número positivo (7 em decimal):**
```
Entrada (4 bits):  0111 (7 em decimal)
Bit de sinal:      0 (número positivo)
Saída (8 bits):    00000111 (7 em decimal)
```

**Exemplo com número negativo (-5 em decimal):**
```
Entrada (4 bits):  1011 (-5 em complemento de 2)
Bit de sinal:      1 (número negativo)
Saída (8 bits):    11111011 (-5 em complemento de 2 de 8 bits)
```

### Fluxo das Operações:

| Entrada (4 bits) | Bit Sinal | Bits Superiores | Bits Inferiores | Saída (8 bits) | Decimal |
|------------------|-----------|-----------------|-----------------|----------------|---------|
| 0000             | 0         | 0000            | 0000            | 00000000       | 0       |
| 0111             | 0         | 0000            | 0111            | 00000111       | +7      |
| 1000             | 1         | 1111            | 1000            | 11111000       | -8      |
| 1111             | 1         | 1111            | 1111            | 11111111       | -1      |
| 0101             | 0         | 0000            | 0101            | 00000101       | +5      |

**Por que funciona:**
Em complemento de 2, o bit mais significativo representa o sinal: 0 para positivo, 1 para negativo. Ao expandir o número, replicar o bit de sinal nas posições superiores mantém a representação matemática correta. Isso é chamado de "extensão de sinal" (sign extension) e é fundamental para manter a integridade dos valores ao converter entre diferentes tamanhos de dados.