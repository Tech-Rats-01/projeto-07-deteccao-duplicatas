# Projeto 07 — Detecção de elementos duplicados

Programa em C que verifica se um vetor contém valores repetidos e conta as comparações realizadas até encontrar uma duplicata ou terminar a busca.

## Objetivos

Implementar a detecção de duplicatas por comparação de pares e analisar como a parada antecipada afeta a quantidade de comparações.

O projeto considera dois cenários:

- Melhor caso: valores iguais nas posições 0 e 1.
- Pior caso: todos os elementos distintos.

## Tecnologias utilizadas

- Linguagem C.
- GCC para compilação.
- MSYS2 UCRT64 como ambiente de compilação no Windows.

## Pré-requisitos

As instruções abaixo utilizam o GCC instalado pelo MSYS2.

### 1. Instalar o MSYS2

Baixe o instalador no [site oficial do MSYS2](https://www.msys2.org/) e siga as instruções de instalação e configuração inicial.

### 2. Instalar o GCC

Abra o terminal **MSYS2 UCRT64** pelo menu Iniciar e execute:

```bash
pacman -S --needed mingw-w64-ucrt-x86_64-gcc
```

Se aparecer um pedido de confirmação, pressione `Y` e depois `Enter`.

### 3. Configurar o PATH

Para utilizar o GCC pelo PowerShell ou pelo terminal do VS Code:

1. Pesquise no menu Iniciar por **Editar as variáveis de ambiente do sistema**.
2. Clique em **Variáveis de Ambiente**.
3. Na seção **Variáveis de usuário**, selecione `Path` e clique em **Editar**.
4. Clique em **Novo** e adicione:

   ```text
   C:\msys64\ucrt64\bin
   ```

5. Confirme as alterações.

Esse caminho considera a instalação do MSYS2 em `C:\msys64`. Se estiver em outra pasta, ajuste o caminho.

### 4. Verificar a instalação

Abra um novo terminal e execute:

```powershell
gcc --version
```

O comando deve mostrar a versão instalada do GCC. Se estiver utilizando o terminal integrado do VS Code, feche e abra novamente o editor para carregar o PATH atualizado.

## Arquivos

- `src/duplicatas.c`: implementação do algoritmo.
- `src/duplicatas.h`: declaração da função e suas condições de uso.
- `src/main.c`: execução dos exemplos de melhor e pior caso.
- `README.md`: descrição do projeto e instruções de execução.

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

A função recebe o vetor, seu tamanho e o endereço de um contador:

```c
bool tem_duplicado(const int vetor[], size_t n, uint64_t *comparacoes);
```

O primeiro laço seleciona um elemento pela posição `i`. O segundo percorre as posições seguintes usando `j`, que começa em `i + 1`.

A implementação segue esta estrutura:

```c
bool tem_duplicado(const int vetor[], size_t n, uint64_t *comparacoes)
{
    *comparacoes = 0;

    for (size_t i = 0; i + 1 < n; i++) {
        for (size_t j = i + 1; j < n; j++) {
            (*comparacoes)++;

            if (vetor[i] == vetor[j]) {
                return true;
            }
        }
    }

    return false;
}
```

Como `j` começa na posição seguinte a `i`, o algoritmo não compara uma posição consigo mesma nem repete um par na ordem inversa. Depois de comparar as posições `(0, 1)`, por exemplo, não precisa comparar `(1, 0)`.

O contador é zerado a cada chamada e registra somente as comparações entre os elementos. Os testes que controlam os laços não entram nessa contagem.

Quando encontra dois valores iguais, `return true` encerra a função inteira. Se terminar a busca sem encontrar repetição, a função retorna `false`.

O vetor recebido permanece inalterado. Para vetores vazios ou com um único elemento, a função retorna `false` com zero comparações.

## Casos demonstrados pelo programa

O arquivo `src/main.c` executa dois vetores de dez elementos:

| Caso | Vetor | Comparações |
|---|---|---:|
| Melhor | `[0, 0, 2, 3, 4, 5, 6, 7, 8, 9]` | 1 |
| Pior | `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]` | 45 |

A saída mostra os vetores, a existência de duplicatas e as comparações obtidas. Também verifica se a detecção e a contagem correspondem ao esperado.

### Melhor caso e parada antecipada

No primeiro vetor, a primeira comparação verifica:

```c
vetor[0] == vetor[1]
```

Os dois valores são iguais a zero. A função identifica a duplicata e executa `return true`, encerrando a busca após uma comparação.

Esse comportamento é chamado de parada antecipada. Para qualquer vetor com pelo menos dois elementos e uma duplicata nas posições 0 e 1, o algoritmo realiza apenas uma comparação entre valores.

### Pior caso

No segundo vetor, todos os valores são distintos. Nenhuma comparação encontra uma igualdade, então o algoritmo examina todos os pares antes de retornar `false`.

Para dez elementos:

```text
C(10) = 10 × (10 − 1) / 2
      = 10 × 9 / 2
      = 45 comparações
```

## Comparações esperadas para diferentes tamanhos de vetores

A tabela considera os mesmos cenários:

- Melhor caso: duplicata nas posições 0 e 1.
- Pior caso: todos os elementos distintos.

No pior caso, a quantidade esperada de comparações é:

```text
C(n) = n(n − 1) / 2
```

| Tamanho do vetor | Melhor caso | Pior caso |
|---:|---:|---:|
| 2 | 1 | 1 |
| 5 | 1 | 10 |
| 10 | 1 | 45 |
| 20 | 1 | 190 |
| 50 | 1 | 1.225 |
| 100 | 1 | 4.950 |
| 500 | 1 | 124.750 |
| 1.000 | 1 | 499.500 |

Os valores desta tabela são teóricos, calculados pela fórmula. O programa em `src/main.c` executa os dois casos com vetores de tamanho 10.

No melhor caso, a quantidade de comparações permanece igual a um. No pior caso, ela aumenta com o tamanho do vetor: passa de 45 comparações para 10 elementos a 4.950 comparações para 100 elementos.

## Análise teórica da complexidade

A operação fundamental analisada é a comparação entre dois valores do vetor.

### Dedução da quantidade de comparações no pior caso

Quando todos os elementos são distintos:

- O elemento da posição 0 é comparado com os `n − 1` seguintes.
- O elemento da posição 1 é comparado com os `n − 2` seguintes.
- O processo continua até o penúltimo elemento, que é comparado somente com o último.

Para `n ≥ 2`, a soma das comparações é:

```text
C(n) = (n − 1) + (n − 2) + ... + 2 + 1
```

Reorganizando os termos:

```text
C(n) = 1 + 2 + ... + (n − 2) + (n − 1)
```

Essa é uma progressão aritmética com primeiro termo igual a 1, último termo igual a `n − 1` e quantidade de termos igual a `n − 1`.

Aplicando a fórmula da soma:

```text
S = (primeiro termo + último termo) × quantidade de termos / 2

C(n) = (1 + (n − 1)) × (n − 1) / 2
C(n) = n(n − 1) / 2
```

Desenvolvendo a expressão:

```text
C(n) = (n² − n) / 2
C(n) = n²/2 − n/2
```

O termo dominante é `n²`, indicando crescimento quadrático.

### Limite superior do pior caso — O(n²)

Para demonstrar que `C(n)` pertence a `O(n²)`, precisamos encontrar constantes positivas `c` e `n₀` tais que:

```text
0 ≤ C(n) ≤ c · n², para todo n ≥ n₀
```

Como `n − 1 ≤ n` para `n ≥ 1`:

```text
C(n) = n(n − 1) / 2 ≤ n²/2
```

Escolhendo `c = 1/2` e `n₀ = 1`, a condição é satisfeita. Portanto, o pior caso pertence a `O(n²)`.

Também podemos estabelecer um limite firme. Para `n ≥ 2`, temos `n − 1 ≥ n/2`, de modo que:

```text
n²/4 ≤ C(n) ≤ n²/2
```

Assim, a quantidade de comparações no pior caso pertence a `Θ(n²)`.

### Limite inferior do melhor caso — Ω(1)

No melhor caso, a duplicata aparece nas posições 0 e 1. Para vetores com pelo menos dois elementos:

```text
C_melhor(n) = 1, para n ≥ 2
```

Para demonstrar que essa função pertence a `Ω(1)`, precisamos encontrar constantes positivas `c` e `n₀` tais que:

```text
C_melhor(n) ≥ c · 1, para todo n ≥ n₀
```

Escolhendo `c = 1` e `n₀ = 2`:

```text
C_melhor(n) = 1 ≥ 1 · 1, para todo n ≥ 2
```

Portanto, o melhor caso pertence a `Ω(1)`. Como a quantidade de comparações é exatamente um nesse cenário, também pertence a `Θ(1)`.

A parada antecipada permite encerrar a busca assim que uma duplicata aparece. Quando todos os elementos são distintos, o algoritmo percorre todos os pares e realiza `n(n − 1)/2` comparações.
