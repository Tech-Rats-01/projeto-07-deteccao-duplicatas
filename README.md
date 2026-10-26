# Projeto 07 - Detecção de elementos duplicados

Programa em C que verifica se um vetor contém valores repetidos e conta as comparações realizadas até encontrar uma duplicata ou terminar a busca.

## Funcionamento

Cada elemento é comparado com os seguintes usando dois laços `for`. O segundo laço começa em `j = i + 1`, evitando comparar uma posição consigo mesma ou repetir um par.

Quando encontra dois valores iguais, a função encerra a busca e retorna `true`. Se terminar sem encontrar repetição, retorna `false`.

O contador é zerado a cada chamada e considera apenas as comparações entre os elementos. O vetor recebido permanece inalterado.

## Arquivos

- `src/duplicatas.c`: implementação do algoritmo.
- `src/duplicatas.h`: declaração da função e suas condições de uso.
- `src/main.c`: execução dos exemplos de melhor e pior caso.

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
