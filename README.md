# 42SP-C03-Tutorial
# C03 - Tutorial de Exercícios da 42

Este documento apresenta um guia detalhado para a implementação das funções do projeto C03 da 42. Aqui, cada função deve ser criada de forma idêntica às originais da biblioteca padrão do C, respeitando os protótipos abaixo (incluindo tabulações e espaçamentos). Cada seção a seguir traz explicações simples e exemplos de uso.

## Protótipos das Funções

```c
00	int        ft_strcmp(char *s1, char *s2);
01	int        ft_strncmp(char *s1, char *s2, unsigned int n);
02	char	*ft_strcat(char *dest, char *src);
03	char	*ft_strncat(char *dest, char *src, unsigned int nb);
04	char	*ft_strstr(char *str, char *to_find);
05	unsigned int	ft_strlcat(char *dest, char *src, unsigned int size);
```

> **Observação:** Mantenha exatamente os espaços e tabulações conforme indicado, para corresponder aos protótipos esperados pelo teste.

## Estrutura do Projeto

1. Crie um diretório `C03/` dentro de sua pasta de trabalho.
2. Dentro de `C03/`, crie um arquivo fonte para cada função:

   * `ft_strcmp.c`
   * `ft_strncmp.c`
   * `ft_strcat.c`
   * `ft_strncat.c`
   * `ft_strstr.c`
   * `ft_strlcat.c`
3. Crie também um `Makefile` simples para compilar todas as implementações em uma única biblioteca estática `libft.a` (ou apenas testar individualmente).

## Como Compilar

Um exemplo de regra no `Makefile`:

```makefile
NAME = libft.a
CC = gcc
CFLAGS = -Wall -Wextra -Werror
SRCS = ft_strcmp.c ft_strncmp.c ft_strcat.c ft_strncat.c ft_strstr.c ft_strlcat.c
OBJS = $(SRCS:.c=.o)

all: $(NAME)

$(NAME): $(OBJS)
	ar rcs $(NAME) $(OBJS)

clean:
	rm -f $(OBJS)

fclean: clean
	rm -f $(NAME)

re: fclean all
```

Compilando manualmente:

```
$ gcc -Wall -Wextra -Werror -c *.c
```

## Explicação das Funções

### 00 - `ft_strcmp`

```c
int        ft_strcmp(char *s1, char *s2);
```

**Descrição:** Compara duas strings caracter a caracter.

* **Parâmetros:**

  * `s1`: ponteiro para a primeira string.
  * `s2`: ponteiro para a segunda string.
* **Retorno:**

  * Um valor inteiro:

    * < 0 se `s1` for lexicograficamente menor que `s2`.
    * \= 0 se `s1` for igual a `s2`.
    * > 0 se `s1` for maior.

**Como funciona:**

1. Percorre `s1` e `s2` simultaneamente.
2. Se encontrar um caractere diferente ou `\0`, retorna `(unsigned char)s1[i] - (unsigned char)s2[i]`.
3. Se terminar ambas iguais, retorna 0.

**Exemplo de uso:**

```c
#include <stdio.h>

int main() {
    printf("%d\n", ft_strcmp("abc", "abd")); // < 0
    printf("%d\n", ft_strcmp("42", "42"));   // 0
    printf("%d\n", ft_strcmp("bee", "be"));  // > 0
}
```

---

### 01 - `ft_strncmp`

```c
int        ft_strncmp(char *s1, char *s2, unsigned int n);
```

**Descrição:** Compara até `n` caracteres de duas strings.

* **Parâmetros:**

  * `s1`, `s2` como antes.
  * `n`: número máximo de caracteres a comparar.
* **Retorno:**

  * Mesmo comportamento de `ft_strcmp`, mas limitado a `n` caracteres.

**Como funciona:**

1. Se `n` for 0, retorna 0 imediatamente.
2. Percorre até `i < n`, comparando `s1[i]` e `s2[i]`.
3. Ao encontrar diferença ou `\0` antes de `n`, retorna diferença.
4. Se atingir `n` sem achar diferença, retorna 0.

**Exemplo:**

```c
printf("%d\n", ft_strncmp("abcd", "abzz", 2)); // 0 ("ab" igual)
printf("%d\n", ft_strncmp("abcd", "abzz", 3)); // < 0
```

---

### 02 - `ft_strcat`

```c
char    *ft_strcat(char *dest, char *src);
```

**Descrição:** Anexa (concatena) a string `src` ao final de `dest`.

* **Parâmetros:**

  * `dest`: string destino, deve ter espaço suficiente.
  * `src`: string a ser anexada.
