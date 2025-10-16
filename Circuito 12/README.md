# Máquina de Estados - Contador de 2 bits

Este circuito implementa uma máquina de estados finita que funciona como um contador binário de 2 bits: a cada pulso de clock, ele incrementa o valor em 1, passando pela sequência 00 → 01 → 10 → 11 → 00. O circuito utiliza flip-flops D e portas lógicas para criar transições de estado síncronas e previsíveis.

<p align="center">
  <img src="./Imagens/MaquinaEstados.png" alt="Máquina de Estados"><br>
  <a href="./maquina_estados.circ">Link da Máquina de Estados</a>
</p>

**Aplicações comuns:** Sistemas de controle, contadores simples, sequenciadores, unidades de controle em processadores, máquinas de estado para protocolos de comunicação e automação.

---

## 1. Objetivo do Circuito

Dado um pulso de clock, o circuito produz uma sequência binária crescente de 2 bits, demonstrando o funcionamento de uma máquina de estados síncrona. A cada transição, o próximo estado é determinado pela lógica combinacional (portas XOR e NOT) aplicada ao estado atual.

---

## 2. Estrutura do Circuito

### 2.1 Entradas e Saídas
**Entradas:**
- **Clock:** sinal de pulso que sincroniza as transições de estado dos flip-flops.

**Saídas:**
- **Q1:** bit mais significativo do contador (estado atual).
- **Q0:** bit menos significativo do contador (estado atual).

### 2.2 Componentes Principais

- 2 flip-flops tipo D (FF1 e FF0).
- 1 porta XOR (para gerar D1).
- 1 porta NOT (para gerar D0).
- 2 LEDs para visualização dos estados.
- Probes para debug.

### **Funções dos componentes:**
- Os flip-flops armazenam cada bit do estado. Um flip-flop D captura o valor da entrada D na borda de subida do clock.
- A porta XOR compara Q1 e Q0 para gerar o próximo valor de D1.
- A porta NOT inverte Q0 para gerar o próximo valor de D0.
- LEDs permitem visualizar os estados Q1 e Q0 em tempo real.

---

## 3. Funcionamento do Circuito

<p align="center">
  Representação de um Flip-Flop tipo D no Logisim.<br>
  <img src="./Imagens/FlipFlopD.png" alt="Flip-Flop D"><br>
  <a href="./flip_flop_d.circ">Link do Flip-Flop D</a>
</p>

### 3.1 Operação Básica

**A) Antes do pulso:**  
As saídas Q1 e Q0 dos flip-flops já estão definidas e as portas lógicas calculam os próximos valores D1 e D0.

**B) Na subida do clock:**  
Cada flip-flop captura o valor de sua entrada D e atualiza sua saída Q.

**C) Após o pulso:**  
Novas saídas Q são apresentadas, e as portas lógicas recalculam D1 e D0 para o próximo pulso.

**D) Ciclo contínuo:**  
Esse ciclo se repete indefinidamente, retornando ao estado inicial (00) após alcançar o estado final (11).

### 3.2 Lógica Combinacional

**Equações de próximo estado:**

- **D0 = Q̄0** (NOT Q0): O bit Q0 sempre alterna a cada pulso de clock.
- **D1 = Q1 ⊕ Q0** (Q1 XOR Q0): O bit Q1 alterna quando Q0 = 1 (transição de Q0 de 1 para 0).

Essas equações definem a lógica de um contador binário crescente de 2 bits.

### Fluxo das Operações:

| Pulso | Estado (Q1 Q0) | Q1 | Q0 | D1 (Q1⊕Q0) | D0 (Q̄0) | Próximo Estado | Descrição |
|-------|----------------|----|----|-----------|---------|----------------|-----------|
| 0     | 00             | 0  | 0  | 0         | 1       | 01             | Estado inicial |
| 1     | 01             | 0  | 1  | 1         | 0       | 10             | Q0 alterna |
| 2     | 10             | 1  | 0  | 1         | 1       | 11             | Q1 alterna (Q0=1 anterior) |
| 3     | 11             | 1  | 1  | 0         | 0       | 00             | Ambos alternam |
| 4     | 00             | 0  | 0  | 0         | 1       | 01             | Reinicia o ciclo |

---

## 4. Análise Detalhada

### 4.1 Diagrama de Estados

```
    ┌─────┐  Clock  ┌─────┐
    │ 00  │────────►│ 01  │
    └─────┘         └─────┘
       ▲               │
       │               │ Clock
       │               ▼
    ┌─────┐         ┌─────┐
    │ 11  │◄────────│ 10  │
    └─────┘  Clock  └─────┘
```

