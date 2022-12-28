# Programação Paralela com Memória Partilhada

## Especificação de Concorrência/Paralelismo

- **Processos**
  - Usados para tarefas não correlacionadas;
    - Por exemplo, um programa.
  - Contém espaço de endereçamento;
    - Que é protegido de outros processos.
  - Efetua *switching* ao nível do *kernel*;
    - Troca entre diferentes processos ou tarefas que estão a ser geridas pelo *kernel*.
  - Cada processo tem, pelo menos, uma *thread*.
- ***Threads***
  - São parte do mesmo trabalho;
  - Partilham espaço de endereçamento, código, dados e ficheiros;
  - Efetuam *switching* ao nível do utilizador e do *kernel*.

## Processos/*Threads* vs *Tasks*

- *Task*: sequência de instruções;
  - Em Java: *Runnable Object*;
- *Thread*/Processo: execução de uma *task* num dado contexto;
  - Em Java: *Thread*.
- Processador/*core*: *hardware* que corre uma *thread*/processo.