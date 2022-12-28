# Aceleradores de Computação: GPU e CUDA

## Aceleradores de Computação

- O melhor acelerador para *number crunching* ou para computação intensiva de vetores/matrizes é a GPU;
- Temos ainda outros aceleradores comuns:
  - **DSP**: *Digital Signal Processor*;
    - Usado, maioritariamente, em equipamentos de telecomunicações.
  - **TPU**: *Tensor Unit Processing Units*;
    - Otimizado para operações com tensores (vetores e matrizes n-dimensionais), popularizado em aplicações AI, nomeadamente para condução autónoma.
  - **FPGA**: *Field Programmable Gate Arrays*.
    - *Hardware*/*Software* reconfigurável;
    - Pode ser configurado em *runtime* de forma a comportar-se como uma dada especificação.

## *Graphical Processing Units* (GPU)

- Ideia base:
  - Modelos de execução heterogéneos;
    - A CPU será o *host* e a GPU o *device*.
  - Deve desenvolver-se um programa numa linguagem semelhante a C para a GPU;
  - Une todas as formas de paralelismo da GPU como uma *thread* CUDA;
  - O modelo de programação segue o SIMT (*Single Instruction Multiple Thread*).

### Arquitetura de uma GPU da Nvidia

- Semelhanças com máquinas vetoriais:
  - Trabalha bem com problemas de paralelismo ao nível dos dados;
  - Aplica o *scatter-gather* às transferências;
  - Aplica *masks* aos registos;
  - Tem grandes ficheiros de registo.
- Diferenças:
  - Não tem um processador escalar;
  - Utiliza *multithreading* de forma a esconder a latência da memória;
  - Tem múltiplas unidades funcionais, mas poucas unidades em *pipeline*, trabalhando como um processador vetorial.

### Terminologia

- Cada *thread* está limitada a 64 registos;
- Grupos de 32 *threads* estão combinados numa *thread* SIMD, também chamada *warp*;
  - Mapeada em 16 pistas físicas.
- Até 32 *warps* são agendados num único processador SIMD (SM);
  - Cada *warp* terá o seu próprio PC;
  - O agente responsável pelo agendamento das *threads* utiliza um *scoreboard* de forma a expedir as *threads*;
  - Por definição, não existem dependências de dados entre *warps*;
  - Ao expedir os *warps* para *pipelines*, esconderá a latência da memória.
- Cada processador SIMD (SM):
  - Tem 32 pistas SIMD;
  - É largo e mais raso quando comparado com processadores vetoriais.

### Estruturas de Memória de uma GPU da Nvidia

- Cada pista SIMD tem uma secção privada de um *off-chip* DRAM;
  - "Memória Privada" (na terminologia da Nvidia, *Local Memory*);
  - Contém um *stack frame*, *spilling registers* e variáveis privadas.
- Cada processador SIMD *multithreaded* (na terminologia da Nvidia, *SM*) também possui memória local (na termionologia da Nvidia, *Shared Memory*);
  - Partilhada pelas pistas SIMD/*threads* num dado bloco.
- A memória partilhada pelos processadores SIMD é memória da GPU e o *off-chip* DRAM (na terminologia da Nvidia, *Global Memory*).
  - O *host* poderá ler e escrever na memória da GPU.

### Inovações da Arquitetura Pascal (maio 2016)

- Cada processador SIMD tem:
  - Dois ou 4 agentes de agendamento de *threads* SIMD, duas unidades de expedição de instruções;
  - 4 pistas de 16 SIMD, 16 unidades de *load-store* e 16 unidades de funções especiais;
  - 2 *threads* de instruções SIMD são agendadas a cada 2 *clock cycles*.
- Introdução de:
  - *Fast single-precision*;
  - *Doube-precision*;
  - *Half-precision*.
- *High Bandwith Memory 2* (HBM2) com 732 GB/s;
- *NVLink* entre múltiplas GPUs (20 GB/s em cada direção);
- Memória virtual e *paging support* unificados.

## Arquiteturas Vetoriais *vs* GPUs

- Processador SIMD análogo ao processador vetorial, ambos têm MIMD;
- Registos:
  - O ficheiro de registo RV64V é capaz de manter vetores inteiros, enquanto que a GPU irá distribuir os vetores em registos das pistas SIMD;
  - O RV64V tem 32 registos de vetores de 32 elementos (total de 1024 elementos), já a GPU tem 256 registos com 32 elementos (total de 8000 elementos);
  - RV64V tem 2 a 8 pistas com o tamanho do vetor a ser de 32, e o *chime* terá entre 4 a 16 ciclos, já no proocessador SIMD o *chime* terá 2 a 4 ciclos;
    - *quick reminder*: o *chime* é uma unidade de tempo que demora a executar um *convoy*;
      - *convoy*: um conjunto de instruções vetoriais que, potencialmente, poderá começar a execução em conjunto num período de relógio.
  - O *loop* vetorizado da GPU é uma grelha;
  - Todas as instruções de leitura da GPU são *gather* e todas as instruções de guardar são *scatter*.

## Arquiteturas SIMD *vs* GPUs

- As GPUs possuem mais pistas SIMD;
- As GPUs têm *hardware* para suportar mais *threads*;
- Ambas têm um rácio de 2:1 entre o desempenho entre *double-precision* e *single-precision*;
- Ambas têm endereços de 64 *bits*, mas as GPUs têm menor memória;
- Arquiteturas SIMD não têm suporte para instruções *scatter-gather*.


## Modelo de programação CUDA

- *Compute Unified Device Architecture*;
- Desenhado para:
  - *hosts* com CPUs *multicore* acoplados a dispositivos *many-core* onde:
    - os dispositivos têm um grande paralelismo SIMD/SIMT;
    - o *host* e o dispositivo não partilham memória.  
- Providencia:
  - uma abstração de *threads* de forma a lidar com o SIMD;
  - sincronização e partilha de dados entre pequenos grupos de *threads*.
- Os programas em CUDA são escritos em C com extensões;
- O OpenCL é inspirado no CUDA, mas o *hardware* e o *software* não têm um produtor específico.
  - O modelo de programação é, essencialmente, idêntico.

### Dispositivos CUDA e *Threads*

- Um dispositivo de computação:
  - é um co-processador para a CPU ou *host*;
  - tem a sua própria DRAM (chamada, *device memory*);
  - corre múltiplas *threads* em paralelo;
  - tipicamente, é uma GPU, mas também ser qualquer outro tipo de dispositivo de processamento em paralelo.
- As porções de dados paralelos de uma aplicação são expressas com *kernels* do dispostivo que irão correr em múltiplas *threads* (SIMT);
- Diferenças entre *threads* de GPU e *threads* de CPU:
  - *threads* de GPU são extremamente leves;
    - pouco *overhead* de criação, mas requerem um grande banco de registos.
  - a GPU precisa de milhares de *threads* para apresentar a sua eficiência mãxima;
    - por outro lado, os CPUs *multicore* precisam de poucas.

### Modelo básico de CUDA: *Single Program, Multiple Data* (SPMD)

- CUDA integrado na CPU + aplicação de um programa em C na GPU;
  - Código sequencial C executa na CPU;
  - *Kernel* paralelo C executa em blocos de *threads* na GPU.

### SPMD + SIMT/SIMD

- **Hierarquia**:
  - Dispostivo $\rightarrow$ Grelhas;
  - Grelhas $\rightarrow$ Blocos;
  - Blocos $\rightarrow$ *Warps*;
  - *Warps* $\rightarrow$ *Threads*.
- Um único *kernel* corre em múltiplos blocos (SPMD);
- As *threads* de um *warp* são executadas numa forma *lock-step* denominada *single instruction, multiple thread* (SIMT);
- Instruções singulares são executas em múltiplas *threads* (SIMD);
  - O tamanho do *warp* define a granularidade do SIMD (32 *threads*).
- A sincronização dentro de um bloco utiliza a memória partilhada.

### Grelha Computacional: *Block IDs* e *Thread IDs*

- Um *kernel* corre numa grelha computacional de blocos de *threads*;
  - As *threads* utilizam memória global partilhada.
- Cada *thread* utiliza IDs para decidir em que dados deve trabalhar;
  - ID do bloco: 1D ou 2D;
  - ID da *thread*: 1D, 2D ou 3D.
- Um bloco de *threads* é um *batch* de *threads* que podem cooperar entre si:
  - Sincronizam a sua execução com uma barreira;
  - Partilham dados de forma eficiente através de memória partilhada de baixa latência;
  - Duas *threads* de dois diferentes blocos não podem cooperar entre si.

### Terminologia (e terminologia da Nvidia)

- *Threads* de instruções SIMD (*warps*);
  - Cada uma tem o seu próprio *Instruction Pointer* (até 48/64 por processador SIMD);
  - O agente responsável por agendar *threads* utiliza *scoreboards* para as expedir;
  - Não existem dependências de dados entre *threads*;
  - As *threads* são organizadas em blocos e executadas em grupos de 32 *threads* (bloco de *threads*).
    - Os blocos estão organizados numa grelha.
- O agente responsável por agendar os *thread blocks* agenda-os para os processadores SIMD (*Streaming Multiprocessors*);
- Em cada processador SIMD:
  - 32 pistas SIMD (*thread processors*);
  - Mais largos e rasos quando comparados a processadores vetoriais.

### Bloco de *Threads* CUDA

- O programador declara o bloco (de *threads*):
  - tamanho irá variar entre 1 a 512 *threads* concorrentes;
  - terá um formato 1D, 2D ou 3D;
  - dimensões dos blocos em *threads*.
- Todos as *threads* num bloco executam o mesmo programa de *thread*;
- As *threads* partilham os dados e sincronizam-se enquanto partilham o seu trabalho;
- As *threads* têm números de identificação dentro de um bloco;
- Os programas de *threads* utilizam os *thread ID* de forma a selecionar os trabalhos e endereçar os dados partilhados.

```c
float x = input[threadID];
float y = func(x);
output[threadID] = y;
```

### Partilha de memória partilhada

- Memória Local (por *thread*):
  - Privada por *thread*;
  - Variáveis automáticas, *register spill*.
    - *quick reminder*: ocorre *register spill* quando os registos da CPU estão cheios, pelo que o conteúdo tem de ser, temporariamente, guardado em memória.
- Memória Partilhada (por bloco):
  - Partilhada por *threads* no mesmo bloco;
  - Comunicação inter-*threads*.
- Memória Global (por aplicação):
  - Partilhada por todas as *threads*;
  - Comunicação inter-grelhas.

### *Overview* do modelo de memória CUDA

- Cada *thread* pode:
  - Ler/Escrever *per-thread* em registos;
  - Ler/Escrever *per-thread* em memória local;
  - Ler/Escrever *per-block* em memória partilhada;
  - Ler/Escrever *per-grid* em memória global;
  - Ler apenas *per-grid* em memória constante;
  - Ler apenas *per-grid* em memória de textura.
- O *host* pode ler e escrever em memória global, constante ou de textura.

### Implementação em *Hardware*: Arquitetura de Memória

- Memória do Dispositivo (DRAM):
  - Lenta (2 a 300 ciclos);
  - Em memória local, global, constante ou de textura.
- *On-chip memory*:
  - Rápida (1 ciclo);
  - Registos, memória partilha e *cache* de constantes/texturas.