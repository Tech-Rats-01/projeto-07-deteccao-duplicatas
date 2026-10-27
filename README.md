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
