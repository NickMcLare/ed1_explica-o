# Explicação do trabalho de Estrutura de Dados
## Estrutura geral do código:
O código tem três partes principais:

1. **Entrada de dados e alocação de memória**:

* Receber as dimensões e elementos das matrizes, e alocar memória para armazená-las.
* Multiplicação das matrizes: Realizar a multiplicação de duas matrizes, conforme as regras de álgebra linear.
Saída e liberação de memória: Mostrar o resultado e liberar a memória dinamicamente alocada.

```
printf("Digite o número de linhas e colunas da matriz A: ");
scanf("%d %d", &linhasA, &colunasA);

printf("Digite o número de linhas e colunas da matriz B: ");
scanf("%d %d", &linhasB, &colunasB);
```

* O usuário insere as dimensões de A (linhasA, colunasA) e B (linhasB, colunasB). Isso define quantas linhas e colunas cada matriz terá.
* A multiplicação de matrizes só é possível quando o número de colunas de A é igual ao número de linhas de B. O código verifica essa condição antes de prosseguir:

```
if (colunasA != linhasB) {
    printf("Erro: As matrizes não podem ser multiplicadas.\n");
    return 1;
}
```
* Se a condição não for atendida, o programa exibe uma mensagem de erro e termina.

**Alocar memória dinamicamente para as matrizes:**

* A seguir, o código aloca dinamicamente a memória para as três matrizes (A, B e resultado).

```
int** A = (int**)malloc(linhasA * sizeof(int*));
int** B = (int**)malloc(linhasB * sizeof(int*));
int** resultado = (int**)malloc(linhasA * sizeof(int*));

for (int i = 0; i < linhasA; i++)
    A[i] = (int*)malloc(colunasA * sizeof(int));

for (int i = 0; i < linhasB; i++)
    B[i] = (int*)malloc(colunasB * sizeof(int));

for (int i = 0; i < linhasA; i++)
    resultado[i] = (int*)malloc(colunasB * sizeof(int));
```

* **Primeira etapa:** Para cada uma das matrizes (A, B e resultado), a memória é alocada dinamicamente.
  * A e B são matrizes de inteiros (int**), ou seja, um ponteiro para um array de ponteiros.
  * resultado é a matriz que armazenará o produto das outras duas matrizes.
* Para cada linha das matrizes A, B, e resultado, o código aloca um array de inteiros para armazenar as colunas correspondentes.

**Preencher as matrizes com os valores do usuário:**

* Agora, o usuário insere os valores para as matrizes:

```
printf("Digite os elementos da matriz A:\n");
for (int i = 0; i < linhasA; i++) {
    for (int j = 0; j < colunasA; j++) {
        printf("A[%d][%d]: ", i, j);
        scanf("%d", &A[i][j]);
    }
}

printf("Digite os elementos da matriz B:\n");
for (int i = 0; i < linhasB; i++) {
    for (int j = 0; j < colunasB; j++) {
        printf("B[%d][%d]: ", i, j);
        scanf("%d", &B[i][j]);
    }
}
```
* Esses loops aninhados preenchem as matrizes A e B com os valores inseridos pelo usuário.

2. **Multiplicação das matrizes**

A multiplicação de matrizes segue a fórmula matemática:
* O elemento na posição resultado[i][j] é a soma do produto dos elementos correspondentes de A e B.
```
for (int i = 0; i < linhasA; i++) {
    for (int j = 0; j < colunasB; j++) {
        resultado[i][j] = 0;  // Inicializa a posição com zero
        for (int k = 0; k < colunasA; k++) {
            resultado[i][j] += A[i][k] * B[k][j];  // Multiplica e acumula
        }
    }
}
```
Para cada linha i da matriz A, e para cada coluna j da matriz B:
* Inicializa resultado[i][j] como 0.
* Em seguida, percorre o loop interno k, que percorre as colunas de A e as linhas de B.
* Multiplica A[i][k] por B[k][j] e soma ao valor atual de resultado[i][j].

No final desse loop, a posição resultado[i][j] terá o valor correto para o produto das duas matrizes.

3. **Saída e liberação de memória**

Após a multiplicação, o programa imprime a matriz resultado:
```
printf("Resultado da multiplicação:\n");
for (int i = 0; i < linhasA; i++) {
    for (int j = 0; j < colunasB; j++) {
        printf("%d ", resultado[i][j]);
    }
    printf("\n");
}
```
* Este loop aninhado percorre a matriz resultado e imprime os valores linha por linha.

**Liberação da memória alocada:**

