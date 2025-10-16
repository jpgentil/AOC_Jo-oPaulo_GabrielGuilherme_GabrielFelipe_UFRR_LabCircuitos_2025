# Memória ROM (Read-Only Memory)

Este circuito implementa uma memória ROM (Read-Only Memory) de 8 posições, cada uma armazenando 8 bits de dados. A ROM utiliza um decoder de 3 bits para selecionar qual das 8 posições será lida, e portas lógicas para codificar permanentemente os valores armazenados em cada endereço.

<p align="center">
  <img src="./Imagens/Memoria ROM (Read_only_memory).png" alt="Memória ROM"><br>
  <a href="./Circuitos_Logisim/Memoria_ROM(read_only_memory).circ">Link da Memória ROM</a>
</p>

**Aplicações comuns:** armazenamento de firmware, BIOS de computadores, tabelas de lookup, instruções de inicialização (bootloader), fontes de caracteres, e dados de configuração permanente.

---

## 1. Objetivo do Circuito

Dado um endereço de 3 bits na entrada (Key/decoder), o circuito seleciona e retorna o conteúdo de 8 bits armazenado permanentemente naquela posição da memória ROM, permitindo acesso rápido a dados pré-programados.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **Key (decoder):** barramento de 3 bits que determina qual endereço será acessado (0 a 7).

**Saídas:**
- **Saída:** barramento de 8 bits contendo os dados armazenados no endereço selecionado.

### 2.2 Componentes Principais

- 1 Splitter de entrada (3 bits para 3 linhas individuais).
- 8 portas AND de 3 entradas (decoder - uma para cada endereço).
- 8 portas OR de 4 entradas (uma para cada bit de saída).
- 1 Splitter de saída (8 linhas para barramento de 8 bits).
- Inversores integrados nas portas AND (para decodificação).

### **Funções dos componentes:**
- O Splitter de entrada separa os 3 bits do endereço em linhas individuais.
- As 8 portas AND funcionam como decoder, ativando apenas uma linha por vez de acordo com o endereço.
- As portas OR combinam os valores de cada bit para formar a saída de 8 bits.
- O Splitter de saída reconstrói o barramento de dados.

---

## 3. Funcionamento do Circuito

### 3.1 Decodificação de Endereço

O circuito utiliza 8 portas AND de 3 entradas para decodificar o endereço de entrada. Cada porta AND representa uma posição de memória (0 a 7) e só é ativada quando o endereço corresponde à sua combinação específica:

| Endereço | Binário (A₂A₁A₀) | Porta AND Ativa | Condição |
|:--------:|:----------------:|:---------------:|:--------:|
| 0        | 000              | AND₀            | ¬A₂ AND ¬A₁ AND ¬A₀ |
| 1        | 001              | AND₁            | ¬A₂ AND ¬A₁ AND A₀ |
| 2        | 010              | AND₂            | ¬A₂ AND A₁ AND ¬A₀ |
| 3        | 011              | AND₃            | ¬A₂ AND A₁ AND A₀ |
| 4        | 100              | AND₄            | A₂ AND ¬A₁ AND ¬A₀ |
| 5        | 101              | AND₅            | A₂ AND ¬A₁ AND A₀ |
| 6        | 110              | AND₆            | A₂ AND A₁ AND ¬A₀ |
| 7        | 111              | AND₇            | A₂ AND A₁ AND A₀ |

### 3.2 Codificação dos Dados

Cada bit de saída (bit 0 a bit 7) possui uma porta OR que recebe sinais das portas AND correspondentes aos endereços onde aquele bit deve ser 1. A programação da ROM é feita conectando ou não as saídas das portas AND às entradas das portas OR.

**Exemplo de codificação:**
- Se o bit 0 deve ser 1 nos endereços 1, 3, 5 e 7, a porta OR do bit 0 recebe conexões das portas AND₁, AND₃, AND₅ e AND₇.
- Se o bit 7 deve ser 1 apenas no endereço 7, a porta OR do bit 7 recebe conexão apenas da porta AND₇.

