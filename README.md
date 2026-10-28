# Projeto 07 - Detecção de elementos duplicados

Este projeto implementa um algoritmo de detecção de elementos duplicados em vetores utilizando a comparação de pares. O programa contabiliza o número de comparações realizadas e permite analisar o melhor e pior caso do algoritmo.

## Tecnologias utilizadas

 • Linguagem C
 • GCC (GNU Compiler Collection)
 • MSYS2 para ambiente de compilação no Windows

## Pré-requisitos

Para compilar e executar o projeto no Windows, é necessário instalar o GCC por meio do MSYS2.

1. Instalação do MSYS2

Acesse o site oficial e baixe o instalador:
https://www.msys2.org/

Siga as instruções de instalação disponibilizadas no site.

2. Instalação do GCC

Abra o terminal MSYS2 UCRT64 pelo menu Iniciar do Windows e execute:

```pacman -S --needed mingw-w64-ucrt-x86_64-gcc
```

Caso seja solicitada uma confirmação, pressione Y e depois Enter.

3. Configuração das variáveis de ambiente

Para executar o GCC pelo Prompt de Comando ou pelo terminal do Visual Studio Code:

 a. Pesquise no menu Iniciar por Editar as variáveis de ambiente do sistema.
 b. Clique em Variáveis de Ambiente.
 c. Na seção Variáveis de usuário, selecione Path e clique em Editar.
 d. Clique em Novo e adicione o caminho:

 ``` C:\msys64\ucrt64\bin ```

 e. Confirme as alterações clicando em OK em todas as janelas.

 Observação: o caminho acima considera a instalação padrão do MSYS2 em C:\msys64. Caso tenha instalado em outro diretório, ajuste o caminho.

4. Verificação da instalação

Abra um novo terminal do Windows e execute:

```gcc --version```

Se o comando exibir as informações da versão do GCC, a instalação e a configuração do PATH foram realizadas corretamente.

## Objetivos

O projeto tem como objetivo implementar e analisar um algoritmo em C capaz de identificar elementos duplicados em um vetor. Além da detecção de valores repetidos, o programa contabiliza a quantidade de comparações realizadas durante a busca.

Com isso, é possível observar na prática a diferença entre o melhor e o pior caso do algoritmo, relacionando os resultados obtidos durante a execução com a análise teórica e matemática de sua complexidade.

## Funcionamento

Cada elemento é comparado com os seguintes usando dois laços `for`. O segundo laço começa em `j = i + 1`, evitando comparar uma posição consigo mesma ou repetir um par.

Quando encontra dois valores iguais, a função encerra a busca e retorna `true`. Se terminar sem encontrar repetição, retorna `false`.

O contador é zerado a cada chamada e considera apenas as comparações entre os elementos. O vetor recebido permanece inalterado.

## Arquivos

- `src/duplicatas.c`: implementação do algoritmo.
- `src/duplicatas.h`: declaração da função e suas condições de uso.
- `src/main.c`: execução dos exemplos de melhor e pior caso.
- `README.md`: documenta o projeto e as instruções de execução.

## Como compilar e executar

Com o GCC instalado, abra o PowerShell na pasta do projeto e compile:

```powershell
New-Item -ItemType Directory -Path build -Force | Out-Null
gcc -std=c11 -Wall -Wextra -Wpedantic -Werror src/main.c src/duplicatas.c -o build/duplicatas.exe
```

Depois, execute:

```powershell
.\build\duplicatas.exe
```
## Funcionamento do algoritmo

bool tem_duplicado(const int vetor[], size_t n, uint64_t *comparacoes);

O algoritmo utiliza dois laços de repetição. O primeiro seleciona um elemento do vetor e o segundo compara esse elemento com todos os elementos que aparecem depois dele.

A estrutura principal é:

for (size_t i = 0; i + 1 < n; i++) {
    for (size_t j = i + 1; j < n; j++) {
        (*comparacoes)++;

        if (vetor[i] == vetor[j]) {
            return true;
        }
    }
}

Como j começa em i + 1, o algoritmo evita comparações desnecessárias. Um elemento não é comparado consigo mesmo e um mesmo par não é analisado duas vezes.

