## Tutorial de Funções - C 04 (42 São Paulo)

Este guia traz explicações detalhadas e comentadas de cada função dos exercícios **ex00** a **ex05**, com descrições dos objetivos, funções autorizadas, trechos de código comentados e dicas para facilitar o entendimento.

---

### ex00: `ft_strlen`

**Descrição do exercício:**
Nesse exercício, você deve implementar a função **ft\_strlen**, que recebe um ponteiro para o início de uma string e conta quantos caracteres ela contém antes do terminador nulo (`\0`), sem utilizar nenhuma função de biblioteca.

**Funções autorizadas:** nenhuma

```c
int ft_strlen(char *str)
{
    int len = 0;               /* inicializa contador de caracteres */

    while (str[len])           /* percorre a string até encontrar '\0' */
        len++;                 /* incrementa o contador a cada caractere */

    return len;                /* retorna o número total de caracteres */
}
```

**Dicas e observações:**

* Strings em C são terminadas por `\0`; o loop para quando encontra esse caractere.
* `len` inicia em `0` e cresce até o final da string.

---

### ex01: `ft_putstr`

**Descrição do exercício:**
Neste exercício, você deve implementar a função **ft\_putstr**, que recebe um ponteiro para o início de uma string e imprime seu conteúdo no stdout, utilizando apenas a função `write`.

**Funções autorizadas:** write

```c
#include <unistd.h>

void ft_putstr(char *str)
{
    int len = 0;               /* conta quantos bytes serão escritos */

    while (str[len])           /* percorre até encontrar '\0' */
        len++;

    write(1, str, len);        /* escreve `len` bytes de `str` no stdout */
}
```

**Dicas e observações:**

* Sem `strlen`, contamos manualmente o tamanho da string.
* `write(1, str, len)` imprime do início até o terminador.

---

### ex02: `ft_putnbr`

**Descrição do exercício:**
Neste exercício, você deve implementar a função **ft\_putnbr**, que recebe um valor `int` e o imprime em decimal no stdout, cobrindo todo o intervalo possível de `int`, usando apenas `write`.

**Funções autorizadas:** write

```c
#include <unistd.h>

static void ft_putnbr_rec(long n)
{
    char c;

    if (n >= 10)
        ft_putnbr_rec(n / 10); /* imprime dígitos anteriores recursivamente */

    c = '0' + (n % 10);      /* converte dígito para caractere ASCII */
    write(1, &c, 1);         /* imprime o dígito atual */
}

void ft_putnbr(int nb)
{
    long n = nb;             /* usar `long` para lidar com INT_MIN */

    if (n < 0)
    {
        write(1, "-", 1);  /* imprime sinal de negativo */
        n = -n;              /* converte para positivo */
    }
    ft_putnbr_rec(n);        /* chamada recursiva que imprime todos os dígitos */
}
```

**Dicas e observações:**

* A recursão garante que o dígito mais significativo seja impresso primeiro.
* A promoção para `long` evita overflow em `-2147483648`.

---

### ex03: `ft_atoi`

**Descrição do exercício:**
Neste exercício, você deve implementar a função **ft\_atoi**, que converte o início de uma string em um inteiro (`int`), considerando espaços em branco iniciais e múltiplos sinais `+` ou `-`, sem usar funções de biblioteca.

**Funções autorizadas:** nenhuma

```c
int ft_atoi(char *str)
{
    int i = 0;
    int sign = 1;            /* sinal positivo por padrão */
    int result = 0;

    /* 1) Pular whitespace */
    while (str[i] == ' ' || (str[i] >= 9 && str[i] <= 13))
        i++;                 /* avança até o primeiro caractere não-whitespace */

    /* 2) Processar sinais '+' e '-' */
    while (str[i] == '+' || str[i] == '-')
    {
        if (str[i] == '-')
            sign = -sign;    /* cada '-' inverte o sinal */
        i++;
    }

    /* 3) Converter dígitos */
    while (str[i] >= '0' && str[i] <= '9')
    {
        result = result * 10 + (str[i] - '0');
        i++;
    }

    return sign * result;    /* retorna o valor com sinal */
}
```

**Dicas e observações:**

* Sinais consecutivos alternam o sinal final (ex.: `---123` resulta em -123).
* Não há tratamento de overflow/underflow; comportamento indefinido.

---

### ex04: `ft_putnbr_base`

