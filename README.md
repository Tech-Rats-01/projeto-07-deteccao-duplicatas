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

## Casos demonstrados

O programa executa dois vetores de dez elementos, definidos em `src/main.c`:

| Caso | Entrada | Comparações esperadas |
|---|---|---:|
| Melhor | Valores iguais nas posições 0 e 1 | 1 |
| Pior | Todos os valores distintos | 45 |

A saída mostra os vetores, a existência de duplicatas e as comparações obtidas. Também informa se a detecção e a contagem correspondem ao esperado.
