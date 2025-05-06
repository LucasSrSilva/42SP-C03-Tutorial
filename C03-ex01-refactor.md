---

# Otimização da Função `ft_strncmp`

## **Problemas na Versão Original**
1. Acesso desnecessário a `s1[i]` e `s2[i]` duas vezes (no `while` e no `return`).  
2. Comparação `i < n` em cada iteração, o que pode ser ligeiramente mais lento.  
3. Conversão para `unsigned char` no final, que poderia ser feita antecipadamente.

---
   
## Versao otimizada
```c
int ft_strncmp(char *s1, char *s2, unsigned int n)
{
    if (n == 0)
        return 0;

    while (--n && *s1 && *s1 == *s2)
    {
        s1++;
        s2++;
    }
    return (unsigned char)*s1 - (unsigned char)*s2;
}
```

## **Melhorias Implementadas**
✅ **Verificação inicial de `n == 0`** → Retorna imediatamente se `n` for zero.  
✅ **Pré-decremento `--n`** → Reduz o número de comparações no loop.  
✅ **Incremento direto de ponteiros (`s1++`, `s2++`)** → Mais eficiente que indexação (`s1[i]`).  
✅ **Conversão para `unsigned char` apenas no final** → Evita operações redundantes.  

## **Testes**
```c
printf("%d\n", ft_strncmp("abc", "abc", 3));  // 0 (igual)
printf("%d\n", ft_strncmp("abc", "abd", 3));  // -1 (diferente)
printf("%d\n", ft_strncmp("abc", "a", 1));    // 0 (primeiro char igual)
printf("%d\n", ft_strncmp("", "", 1));        // 0 (strings vazias)
```

### **Resultado Esperado**
- **Mais rápido** em comparações longas.  
- **Evita timeout** em casos extremos (strings muito grandes ou `n` muito alto).  

🚀 **Pronto para uso!**  

---

### **Como Usar?**
1. Copie o código otimizado.  
2. Substitua a versão antiga da função.  
3. Teste com os casos de uso acima para verificar a eficiência.  