Por exemplo, depois de comparar as posições (0, 1), não existe necessidade de posteriormente comparar (1, 0).

A variável apontada por comparacoes registra quantas comparações entre elementos foram efetivamente realizadas.

Se dois elementos iguais forem encontrados, a função retorna true. Caso todos os pares sejam analisados sem encontrar uma duplicata, a função retorna false.

## Melhor caso e parada antecipada

O melhor caso ocorre quando a duplicata está presente logo no primeiro par analisado pelo algoritmo.

No programa, o vetor utilizado para representar esse cenário é:

[0, 0, 2, 3, 4, 5, 6, 7, 8, 9]

A primeira comparação realizada é:

vetor[0] == vetor[1]
0 == 0

Como os dois valores são iguais, a duplicata é identificada imediatamente.

Nesse momento é executado:

return true;

O return encerra imediatamente a execução da função, portanto nenhuma outra comparação entre elementos é necessária.

Esse comportamento é chamado de parada antecipada (early stopping ou early return).

Dessa forma, no melhor caso são realizadas apenas:

1 comparação

Considerando a comparação entre elementos como a operação fundamental analisada, o melhor caso apresenta comportamento constante:

Ω(1)

Isso significa que, no cenário mais favorável, a quantidade de comparações necessária para encontrar uma duplicata não cresce com o tamanho do vetor.

## Pior caso

O pior caso utilizado pelo programa é representado pelo vetor:

