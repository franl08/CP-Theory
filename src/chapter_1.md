# Teórica 01

## **Foco Principal**: Engenharia de *Performance*

- Como?
  - Como é que funcionam os novos computadores e o que é que o futuro lhes trará?
  - Como é que podemos medir a eficiência da execução?
  - Como é que funcionam os algoritmos? 
  - Como é que podemos organizar estruturas de dados?

- Onde no *hardware*?
  - Código sequencial com ILP (Parelelismo ao nível das instruções);
  - Memória hierárquica;
  - Paralelismo de processos;
  - Paralelismo em *threads*;
  - Aceleradores (GPU, etc...).

É necessário apostar em paralelismo (tanto ao nível do código, como ao nível dos dados), pois o ILP não pode continuar a ser melhorado por questões físicas.

Assim, temos de apostar em novos modelos de paralelismo:
- Classes de paralelismo em aplicações:
  - **DLP**: Paralelismo ao nível dos dados;
  - **TLP**: Paralelismo ao nível das *tasks*;

- Classes de paralelismo arquitetural:
  - **ILP**: Paralelismo ao nível das instruções;
  - **RLP**: Paralelismo ao nível dos pedidos;
  - **GPUs**: Arquiteturas vetoriais e unidades de processamento gráficas;
  - Paralelismo ao nível das *threads*.

## Taxonomia de *Flynn*

- **SISD**: *Stream* singular de instruções e *stream* singular de dados;
- **SIMD**: *Stream* singular de instruções e múltiplas *streams* de dados;
- **MISD**: Múltiplas *streams* de instruções e *stream* singular de dados;
- **MIMD**: Múltiplas *streams* de instruções e múltiplas *streams* de dados.

## Como medir a *Performance*

- **Métricas**:
    - **Largura de Banda ou *Throughput***:
      - Mede a velocidade;
      - Quantidade de trabalho num intervalo de tempo.
    - **Latência**:
      - Tempo entre o início e o fim de um evento;
      - "Quanto tempo demora a responder?".

- ***Speedup* de X relativa a Y**:
$$ \frac{Tempo\ de\ Execucão_{y}}{Tempo\ de\ Execucão_{x}}$$

- **Tempo de Execução**:
  - *Wall Clock Time*:
    - Inclui os *overheads* do sistema.
  - CPU Time*:
    - Apenas o tempo de computação do programa.

## Princípios de *design* de computadores

- Tirar vantagem do paralelismo;
- Princípio da Localidade:
  - Reutilização de dados e instruções.
- Focar-se no caso comum:
  - Lei de *Amdahl*:
$$speedup\ overall = \frac{t_{exec}\ antigo}{t_{exec}\ novo} = \frac{1}{\sum{\frac{f_i}{s_i}}}, f_i \equiv fracões\ com\ melhoria; s_{i} \equiv speedup\ de\ cada\ funcão$$ 

**Nota**:

Melhorar uma porção do programa 90x não é equivalente a um ganho de 90x no programa.