### 3.3 Processo de Leitura

**A) Aplicação do endereço:**  
Os 3 bits do endereço são aplicados na entrada Key e separados pelo Splitter.

**B) Decodificação:**  
As 8 portas AND avaliam simultaneamente suas condições. Apenas uma delas terá saída 1 (correspondente ao endereço fornecido).

**C) Seleção dos dados:**  
A porta AND ativa envia sinal 1 para as portas OR dos bits que devem estar em 1 naquele endereço.

**D) Composição da saída:**  
Cada porta OR determina o valor de seu respectivo bit (0 ou 1) baseado nas conexões programadas.

**E) Saída final:**  
O Splitter de saída reconstrói o barramento de 8 bits com os dados lidos.

---

## 4. Exemplo Prático

### Exemplo: Leitura do Endereço 3 (011)

**Configuração hipotética da ROM no endereço 3:** 10101100

**Processo:**

1. **Entrada:** Key = 011 (A₂=0, A₁=1, A₀=1)
2. **Decodificação:** Apenas AND₃ é ativado (¬0 AND 1 AND 1 = 1)
3. **Bits programados:**
   - Bit 7: AND₃ conectado → OR₇ = 1
   - Bit 6: AND₃ não conectado → OR₆ = 0
   - Bit 5: AND₃ conectado → OR₅ = 1
   - Bit 4: AND₃ não conectado → OR₄ = 0
   - Bit 3: AND₃ conectado → OR₃ = 1
   - Bit 2: AND₃ conectado → OR₂ = 1
   - Bit 1: AND₃ não conectado → OR₁ = 0
   - Bit 0: AND₃ não conectado → OR₀ = 0

**Saída:** 10101100

---

## 5. Características da Memória ROM

### 5.1 Vantagens
- **Não-volátil:** os dados permanecem armazenados mesmo sem energia elétrica.
- **Rapidez:** acesso instantâneo através de lógica combinacional, sem necessidade de clock.
- **Confiabilidade:** dados não podem ser corrompidos acidentalmente durante operação normal.
- **Simplicidade:** estrutura puramente combinacional, sem elementos de memória sequencial.

### 5.2 Desvantagens
- **Imutabilidade:** os dados são fixos durante a fabricação e não podem ser modificados.
- **Desperdício de espaço:** posições não utilizadas ainda ocupam recursos do circuito.
- **Escalabilidade limitada:** aumentar a capacidade requer crescimento exponencial de componentes.

### 5.3 Comparação com Outras Memórias

| Tipo | Escrita | Volatilidade | Velocidade | Custo |
|:----:|:-------:|:------------:|:----------:|:-----:|
| **ROM** | Não (fabricação) | Não-volátil | Rápida | Baixo |
| **RAM** | Sim | Volátil | Muito rápida | Médio |
| **EEPROM** | Sim (limitado) | Não-volátil | Média | Alto |
| **Flash** | Sim (blocos) | Não-volátil | Rápida | Médio |

---

## 6. Aplicações Práticas

### 6.1 BIOS de Computadores
A ROM armazena as instruções de inicialização (POST - Power-On Self-Test) que são executadas quando o computador é ligado.

### 6.2 Tabelas de Lookup
Armazenamento de valores pré-calculados para funções matemáticas complexas (seno, cosseno, logaritmos), acelerando cálculos.

### 6.3 Firmware
Programas embutidos em dispositivos eletrônicos (calculadoras, microcontroladores, aparelhos domésticos) que controlam suas funções básicas.

### 6.4 Fontes de Caracteres
Armazenamento dos padrões de pixels que formam letras e símbolos em displays e impressoras.

### 6.5 Decoders e Conversores
Implementação de tabelas de conversão (BCD para 7-segmentos, ASCII para códigos de controle).
