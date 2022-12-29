# De *Multicore* para *Manycore*

## Problemas Chave

- **Dicas para colocar múltiplos *cores* num único *chip***:
  - Diminuir a capacidade computacional de cada *core*, mas não em demasia;
  - Utiliza uma rede de interconexão escalável no *chip* (NoC);
    - minimiza a latência de acesso à *cache*-partilhada e à memória-partilhada;
    - providencia largura de banda suficiente para a comunicação de dados;
    - minimiza os *bottlenecks* do tráfego.
  - Agrupar *cores* em *clusters* de forma a melhorar a qualidade do NoC;
  - Reduzir o número de níveis e o tamanho da *cache* (é preciso ter em atenção ao impacto no desempenho);
  - Ter processos de fabrico mais pequenos;
  - Misturar PUs de propósito geral com módulos orientados à aplicação: GPUs para computação vetorial, TPU para *tensor computing*, ...
  - Mover para MCM ou *chiplets* (*chips* mais simples)

## Fundamentos da Interconexão

- ***Networks-on-chip***: um avanço nos sistemas de interconexão para conectar servidores em super-computadores;
- Parâmetros chave para definir uma NoC:
  - **topologia**: define como é que os nodos e os *links* estão conectados, nomeadamente todos os caminhos possíveis que uma mensagem poderá tomar pela rede;
  - **algoritmo de *routing***: seleciona o caminho específico que uma mensagem deverá tomar desde a fonte até ao destino;
  - **protocolo de controlo de *flow***: determina a forma como uma mensagem irá atravessar a rota que lhe foi atribuída;
  - ***router micro architecture***: implementa os protocolos de *routing* e de controlo de *flow*, bem como, de forma crítica, dá forma aos seus circuitos.

## Pacotes e *Chips Manycore*

- **Intel**: do *Intel MIC* para a família *Xeon Scalable*;
- **AMD**: família *Epyc Zen*;
- **ARM**: chaves *ARMv8* e *v9* competidores ao nível do servidor;
  - família *Marvell ThunderX*;
  - *chip Fujitsu A64FX Arm*;
  - referência para o design *Neoverse* híper-escalável:
    - *Ampere Altra Arm*;
    - *Amazon Graviton*.
  - *Alibaba Yitian 710*;
  - *Huawei HiSilicon Kunpeng 920*.
- **Sunway**: família *SX260x0*;
- **Cerebras**: *Wafer Scale Engine*;
- **Apple** (não é um servidor): abordagem *SoC* (sem *chiplets*).
  - Defendem que é a melhor abordagem:
    - Menores latências;
    - Melhor processo de silicone (5nm);
    - Melhor *wafer fabrication*.
- **NVidia** (CPU+GPU): *Grace Hopper Superchip*.


