# ft\_strcmp Otimizado

Este documento apresenta uma versão otimizada e simplificada da função `ft_strcmp`, comparando duas strings em C. Inclui código atualizado em C e uma explicação detalhada das alterações.

---

## 1. Código Otimizado

```c
#include <stddef.h>

int ft_strcmp(char *s1, char *s2)
{
    /* Converte ponteiros para unsigned char apenas uma vez */
    const unsigned char *p1 = (const unsigned char *)s1;
    const unsigned char *p2 = (const unsigned char *)s2;

    /* Avança enquanto não chegar no fim e os bytes forem iguais */
    while (*p1 && (*p1 == *p2)) {
        p1++;
        p2++;
    }

    /* Retorna a diferença entre os bytes onde parou */
    return *p1 - *p2;
}
```

---

## 2. Melhorias Implementadas

1. **Ponteiros em vez de índice**

   * Eliminamos a variável de índice (`i`) e acessos `s1[i]`/`s2[i]`.
   * Substituímos por aritmética de ponteiros (`p1++`, `p2++`), reduzindo instruções de cálculo de endereço.

2. **Cast único**

   * Convertendo `s1` e `s2` para `unsigned char *` logo no início, garantimos que comparações e subtrações usem valores de 0 a 255, sem necessidade de múltiplos casts.

3. **Loop enxuto**

   * A condição `*p1 && (*p1 == *p2)` combina a verificação de fim de string e a igualdade de bytes em um único teste.

4. **Retorno direto**

   * Ao sair do loop, `*p1 - *p2` já representa a diferença lexicográfica desejada, evitando cálculos adicionais.

---

## 3. Exemplo de Uso (main.c)

Para testar a função `ft_strcmp`, crie um arquivo `main.c` com o seguinte conteúdo:

```c
#include <stdio.h>

int ft_strcmp(char *s1, char *s2)
{
    const unsigned char *p1 = (const unsigned char *)s1;
    const unsigned char *p2 = (const unsigned char *)s2;

    while (*p1 && (*p1 == *p2)) {
        p1++;
        p2++;
    }

    return *p1 - *p2;
}

int main(void)
{
    /* Casos de teste */
    char a[] = "hello";
    char b[] = "hello";
    char c[] = "hell";
    char d[] = "help";
    char e[] = "Hello";

    /* Testes */
    printf("Comparando \"%s\" com \"%s\" → %d\n", a, b, ft_strcmp(a, b)); /* 0 */
    printf("Comparando \"%s\" com \"%s\" → %d\n", a, c, ft_strcmp(a, c)); /* 'o' - '\0' > 0 */
    printf("Comparando \"%s\" com \"%s\" → %d\n", a, d, ft_strcmp(a, d)); /* 'l' - 'p' < 0 */
    printf("Comparando \"%s\" com \"%s\" → %d\n", a, e, ft_strcmp(a, e)); /* 'h' - 'H' > 0 */

    return 0;
}
```

---

## 4. Resultados Esperados

```
Comparando "hello" com "hello" → 0
Comparando "hello" com "hell"  → 111
Comparando "hello" com "help"  → -4
Comparando "hello" com "Hello" → 32
```

---

## 5. Explicação dos Resultados

1. **Comparando `"hello"` com `"hello"` → `0`**
   As duas strings são idênticas, logo não há diferença de bytes.

2. **Comparando `"hello"` com `"hell"` → `111`**

   * Percorremos até o quarto caractere (`'l'`).
   * No quinto passo, `p1` aponta para `'o'` (ASCII 111) e `p2` para `'\0'` (ASCII 0), resultando em `111 - 0`.

3. **Comparando `"hello"` com `"help"` → `-4`**

   * As strings divergem na quarta posição: `'l'` (ASCII 108) vs. `'p'` (ASCII 112), `108 - 112 = -4`.

4. **Comparando `"hello"` com `"Hello"` → `32`**

   * Diferença entre `'h'` (ASCII 104) e `'H'` (ASCII 72), `104 - 72 = 32`.