[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

Como não existem elementos repetidos, a condição:

vetor[i] == vetor[j]

nunca é verdadeira.

Consequentemente, a parada antecipada não acontece e o algoritmo precisa analisar todos os pares possíveis.

Para um vetor contendo n elementos, a quantidade de comparações é:

(n - 1) + (n - 2) + ... + 2 + 1

Essa soma pode ser representada pela fórmula:

n(n - 1) / 2

Para o vetor utilizado no programa, temos n = 10:

10(10 - 1) / 2
10 × 9 / 2
90 / 2
45

Portanto, são realizadas:

45 comparações

O número de comparações cresce quadraticamente em função do tamanho do vetor. Assim, a complexidade do pior caso é:

Θ(n²)


## Casos demonstrados

O programa executa dois vetores de dez elementos, definidos em `src/main.c`:

| Caso | Entrada | Comparações esperadas |
|---|---|---:|
| Melhor | Valores iguais nas posições 0 e 1 | 1 |
| Pior | Todos os valores distintos | 45 |

A saída mostra os vetores, a existência de duplicatas e as comparações obtidas. Também informa se a detecção e a contagem correspondem ao esperado.


# Testes com diferentes tamanhos de vetores

Para analisar o comportamento do algoritmo, foram considerados diferentes tamanhos de vetores, permitindo observar como a quantidade de comparações varia conforme o número de elementos.

## Foram considerados dois cenários:

Melhor caso: a duplicata está no primeiro par examinado, permitindo que o algoritmo encerre a busca após uma comparação.
Pior caso: todos os elementos são distintos, obrigando o algoritmo a examinar todos os pares possíveis.
Tabela de comparações

No pior caso, a quantidade de comparações é calculada pela fórmula:

𝐶(𝑛)=𝑛(𝑛−1)


A tabela apresenta os valores esperados para diferentes tamanhos de vetores.

Tamanho do vetor	Melhor caso	Pior caso
2	1	1
5	1	10
10	1	45
20	1	190
50	1	1.225
100	1	4.950
500	1	124.750
1.000	1	499.500

Observação: os valores apresentados são resultados teóricos esperados, calculados com base na lógica do algoritmo. A implementação atual demonstra diretamente os casos com vetores de tamanho 10. Os demais tamanhos são utilizados para ilustrar o comportamento previsto do algoritmo.

## Análise dos resultados

Os resultados mostram que o número de comparações depende da posição em que a duplicata é encontrada e do tamanho do vetor.

No melhor caso, a duplicata aparece logo na primeira comparação. O algoritmo identifica os valores iguais e encerra a execução por meio do comando return true. Assim, é realizada apenas uma comparação entre elementos, independentemente do tamanho do vetor, desde que ele possua pelo menos dois elementos e a duplicata esteja no primeiro par examinado.

No pior caso, todos os elementos são distintos. Como nenhuma duplicata é encontrada, o algoritmo precisa comparar todos os pares possíveis antes de retornar false.

Por exemplo, para um vetor de 10 elementos, são realizadas 45 comparações. Para um vetor de 100 elementos, esse número aumenta para 4.950 comparações.

Esse comportamento demonstra a diferença entre os cenários analisados: considerando a quantidade de comparações entre elementos, o melhor caso apresenta complexidade 
Θ(1), enquanto o pior caso apresenta complexidade 
Θ(𝑛2).

## Conclusão dos testes

A análise permite compreender como a parada antecipada influencia o desempenho do algoritmo. Quando uma duplicata é encontrada rapidamente, a quantidade de comparações é reduzida. Quando não existem elementos repetidos, todos os pares precisam ser examinados.

Portanto, a contagem de comparações permite analisar o comportamento do algoritmo e compreender como o aumento do tamanho do vetor influencia a quantidade de operações realizadas.


# Análise teórica da complexidade

Esta seção apresenta a análise matemática do algoritmo de detecção de elementos duplicados, considerando a quantidade de comparações entre elementos como operação fundamental.

## 1. Dedução da soma de passos no pior caso

O pior caso ocorre quando todos os elementos do vetor são distintos. Nessa situação, o algoritmo precisa examinar todos os pares possíveis, pois nenhuma duplicata é encontrada.

Para um vetor de tamanho n, o primeiro elemento é comparado com os n - 1 elementos seguintes. O segundo é comparado com os n - 2 seguintes, e assim sucessivamente, até a última comparação.

A quantidade total de comparações é representada pela soma:

C(n) = (n - 1) + (n - 2) + ... + 2 + 1

Reorganizando os termos:

C(n) = 1 + 2 + 3 + ... + (n - 2) + (n - 1)

Essa expressão corresponde à soma dos primeiros n - 1 números naturais positivos. Utilizando a fórmula da soma de uma progressão aritmética:

S = (primeiro termo + último termo) × quantidade de termos / 2

Temos:

Primeiro termo: 1
Último termo: n - 1
Quantidade de termos: n - 1

Substituindo os valores:

C(n) = (1 + (n - 1)) × (n - 1) / 2

Simplificando:

C(n) = n × (n - 1) / 2

Portanto, a quantidade de comparações no pior caso é C(n) = n(n - 1)/2.

Desenvolvendo a expressão:

C(n) = (n² - n)/2 = n²/2 - n/2

O termo dominante é n², indicando crescimento quadrático.

## 2. Prova de que o pior caso pertence a O(n²)

A notação Big O representa um limite superior assintótico para o crescimento da quantidade de operações.

Formalmente, uma função pertence a O(n²) quando existem constantes positivas c e n₀ tais que 0 ≤ C(n) ≤ c × n² para todo n ≥ n₀.

No pior caso, temos:

C(n) = n(n - 1)/2

Como n - 1 ≤ n para n ≥ 1, podemos escrever:

C(n) = n(n - 1)/2 ≤ n²/2

Escolhendo c = 1/2 e n₀ = 1, a condição formal é satisfeita. Portanto, o algoritmo pertence a O(n²) no pior caso.

Isso significa que a quantidade de comparações cresce, no máximo, proporcionalmente ao quadrado do tamanho do vetor, desconsiderando constantes multiplicativas e termos de menor ordem.

## 3. Prova de que o melhor caso pertence a Ω(1)

A notação Big Omega representa um limite inferior assintótico.

No melhor caso, a duplicata está nas posições 0 e 1. Para vetores com pelo menos dois elementos, o algoritmo realiza uma comparação e encerra imediatamente a execução.

Assim, a quantidade de comparações no melhor caso é C_melhor(n) = 1, para n ≥ 2.

Formalmente, uma função pertence a Ω(1) quando existem constantes positivas c e n₀ tais que C_melhor(n) ≥ c para todo n ≥ n₀.

Escolhendo c = 1 e n₀ = 2, temos `C_melhor
