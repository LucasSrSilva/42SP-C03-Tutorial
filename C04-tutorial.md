# Projeto C04 - Funções Básicas em C

Este projeto tem como objetivo implementar e testar funções fundamentais de manipulação de strings e conversão numérica em linguagem C.

## 📁 Arquivos Esperados

Você deve implementar as seguintes funções, preferencialmente separadas em arquivos `.c` e `.h`:

- `int ft_strlen(char *str);`
- `void ft_putstr(char *str);`
- `void ft_putnbr(int nb);`
- `int ft_atoi(char *str);`
- `void ft_putnbr_base(int nbr, char *base);`
- `int ft_atoi_base(char *str, char *base);`

---

## 📚 Descrição das Funções

### `ft_strlen`
Conta e retorna o número de caracteres de uma string (sem considerar o caractere nulo `'\0'`).

### `ft_putstr`
Imprime a string passada como parâmetro na saída padrão (`stdout`), caractere por caractere.

### `ft_putnbr`
Imprime um número inteiro (positivo ou negativo) na saída padrão.

### `ft_atoi`
Converte uma string para número inteiro, ignorando espaços em branco e tratando sinais `+` e `-`.

### `ft_putnbr_base`
Imprime um número inteiro usando uma base numérica fornecida como string (ex: binário, decimal, hexadecimal). A base precisa ser válida (sem símbolos repetidos, vazia ou contendo `+` ou `-`).

### `ft_atoi_base`
Converte uma string que representa um número em uma determinada base para o valor decimal (base 10). A base deve seguir as mesmas regras de validade da função anterior.

---

## 🚀 Como Testar

1. Implemente todas as funções em seus respectivos arquivos `.c` e `.h` (ou tudo em um único `main.c` para simplicidade).
2. Escreva um `main()` com chamadas de teste para cada função.
3. Compile com:

```bash
gcc -Wall -Wextra -Werror main.c -o main
```
4. Execute:
```bash
./main
```
