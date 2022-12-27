# Hierarquia de Memória

## Hiato Processador-Memória

- Para cada instrução, deve ser feito o seguinte processo:
  - Ler Instrução;
  - Ler Operando;
  - Escrever Resultado.
- O **hiato processador-memória** diz-nos:
  - "A memória é incapaz de alimentar o processador com instruções e dados com uma taxa suficiente de forma a mantê-lo constantemente ocupado".
- A causa para este problema é a diferente taxa de aumento do desempenho entre os processadores e a memória nos últimos anos;
- Trata-se de um dos principais obstáculos à melhoria do desempenho dos sistemas de computação.

## Princípio da Localidade

- Permite acelerar os acessos à memória através de uma hierarquia;
  - "Os programas tendem a aceder a uma porção limitada de memória num dado período de tempo".
- Permite utilizar memória mais rápida para armazenar a informação usada mais frequentemente/recentement;
- Permite tirar partido da largura de banda, uma vez que a informação transferida entre diferentes níveis da hierarquia é efetuada por blocos.

### Localidade Temporal

Um elemento de memória acedido pelo processador será, com grande probabilidade, acedido de novo num futuro próximo;

- **Exemplos**:
  - Tanto as instruções dentro dos ciclos, como as variáveis usadas como contadores de ciclos, são acedidas repetidamente em curtos intervalos de tempo.
- **Consequência**:
  - A 1ª vez que um elemento de memória é acedido deve ser lido do nível mais baixo (p.e. da memória central);
  - Da 2ª vez que é acedido, no entanto, é muito provável que este se encontre em *cache*, evitando-se assim o tempo de leitura da memória central.

### Localidade Espacial

Se um elemento de memória é acedido pelo CPU, então elementos com endereços na sua proximidade serão, com grande probabilidade, acedidos num futuro próximo.

- **Exemplos**:
  - As instruções do programa são, normalmente, acedidas em sequência, assim como, na maior parte dos programas, os elementos de vetores/matrizes.
- **Consequência**:
  - A 1ª vez que um elemento de memória é acedido, deve ser lido do nível mais baixo (p.e. memória central), no entanto, não será lido sozinho, mas sim com um bloco de elementos com endereços na sua vizinhança.
  - Se o processador, nos próximos ciclos, aceder a um endereço vizinho do anterior (p.e. próxima instrução ou próximo elemento de um vetor), a probabiliade desse elemento já estar em *cache* é elevada.

### Inclusão

- Os dados contidos num nível mais próximo do processador são um subconjunto dos dados contidos no nível anterior;
- O nível mais baixo contém a totalidade dos dados;
- Os dados são copiados entre níveis em blocos.

![image Níveis hierárquicos](images/hierarquia_mem.png)

### Terminologia

- **Linha**: A *cache* encontra-se dividida em linhas. Casa linha terá o seu endereço (índice) e tem a capacidade de um bloco;
- **Bloco**: Quantidade de informaçõa que é transferida de cada vez da memória central para a *cache* (ou entre níveis de *cache*). É igual à capacidade da linha;
- ***Hit***: Diz-se que ocorreu um *hit* quando o elemento de memória acedido pelo CPU se encontra em *cache*;
- ***Miss***: Diz-se que ocorreu um *miss* quando o elemento de memória acedido pela CPU não se encontra em *cache*, sendo necessário lê-lo do nível inferior da hierarquia.
- ***Hit Rate***: Percentagem de *hits* ocorridos relativamente ao total de acessos à memória;
- ***Miss Rate***: Percentagem de *misses* ocorridos relativamente ao total de acessos à memória. ($Miss\ Rate\ = 1 - hit\ rate$);
- ***Hit Time***: Tempo necessário para aceder à *cache*, incluindo o tempo necessário para determinar se o elemento a que o CPU está a aceder se encontra, ou não, em *cache*;
- ***Miss Penalty***: Tempo necessário para carregar um bloco da memória central (ou de um nível inferior) para a *cache* quando ocorre um *miss*.

## Causas de *Misses*

- **Obrigatória**;
  - Primeira referência ao bloco.
- **Capacidade**;
  - Blocos que são descartados e mais tarde são necessários.
- **Conflito**;
  - O programa faz repetidas referências a endereços de diferentes blocos de forma a mapear a mesma localização em *cache*.

## *Multilevel Caches*

De forma a evitar o máximo de *misses* possível, opta-se por uma arquitetura de *caches multilevel*. Assim, tem-se os seguintes níveis:

- *Cache* primária presa à CPU;
  - Muito pequena, mas rápida.
- *Cache* de nível 2;
  - Serve os *misses* da *cache* de nível 1;
  - Maior que a anterior, mas mais lenta, no entanto, mais rápida que a seguinte ou que a memória principal.
- *Cache* de nível 3 (em alguns casos, não existe e poderá já ser a memória principal);
  - Serve os *misses* da *cache* de nível 2;
  - Maior que a anterior, mas mais lenta, sendo mais rápida que a memória principal.
- Memória Principal.
  - Serve os *misses* da *cache* de nível 3 ou, caso esta não exista, os *misses* da *cache* de nível 2.

![image Exemplos](images/3_level_cache_organization.png)
