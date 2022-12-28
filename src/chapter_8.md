# Medição e Otimização de Desempenho em Memória Partilhada

## Desempenho em Aplicações Paralelas

- Qual é a definição de desempenho?
  - Existem múltiplas alternativas.
  - Tempo de execução, escalabilidade, requisitos de memória, latência, débito, custos, portabilidade, potencial de reutilização, etc...
  - A importância de cada um, dependerá da aplicação em concreto.
- A medida mais comum das aplicações paralelas é o tempo de execução ou *speed-up*.
  - Temos da melhor implementação sequencial *vs* Tempo da versão paralela;
  - Forte análise da escalabilidade:
    - aumento do *speed-up* com a PU para um problema com um tamanho fixo de dados;
    - O *speed-up* ideal é proporcional à PU.
  - Fraca análise da escalabilidade:
    - Aumentam o problema do tamanho de dados assim que o número de PUs aumenta;
    - Idealmente, o tempo de execução dever-se-ia manter constante.

### Lei de Amdhal (Forte Análise da Escalabilidade)

- Mede o tempo da versão paralela ($T_{par}$) assim que o número de PUs aumenta;
- O $T_{seq}$ pode ser dividida em:
  - Tempo a fazer trabalho não paralelizável (trabalho sequencial);
  - Tempo a fazer trabalho paralelizável.
- A fração do trabalho não paralelizável irá limitar o *speed-up* máximo;
  - $P$ $\rightarrow$ número de PUs;
  - $f$ $\rightarrow$ fração sequencial.

$$S_P \leq \frac{1}{f + (1 - f) / P}$$
$$Max_{speedup} = \frac{1}{serial\ fraction\ of\ work}$$

- Isto reforça a ideia que devemos preferir algoritmos que suportem uma execução paralela: *think parallel*.

### Anomalias de *Speed-up*

- *Super-linear* (o ganho é maior que o número de PUs): em muitos casos isto deve-se aos efeitos da *cache*.

### Lei de Gustafson (Fraca Análise da Escalabilidade)

- Aumenta o tamanho do problema consoante o número de PUs aumenta;
  - Grandes recursos computacionais, geralmente, são utilizados para problemas de grandes dimensões.
- A fração de trabalho sequencial, geralmente, diminui com o tamanho do problema.

### Estudos Experimentais

- Perfil de execução sequencial:
  - Identifica *hot-spots* de aplicações:
    - Funções que levam mais tempo a executar.
  - Podem ser implementadas por ferramentas específicas ou instrumentalizar diretamente o código:
    - Existe sempre *overhead* introduzido na base de aplicação.
- Perfil de execução paralela:
  - Recolha dados de desempenho *per-thread*;
  - Mais difícil de interpretar.
- Os *hot-spots* podem alterar enquanto a aplicação é melhorada.

### Técnicas para fazer *profiling*

- *Polling (sampling)*
  - A aplicação é, periodicamente, interrompida para colecionar dados de desempenho;
  - Exemplos: `gprof` e `perf record`.
- *Instrumentation*
  - O código é introduzido (pelo programador ou ferramentas) para colecionar dados de desempenho acerca de eventos úteis:
    - Tende a produzir melhores resultados, mas mais *overhead*.
  - Exemplos: `Valgrind`.

## Problemas de Escalabilidade na Memória Partilhada

### Algumas Razões para as Aplicações não terem o Speed-Up ideal?

- Trabalho Sequencial:
  - \% de trabalho sequencial (Lei de Amdahl);
  - Barreira de Memória.
    - Serializa os acessos à memória.
