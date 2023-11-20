---
title: "Rinha de Compiladores: Uma Experiência com Haskell"
author: cleissonb
date: 2023-11-20 00:00:00 -0300
image: 
    path: /assets/img/posts/ec84f7175-ffa6-474d-bfba-21b4cbe23439.png
    alt: "Rinha de Compiladores: Uma Experiência com Haskell"
categories: [Haskell, Compiladores, Competição]
tags: [haskell, compiler, compiladores, rinha, compiler-optimization, compiler-architecture, compiler-construction]
pin: true
---

# Rinha de Compiladores: Uma Experiência com Haskell

---

## Introdução

Neste artigo, eu relato minha participação na Rinha de Compiladores, uma competição online de programação que teve como objetivo escrever um compilador para uma linguagem criada apenas para o evento chamada rinha, que era parecida com JavaScript. A competição foi organizada por [Gabrielle Guimarães de Oliveira](https://github.com/aripiprazole){:target="_blank"} e [Sofia Rodrigues](https://github.com/algebraic-sofia){:target="_blank"}, e contou com a participação de mais de 150 competidores de diferentes níveis de experiência e conhecimento.

Escolhi fazer 3 projetos de compiladores em 3 linguagens diferentes mas com abordagens muito parecidas, nesse artigo vamos nos concentrar em apenas um desses projetos, vamos falar da linguagem Haskell, escolhi o Haskell porque achei que seria interessante usar uma linguagem puramente funcional para esse desafio, além de outras peculiaridades bem interessantes da linguagem. Neste artigo, eu descrevo algumas des principais características da linguagem rinha, as ferramentas e bibliotecas que eu utilizei para criar meu compilador, e os resultados que eu obtive.

## A Linguagem Rinha

A linguagem rinha foi criada pelas organizadoras da competição como uma forma de tornar o desafio mais divertido e original. Ela é uma linguagem imperativa, dinamicamente tipada, com suporte a funções de primeira classe, closures, operadores aritméticos e lógicos, estruturas de controle de execução como ```if``` e ```else```. A sintaxe da linguagem é inspirada no JavaScript, mas com algumas diferenças, como o uso de ```let``` para declarar variáveis, o uso de ```fn``` para declarar funções, o uso de ```=>``` para separar os parâmetros e o corpo das funções, o uso de ```first``` e ```second``` para acessar os elementos de uma **tupla** etc.

### Um exemplo de código em rinha:

```javascript
let fib_tail_rec = fn (i, n, a, b) => {
  if (n == 0) {
    let _ = print("@Result:: " + i + " ::");
    a
  } else {
    fib_tail_rec(i + 1, n - 1, b, a + b)
  }
};

let fib = fn (n) => {
  fib_tail_rec(0, n, 0, 1)
};

let x = fib(5);
print(x) // imprime 5
```

## O Compilador em Haskell

Para implementar meu compilador em Haskell, utilizei as seguintes ferramentas e bibliotecas:

1. **GHC**: o compilador de Haskell mais usado e completo, que oferece várias extensões e otimizações da linguagem.

1. **bytestring**: uma biblioteca que fornece uma representação eficiente de sequências de bytes, que podem ser usadas para manipular dados binários, como arquivos JSON.

1. **aeson**: uma biblioteca que fornece uma interface para codificar e decodificar dados JSON, usando o tipo de dados Value, que representa os valores JSON, e as classes de tipos ToJSON e FromJSON, que permitem converter valores Haskell para e de JSON.

1. **Cabal**: uma ferramenta que permite gerenciar projetos em Haskell, especificando as dependências, os módulos, as opções de compilação e os testes do projeto.

1. **containers**: uma biblioteca que fornece estruturas de dados eficientes e persistentes, como mapas, conjuntos, sequências e árvores, que podem ser usadas para armazenar e manipular dados de forma organizada e funcional.

1. **process**: uma biblioteca que fornece uma interface para criar e interagir com processos externos, permitindo executar comandos do sistema operacional, ler e escrever na entrada e saída padrão, e obter o código de retorno do processo.

1. **time**: uma biblioteca que fornece uma interface para lidar com datas e horas, permitindo obter o tempo atual, formatar e analisar strings de tempo, e realizar cálculos com unidades de tempo.

### O processo de compilação consistiu nas seguintes etapas:

1. **Análise léxica**: usando a biblioteca bytestring, eu li o arquivo JSON que continha a AST do código fonte em rinha, e converti os bytes em uma string UTF-8, que foi passada para a próxima etapa.

1. **Análise sintática**: usando a biblioteca aeson, eu decodifiquei a string JSON em um valor Haskell do tipo File, que continha o nome, a expressão e a localização do código fonte. Em seguida, eu usei a classe de tipo FromJSON para converter o valor Haskell em uma árvore sintática abstrata (AST) do tipo Term, que representava a estrutura e o significado do código fonte. O resultado dessa etapa foi uma AST, que foi passada para a próxima etapa.

1. **Geração de código intermediário**: usando a biblioteca containers, eu criei uma tabela de símbolos que armazenava os nomes e os valores das variáveis e das funções definidas no código fonte. Em seguida, eu usei uma função recursiva para percorrer a AST e avaliar os termos, usando a tabela de símbolos para resolver as referências e as chamadas de funções. O resultado dessa etapa foi um valor Haskell do tipo Varr, que representava o valor final do código fonte, que podia ser um inteiro, uma string, um booleano, uma tupla, uma função etc.

## Resultados

Após o término do desenvolvimento do meu compilador, eu submeti o meu código fonte que continha um arquivo Dockerfile onde era possivel executar o compilador conforme as regras da competição e solicitação das organizadoras, que avaliaram o meu compilador de acordo com os critérios definidos por elas. 

Os critérios foram baseados em sistemas de pontos e pesos sobre determinados aspectos do compilador, como a completude, a eficiência, compilação etc. Acabei ficando em terceiro lugar na competição, com um total de 72458 pontos, sendo superado apenas pelos competidores que ficaram em primeiro e segundo lugar, com 72786 e 72582 pontos, respectivamente. Os dois primeiros colocados foram Raphael M. R. Victal e Tacio, que usaram a linguagem Golang e o mesmo tipo de compilador que eu usei, o Tree-Walker. Um Tree-Walker é um tipo de compilador que percorre a AST e executa as ações correspondentes a cada nó, sem gerar código intermediário ou de máquina.

[Raking final da competição](https://github.com/aripiprazole/rinha-de-compiler/blob/main/README.md#resultados){:target="_blank"}:

| Rank | Name | Language | TYpe | Points |
| :--: | :--: | :--: | :--: | :--: |
| 1 | Raphael M. R. Victal | Golang | Tree-Walker | 72786 |
| 2 | Tacio | Golang | Tree-Walker | 72582 |
| 3 | cleissonbarbosa | Haskell | Tree-Walker | 72458 |
| 4 | Danfs | TypeScript | Tree-Walker | 70096 |
| 5 | Valmor Flores | Flutter |  | 69584 |
| 6 | Victor Augusto | TypeScript | Tree-Walker | 69273 |
| 7 | fabiosvm | C | Bytecode Interpreter | 68737 |
| 8 | coproduto | ⚡Zig | Tree-Walker | 68647 |
| 9 | Adriano dos Santos Fernandes | C++ | Tree-Walker | 68309 |
| 10 | Ítalo Paulino (irbp) | Dart 🎯 | Tree-Walker | 67919 |