* Como foi alocada memória dinamicamente, é importante liberar essa memória ao final do programa para evitar vazamentos de memória. Isso é feito da seguinte forma:
```
for (int i = 0; i < linhasA; i++)
    free(A[i]);
for (int i = 0; i < linhasB; i++)
    free(B[i]);
for (int i = 0; i < linhasA; i++)
    free(resultado[i]);

free(A);
free(B);
free(resultado);
```
* Esse código percorre cada linha de A, B e resultado e libera a memória alocada para cada linha. Em seguida, libera a memória alocada para os arrays de ponteiros (A, B, resultado).

**Resumo do funcionamento:**
1. Entrada de dimensões: O usuário fornece as dimensões das matrizes.
2. Alocação dinâmica de memória: O código aloca a memória para as matrizes com base nas dimensões.
3. Entrada de valores: O usuário preenche as matrizes com os valores.
4. Multiplicação de matrizes: O código multiplica as matrizes usando a fórmula padrão de multiplicação de matrizes.
5. Exibição do resultado: O programa imprime a matriz resultante.
6. Liberação de memória: O programa libera a memória dinamicamente alocada.

### Código completo:
```
#include <stdio.h>
#include <stdlib.h>

void multiplicarMatrizes(int** A, int linhasA, int colunasA, int** B, int linhasB, int colunasB, int** resultado) {
    // Verifica se as matrizes podem ser multiplicadas
    if (colunasA != linhasB) {
        printf("Erro: As matrizes não podem ser multiplicadas.\n");
        printf("Motivo: O número de colunas da matriz A (%d) não é igual ao número de linhas da matriz B (%d).\n", colunasA, linhasB);
        return;
    }

    // Realiza a multiplicação
    for (int i = 0; i < linhasA; i++) {
        for (int j = 0; j < colunasB; j++) {
            resultado[i][j] = 0;  // Inicializa a posição
            for (int k = 0; k < colunasA; k++) {
                resultado[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}

int main() {
    int linhasA, colunasA, linhasB, colunasB;

    // Solicita ao usuário as dimensões da matriz A
    printf("Digite o número de linhas e colunas da matriz A: ");
    scanf("%d %d", &linhasA, &colunasA);

    // Solicita ao usuário as dimensões da matriz B
    printf("Digite o número de linhas e colunas da matriz B: ");
    scanf("%d %d", &linhasB, &colunasB);

    // Verifica se as dimensões são compatíveis para multiplicação
    if (colunasA != linhasB) {
        printf("Erro: As matrizes não podem ser multiplicadas.\n");
        printf("Motivo: O número de colunas da matriz A (%d) não é igual ao número de linhas da matriz B (%d).\n", colunasA, linhasB);
        return 1;  // Finaliza o programa se as matrizes não puderem ser multiplicadas
    }

    // Alocação dinâmica para as matrizes A, B e resultado
    int** A = (int**)malloc(linhasA * sizeof(int*));
    int** B = (int**)malloc(linhasB * sizeof(int*));
    int** resultado = (int**)malloc(linhasA * sizeof(int*));

    for (int i = 0; i < linhasA; i++)
        A[i] = (int*)malloc(colunasA * sizeof(int));

    for (int i = 0; i < linhasB; i++)
        B[i] = (int*)malloc(colunasB * sizeof(int));

    for (int i = 0; i < linhasA; i++)
        resultado[i] = (int*)malloc(colunasB * sizeof(int));

    // Entrada de dados para a matriz A
    printf("Digite os elementos da matriz A:\n");
    for (int i = 0; i < linhasA; i++) {
        for (int j = 0; j < colunasA; j++) {
            printf("A[%d][%d]: ", i, j);
            scanf("%d", &A[i][j]);
        }
    }

    // Entrada de dados para a matriz B
    printf("Digite os elementos da matriz B:\n");
    for (int i = 0; i < linhasB; i++) {
        for (int j = 0; j < colunasB; j++) {
            printf("B[%d][%d]: ", i, j);
            scanf("%d", &B[i][j]);
        }
    }

    // Multiplicação das matrizes
    multiplicarMatrizes(A, linhasA, colunasA, B, linhasB, colunasB, resultado);

    // Exibição da matriz resultado
    printf("Resultado da multiplicação:\n");
    for (int i = 0; i < linhasA; i++) {
        for (int j = 0; j < colunasB; j++) {
            printf("%d ", resultado[i][j]);
        }
        printf("\n");
    }

    // Liberação da memória alocada
    for (int i = 0; i < linhasA; i++)
        free(A[i]);
    for (int i = 0; i < linhasB; i++)
        free(B[i]);
    for (int i = 0; i < linhasA; i++)
        free(resultado[i]);

    free(A);
    free(B);
    free(resultado);

    return 0;
}

```