**Descrição do exercício:**
Aqui você deve implementar a função **ft\_putnbr\_base**, que recebe um inteiro e uma string representando os símbolos da base e imprime o número nessa base, tratando números negativos e validando a base, usando apenas `write`.

**Funções autorizadas:** write

```c
#include <unistd.h>

/* Verifica se a base é válida */
static int valid_base(char *base)
{
    int i, j;
    for (i = 0; base[i]; i++)
        ; /* calcula comprimento da base */
    if (i < 2) return 0;    /* precisa ter pelo menos 2 símbolos */

    for (i = 0; base[i]; i++)
    {
        if (base[i] == '+' || base[i] == '-' ||
            base[i] == ' ' || (base[i] >= 9 && base[i] <= 13))
            return 0;        /* caracteres proibidos */
        for (j = i + 1; base[j]; j++)
            if (base[i] == base[j])
                return 0;    /* não pode ter símbolos repetidos */
    }
    return 1;
}

/* Imprime recursivamente cada dígito na base */
static void ft_putnbr_base_rec(long nbr, char *base, int blen)
{
    if (nbr >= blen)
        ft_putnbr_base_rec(nbr / blen, base, blen);
    write(1, &base[nbr % blen], 1);
}

void ft_putnbr_base(int nbr, char *base)
{
    long n = nbr;
    int blen;

    if (!valid_base(base))
        return;              /* saída imediata se base inválida */

    for (blen = 0; base[blen]; blen++)
        ; /* obtém tamanho da base */

    if (n < 0)
    {
        write(1, "-", 1);
        n = -n;
    }
    ft_putnbr_base_rec(n, base, blen);
}
```

**Dicas e observações:**

* `valid_base` garante que a string de base seja correta (tamanho mínimo, sem duplicatas nem caracteres proibidos).
* A recursão permite imprimir dígitos em ordem correta.

---

### ex05: `ft_atoi_base`

**Descrição do exercício:**
Neste exercício, você deve implementar a função **ft\_atoi\_base**, que converte o início de uma string em um inteiro de acordo com a base fornecida (string de símbolos), considerando espaços em branco iniciais e sinais, sem usar funções de biblioteca.

**Funções autorizadas:** nenhuma

```c
/* Valida a base para `ft_atoi_base` */
static int valid_base_atoi(char *base)
{
    int i, j;
    for (i = 0; base[i]; i++)
        ;
    if (i < 2) return 0;

    for (i = 0; base[i]; i++)
    {
        if (base[i] == '+' || base[i] == '-' ||
            base[i] == ' ' || (base[i] >= 9 && base[i] <= 13))
            return 0;
        for (j = i + 1; base[j]; j++)
            if (base[i] == base[j])
                return 0;
    }
    return 1;
}

int ft_atoi_base(char *str, char *base)
{
    int i = 0;
    int sign = 1;
    int result = 0;
    int blen;
    int digit;

    if (!valid_base_atoi(base))
        return 0;           /* retorna 0 se base inválida */

    for (blen = 0; base[blen]; blen++)
        ; /* calcula tamanho da base */

    while (str[i] == ' ' || (str[i] >= 9 && str[i] <= 13))
        i++;               /* pula whitespace */

    while (str[i] == '+' || str[i] == '-')
    {
        if (str[i] == '-')
            sign = -sign;  /* processa sinais */
        i++;
    }

    while (str[i])
    {
        digit = -1;
        for (int j = 0; j < blen; j++)
        {
            if (str[i] == base[j])
            {
                digit = j;  /* encontra valor numérico correspondente */
                break;
            }
        }
        if (digit == -1)
            break;         /* para na primeira letra inválida */

        result = result * blen + digit;  /* soma dígito ao acumulado */
        i++;
    }

    return sign * result;
}
```

**Dicas e observações:**

* Funciona de forma similar ao `ft_atoi`, mas mapeia cada caractere ao seu índice na base.
* A conversão interrompe ao encontrar caractere não pertencente à base.

---

## Dicas Gerais

* **Testes de Borda:** experimente strings vazias, apenas sinais, bases curtas e inválidas.
* **Whitespace ASCII:** caracteres 9–13 são tabs e quebras de linha.
* **Overflow/Underflow:** não tratado aqui, mas é importante entender o comportamento em C.
* **Makefile e Testes:** crie um `Makefile` para compilar cada exercício e escreva pequenos programas de teste para validar todas as situações.

> **Bônus:** Documente seu processo de testes em um arquivo `README.md`, descrevendo cada caso de teste e o resultado esperado.