### 4.2 Propagação dos Sinais

1. **Q0** sempre inverte a cada clock devido à porta NOT.
2. **Q1** inverte apenas quando Q0 passa de 1 para 0 (detectado pela porta XOR).
3. A porta XOR funciona como um detector de "carry" do bit menos significativo para o mais significativo.

---

## 5. Características Importantes

### Vantagens:
- **Simplicidade:** Usa apenas 2 tipos de portas lógicas (XOR e NOT).
- **Sincronismo:** Clock garante transições coordenadas.
- **Previsibilidade:** Sequência de estados bem definida.
- **Modularidade:** Pode ser expandido para contadores de N bits.
- **Eficiência:** Circuito compacto com poucos componentes.

### Limitações:
- Conta apenas de 0 a 3 (4 estados).
- Não possui entrada de reset ou carga paralela.
- Sequência fixa (não programável).

---

## 6. Expansão e Variações

### 6.1 Contador de 4 bits
Para expandir para 4 bits (conta de 0 a 15):
- Adicionar 2 flip-flops (FF2 e FF3).
- Ajustar as equações:
  - D0 = Q̄0
  - D1 = Q1 ⊕ Q0
  - D2 = Q2 ⊕ (Q1 ∧ Q0)
  - D3 = Q3 ⊕ (Q2 ∧ Q1 ∧ Q0)

### 6.2 Contador Decrescente
Inverter a lógica das portas:
- D0 = Q̄0 (mantém)
- D1 = Q1 ⊕ Q̄0 (inverte a lógica do XOR)

### 6.3 Adicionando Reset
Conectar um sinal de reset aos pinos "Clear" dos flip-flops para forçar o estado inicial 00.

---

## 7. Tabela Verdade Completa

| Q1 atual | Q0 atual | D1 | D0 | Q1 próximo | Q0 próximo | Decimal atual | Decimal próximo |
|----------|----------|----|----|------------|------------|---------------|-----------------|
| 0        | 0        | 0  | 1  | 0          | 1          | 0             | 1               |
| 0        | 1        | 1  | 0  | 1          | 0          | 1             | 2               |
| 1        | 0        | 1  | 1  | 1          | 1          | 2             | 3               |
| 1        | 1        | 0  | 0  | 0          | 0          | 3             | 0               |

---

## 8. Teste e Validação

### Procedimentos de teste:
1. **Reset inicial:** Usar a ferramenta "Poke" para limpar os flip-flops (estado 00).
2. **Execução automática:** Ativar "Ticks Enabled" e observar a contagem.
3. **Passo a passo:** Usar "Tick Once" (Ctrl+T) para avançar um clock por vez.
4. **Verificação:** Confirmar que a sequência 00→01→10→11→00 se repete.

### Sinais de problemas:
- Sequência incorreta: verificar conexões das portas lógicas.
- Sem contagem: verificar se o clock está conectado aos flip-flops.
- Estados instáveis: ajustar a frequência do clock.

---

## 9. Fundamentos Teóricos

### Máquinas de Estados Finitas (FSM)
Este circuito é uma **Máquina de Moore**, onde:
- As saídas (Q1, Q0) dependem apenas do estado atual.
- A transição de estados é síncrona (controlada pelo clock).
- O próximo estado é função do estado atual (lógica combinacional).

### Flip-Flop D
- Captura o valor de D na borda de subida do clock.
- Mantém o valor armazenado até o próximo pulso.
- É a base da memória em sistemas digitais síncronos.

---

## 10. Aplicações Práticas

- **Divisores de frequência:** Cada bit divide a frequência do clock por 2.
- **Sequenciadores:** Geração de sequências de controle em sistemas digitais.
- **Endereçamento:** Geração de endereços para memórias pequenas.
- **Temporizadores:** Contagem de eventos ou intervalos de tempo.
- **Controladores:** Estados de máquinas de lavar, semáforos, etc.

---

## 11. Material Complementar

### Fórmulas importantes:
- Número de estados = 2^n (onde n = número de bits)
- Período do ciclo completo = (2^n) × período do clock
- Para 2 bits: 4 estados, ciclo completo em 4 pulsos de clock

### Frequência de saída:
- Q0 oscila a f_clock/2
- Q1 oscila a f_clock/4

---

## Conclusão

Este circuito demonstra os fundamentos de máquinas de estados síncronas usando componentes digitais básicos. A combinação de elementos de memória (flip-flops D) com lógica combinacional (portas XOR e NOT) cria um sistema capaz de gerar sequências previsíveis e controláveis, essencial para o design de sistemas digitais complexos.