* **Retorno:**

  * Ponteiro para `dest`.

**Como funciona:**

1. Localiza o fim de `dest` (onde está `\0`).
2. Copia caracter por caracter de `src`, incluindo o `\0`, para a partir desse ponto.
3. Retorna `dest`.

**Exemplo:**

```c
char buf[20] = "Hello";
printf("%s\n", ft_strcat(buf, " World!")); // "Hello World!"
```

---

### 03 - `ft_strncat`

```c
char    *ft_strncat(char *dest, char *src, unsigned int nb);
```

**Descrição:** Igual ao `ft_strcat`, mas copia no máximo `nb` caracteres de `src`.

* **Parâmetros:**

  * `dest`, `src` como em `strcat`.
  * `nb`: número máximo de caracteres de `src` a anexar.
* **Retorno:**

  * Ponteiro para `dest`.

**Como funciona:**

1. Encontra o fim de `dest`.
2. Copia até `nb` caracteres de `src` ou até `\0`.
3. Adiciona `\0` no fim de `dest`.
4. Retorna `dest`.

**Exemplo:**

```c
char buf[20] = "Foo";
printf("%s\n", ft_strncat(buf, "BarBaz", 3)); // "FooBar"
```

---

### 04 - `ft_strstr`

```c
char    *ft_strstr(char *str, char *to_find);
```

**Descrição:** Procura a primeira ocorrência de `to_find` em `str`.

* **Parâmetros:**

  * `str`: string onde será feita a busca.
  * `to_find`: string que se quer encontrar.
* **Retorno:**

  * Ponteiro para o início da substring em `str`, ou `NULL` se não encontrar.

**Como funciona:**

1. Se `to_find` for vazia, retorna `str`.
2. Para cada posição `i` em `str`:

   * Compara caractere a caractere com `to_find`.
   * Se todos os caracteres de `to_find` casarem, retorna `&str[i]`.
3. Se não encontrar, retorna `NULL`.

**Exemplo:**

```c
char *h = "O rato roeu a roupa";
char *p = ft_strstr(h, "roeu");
printf("%s\n", p); // "roeu a roupa"
```

---

### 05 - `ft_strlcat`

```c
unsigned int    ft_strlcat(char *dest, char *src, unsigned int size);
```

**Descrição:** Anexa `src` a `dest`, mas copia no máximo `size - strlen(dest) - 1` caracteres, garantindo `\0`. Retorna o tamanho da string que tentou criar: `min(size, strlen(dest) + strlen(src))`.

* **Parâmetros:**

  * `dest`: buffer destino.
  * `src`: string fonte.
  * `size`: tamanho total do buffer em `dest`.
* **Retorno:**

  * Comprimento que a concatenação completa teria (tamanho de `dest` inicial + tamanho de `src`).

**Como funciona:**

1. Calcula `dlen = strlen(dest)` e `slen = strlen(src)`.
2. Se `dlen >= size`, retorna `size + slen`.
3. Copia até `size - dlen - 1` caracteres de `src` para o final de `dest`.
4. Coloca `\0` ao final.
5. Retorna `dlen + slen`.

**Exemplo:**

```c
char buf[10] = "ABCD";
unsigned int r = ft_strlcat(buf, "12345678", 10);
// buf = "ABCD1234" (4 dest + 4 src + '\0')
// r = 4 + 8 = 12 (tamanho total pretendido)
printf("buf: %s, retorno: %u\n", buf, r);
```

---

# Soluções do C03 com Explicações

Este documento apresenta a implementação completa de cada função do projeto C03, seguindo exatamente os protótipos e adicionando comentários que explicam passo a passo o raciocínio.

---

## 00 - `ft_strcmp`

```c
int        ft_strcmp(char *s1, char *s2)
{
    unsigned int i = 0;
    
    // Percorre enquanto caracteres forem iguais e não for fim de string
    while (s1[i] && s2[i] && s1[i] == s2[i])
        i++;
    
    // Retorna diferença entre os bytes (sinaliza ordem lexicográfica)
    return ((unsigned char)s1[i] - (unsigned char)s2[i]);
}
```

**Explicação:**

1. **Índice `i`**: usado para comparar caractere a caractere.
2. **Loop `while`**: continua enquanto `s1[i]` e `s2[i]` não forem `\0` e forem iguais.
3. **Retorno**: quando sair do loop, pode ser porque encontrou fim de string ou caractere diferente. Converte em `unsigned char` para evitar valores negativos inesperados e subtrai.

---

## 01 - `ft_strncmp`

