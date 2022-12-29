# Sistemas Top HPC nas listas do TOP500

## HPC

Acrónimo para *High Performance Computing*.

## O que é o TOP500

É uma lista dos 500 sistemas de computação mais poderosos no mundo, sendo compilada pela organização TOP500.

Esta lista é baseada nos *benchmarks* LINPACK que mede a velocidade à qual um computador consegue resolver um denso sistema de equações lineares.

É muito conceituada e, por isso, é utilizada para seguir a evolução de tecnologia dos computadores e o aumento do poder dos sistemas de computação. É muito seguida na indústria da computação e, por vezes, utilizada por governos, instituições de pequisa e empresas para comparar os seus sistemas aos melhores do mundo.

## LINPACK *Benchmarks* (HPL)

Mede a velocidade com a qual um computador é capaz de resolver um sistema denso de $n$ equações lineares do tipo $Ax = b$.

O HPL, escrito em C, mede a taxa de *floating-point* sustentado (*GFLOPs/s*) para resolver um sistema linear de equações utilizando aritmética em *double-precision floating-point*.

## TOP500

### Análise de Sistemas Chave em 2022

1. *Frontier* (*AMD Epyc Trento 64c* + *AMD Instinct MI250x*);
2. *Fugaku* (*Fujitsu A64FX*, 48 *cores*);
3. *Leonardo* (*3ª Gen Xeon, 32c* + *NVidia Ampere A100*);
4. *Summit* (*IBM POWER9*, 22 *cores* + *NVidia Volta GV100*) + *Sierra*;
5. *TaihuLight* (*Sunway SW26010*, 260 *cores*);
6. *Selene* (*AMD Epyc Rome 64c* + *NVidia A100*);
7. *Tianhe-2A* (*MilkyWay-2A*) (*Xeon*, 12c + *Matrix-2000*).

## O que é o GREEN500?

Lista dos 400 sistemas de computação ordenados com base na sua eficiência energética, tipicamente medida em *LINPACK FLOPS per Watt*.

## O que é a *benchmark* HPCG? 

É um programa em C++ auto-contido com MPI e OpenMP que suporta medidas de desempenho em operações básicas num código unificado:
- Multiplicação de matrizes ou vetores dispersos;
- Atualizações de vetores;
- *Dot Products* globais;
- *Local Symmetric Gauss-Seidl*;
- *Sparse triangular solve*.

## O que é o HPL-AI?

*High Performance Linpack for Artifical Intelligence* é uma *benchmark* para avaliar o desempenho de sistemas de computação em cargas de trabalho de *deep learning*. É baseado na *benchmark LINPACK*.

É uma ferramenta importante para identificar os sistemas mais poderosos para aplicação de inteligência artificial e para dar *track* ao progresso das tecnologias de computação nesta área.