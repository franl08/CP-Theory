# Introdução à Programação com Memória Partilhada

## Níveis de Paralelismo (*Hardware* e *Software*)

- Instrução (ILP):
  - Execução de múltiplas instruções de um programa em paralelo;
  - Processamento vetorial;
  - Explorado pelo *hardware* atual;
  - Limitado pelas dependências de dados/controlo do programa.
- Tarefas/Fios de Execução
  - Múltiplos fluxos de instruções de um mesmo programa executam em paralelo;
  - Limitado pelas dependências e características do algoritmo.
- Processos
  - Múltiplos processos de um mesmo programa/vários programas.

## Desenvolvimento de Aplicações Paralelas

**Partição do problema e dos dados a processar**:

- Identifica oportunidades de paralelismo:
  - Define um elevado número de tarefas (de grão fino);
  - Pode obter várias decomposições alternativas.
- Duas vertentes complementares na identificação das tarefas:
  - **Decomposição dos Dados**: identifica dados que podem ser processados em paralelo;
    - foca-se nos dados e processar e na sua divisão em conjuntos que possam ser processados em paralelo.
  - **Decomposição Funcional**: identifica fases do algoritmo que podem ser efetuadas em paralelo.
    - foca-se no processamento a realizar, dividindo-o em tarefas independentes.
- A partição deverá obter um número de tarefas, pelo menos, uma ordem de magnitude superior ao número de unidades de processamento.
  - Introduz flexibilidade nas fases posteriores do desenvolvimento.
- Tarefas de dimensões idênticas facilitam a distribuição da carga;
- O número de tarefas deve aumentar em conformidade com a dimensão do problema.