- *Overhead* de Paralelismo:
  - Granularidade de Paralelismo/*Task*.
    - Trabalho adicional feito em aplicações paralelas (gestão de tarefas, computações redundantes, etc...).
- *Overhead* de Paralelismo **e** Trabalho Sequencial/Tempo em *Idle*:
  - *Overhead* de Sincronização.
    - Também deveria serializar a execução (p.e. *critical*).
      - Inclui múltiplas chamadas a rotinas externas (p.e. `malloc`).
- Trabalho Sequencial/Tempo em *Idle*
  - *Over-decomposition* poderá melhorar o *load balancing*.

### Razões para a Falta da Escalabilidade

- Limitação à largura de banda na memória/*cache*:
  - Diagnóstico (algumas opções):
    - Medir a largura de banda (de memória, por *core*) necessária e comparar à largura de banda necessária;
    - Utilização do *roofline model* para analisar a intensidade computacional;
    - O CPI aumenta com o número de *threads*.
  - Ações:
    - Melhorar a localidade de dados.
  - Abordagem:
    - Converter *Arrays of Points* em *Arrays of Structures* ou *Structures of Arrays*.
- Paralelismo de grão fino (demasiado *overhead* no paralelismo):
  - Diagnóstico:
    - Medir a granularidade das tarefas.
  - Ação:
    - Aumentar a granularidade das tarefas para diminuir o *overhead* do paralelismo.
  - Abordagens:
    - Favorecer *static loop schedulling* (em certos casos precisa de ser implementado explicitamente);
    - Diminuir a frequência de criação de tarefas.
- Demasiada sincronização de tarefas (devido a dependências):
  - Diagnóstico:
    - Correr tarefas sem sincronizá-las (irão produzir resultados errados).
  - Ação:
    - Remover sincronização.
  - Abordagens:
    - Aumenta a granularidade das tarefas;
    - Computações redundates/especulativas;
    - Utilização de valores de *threads* locais (preciso ter cuidado com falsa partilha de linhas de *cache*/utilização de memória).
- Má distribuição de carga:
  - Diagnóstico:
    - Medir o tempo de cada tarefa computacional.
  - Ação:
    - Melhorar o agendamento/mapeamento.
  - Abordagens:
    - Agendamento cíclico/dinâmico/guiado;
    - Agendamento (estático) personalizado do *loop*.

## Medir o desempenho

### Princípios

- Isolar de fatores externos;
  - Considerar o *overhead* inserido pela medição;
  - Repetir várias vezes a medição;
  - Evitar sobrecargas do sitema.
- Documentar o ocorrido para que pode ser replicado por outros;
  - *Hardware*, versões de *software*, estado do sistema, etc...
- **Importante**: Resolução do Relógio
  - Precisão: diferença entre o tempo medido e o tempo real;
  - Resolução: unidade de tempo entre os incrementos do relógio.
    - Em princípio, não é possível medir eventos menores que a resolução do relógio, mas...

### Tempo para correr uma aplicação

- **Tempo de CPU**
  - Tempo dedicado, em exclusivo, para a execução do programa;
  - Não depende de outras atividades do sistema.
- ***Wall Time***
  - Tempo medido entre o início e o fim da execução;
  - Dependerá da carga do sistema, de I/O, etc...
- **Complexidades**
  - Agendamento de processos (10ms?);
  - Carga introduzida por outros processos (p.e. pelo *garbage collector* em Java).

### Opções para medir o tempo

- `time` na *command line*;
  - Apenas para medidas superiores a 1s.
- `gettimeofday()`
  - Devolve o número de microsegundos desde o dia 1 de janeiro de 1970;
  - Utiliza o "*Timer*" ou o contador de ciclos (depende da plataforma);
  - No melhor caso: 1 microsegundo.
- Contador de *clock cycle* (introduzido nos processadores modernos)
  - Grande resolução;
  - Útil para medidas inferiores a 1s.
- Por fim, temos as abordagens preferidas que são implementações de alta resolução:
  - Função de *timer* do *OpenMP*/MPI
    - `omp_get_wtime`, `omp_get_wtick`;
    - `MPI_WTime`.
  - `System.nanoTime()` em Java 4.

### Como combinar os resultados das várias medições?