```c
int        ft_strncmp(char *s1, char *s2, unsigned int n)
{
    unsigned int i = 0;

    // Se n for zero, nenhuma comparação é feita
    if (n == 0)
        return (0);
    
    // Percorre até n-1 caracteres ou até diferença/fim de string
    while (i < n - 1 && s1[i] && s2[i] && s1[i] == s2[i])
        i++;
    
    // Retorna diferença no índice atual
    return ((unsigned char)s1[i] - (unsigned char)s2[i]);
}
```

**Explicação:**

1. Se `n` for `0`, devolve `0` imediatamente (nada a comparar).
2. Compara até o menor valor entre `n-1`, fim de string ou caractere desigual.
3. Usa mesma lógica de subtração para indicar ordem/igualdade.

---

## 02 - `ft_strcat`

```c
char    *ft_strcat(char *dest, char *src)
{
    unsigned int i = 0;
    unsigned int j = 0;

    // Avança j até o fim de dest
    while (dest[j])
        j++;
    
    // Copia src, incluindo '\0', para o fim de dest
    while (src[i])
    {
        dest[j + i] = src[i];
        i++;
    }
    dest[j + i] = '\0';   // finaliza string
    
    return (dest);
}
```

**Explicação:**

1. **Encontra fim de `dest`**: percorre `dest` com `j` até `\0`.
2. **Copia `src`**: com índice `i`, copia caractere por caractere para `dest[j + i]`.
3. **Adicionar termoiante `\0`**: garante que `dest` continue válida.

---

## 03 - `ft_strncat`

```c
char    *ft_strncat(char *dest, char *src, unsigned int nb)
{
    unsigned int i = 0;
    unsigned int j = 0;

    // Posição final de dest
    while (dest[j])
        j++;
    
    // Copia até nb caracteres ou até fim de src
    while (i < nb && src[i])
    {
        dest[j + i] = src[i];
        i++;
    }
    dest[j + i] = '\0';
    
    return (dest);
}
```

**Explicação:**

1. Igual ao `strcat`, mas o loop de cópia para quando `i == nb` ou `src[i] == '\0'`.
2. Garante não ultrapassar `nb` caracteres.

---

## 04 - `ft_strstr`

```c
char    *ft_strstr(char *str, char *to_find)
{
    unsigned int i;
    unsigned int j;

    // Se to_find é vazio, retorne início de str
    if (!to_find[0])
        return (str);

    i = 0;
    while (str[i])
    {
        j = 0;
        // Verifica se substring casa começando em i
        while (to_find[j] && str[i + j] == to_find[j])
            j++;
        
        // Se chegou ao fim de to_find, encontrou
        if (!to_find[j])
            return (&str[i]);
        i++;
    }
    return (NULL);  // não encontrou
}
```

**Explicação:**

1. **Caso especial**: `to_find` vazio sempre casaria no início.
2. Percorre `str` com `i`. Para cada `i`, zera `j` e compara `to_find[j]` com `str[i+j]`.
3. Se `to_find[j]` chegar a `\0`, toda a substring casou e retorna ponteiro `&str[i]`.
4. Se terminar `str` sem casamentos, retorna `NULL`.

---

## 05 - `ft_strlcat`

```c
unsigned int    ft_strlcat(char *dest, char *src, unsigned int size)
{
    unsigned int dlen = 0;
    unsigned int slen = 0;
    unsigned int i;

    // Calcula comprimento de dest e src
    while (dest[dlen] && dlen < size)
        dlen++;
    while (src[slen])
        slen++;

    // Se buffer menor que comprimento inicial de dest,
    // retorno é tamanho de src + size
    if (dlen == size)
        return (size + slen);

    // Copia até caber em size-1
    i = 0;
    while (src[i] && (dlen + i + 1) < size)
    {
        dest[dlen + i] = src[i];
        i++;
    }
    dest[dlen + i] = '\0';

    return (dlen + slen);
}
```

**Explicação:**

1. **`dlen`**: percorre `dest` até `\0` ou até `size` (para não ler fora do buffer).
2. **`slen`**: tamanho completo de `src`.
3. Se `dlen == size`, não há espaço, retorna `size + slen`, o tamanho que tentaria criar.
4. Copia `src` até cenário de lotação: `(dlen + i + 1) < size` garante espaço para `\0`.
5. Retorna `dlen + slen`, o tamanho total pretendido.

---

> Com essas implementações, você terá uma `libft` compatível com os testes do C03 da 42, com todas as funções comportando-se exatamente como as originais da biblioteca padrão C. Boa codificação!


