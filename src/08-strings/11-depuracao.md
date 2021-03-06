## 8.11 - Depuração

Ao usar índices para [atravessar](12-glossario.md#atravessar) os valores em uma [sequência](12-glossario.md#sequência), é complicado acertar o começo e o fim da travessia. Aqui está uma função que supostamente compara duas palavras e retorna True se uma das palavras for o reverso da outra, mas contém dois erros:

```python
def is_reverse(word1, word2):
    if len(word1) != len(word2):
        return False
    i = 0
    j = len(word2)
    while j > 0:
        if word1[i] != word2[j]:
            return False
        i = i+1
        j = j-1
    return True
```

A primeira instrução if verifica se as palavras têm o mesmo comprimento. Se não for o caso, podemos retornar False imediatamente. Do contrário, para o resto da função, podemos supor que as palavras tenham o mesmo comprimento. Este é um exemplo do modelo de guardião em “Verificação de tipos”, na página 101.

i e j são índices: i atravessa word1 para a frente, enquanto j atravessa word2 para trás. Se encontrarmos duas letras que não combinam, podemos retornar False imediatamente. Se terminarmos o loop inteiro e todas as letras corresponderem, retornamos True.

Se testarmos esta função com as palavras “pots” e “stop”, esperamos o valor de retorno True, mas recebemos um IndexError:

```python
>>> is_reverse('pots', 'stop')
...
  File "reverse.py", line 15, in is_reverse
    if word1[i] != word2[j]:
IndexError: string index out of range
```

Para depurar este tipo de erro, minha primeira ação é exibir os valores dos índices imediatamente antes da linha onde o erro aparece.

```python
while j > 0:
    print(i, j)        # exibir aqui
    if word1[i] != word2[j]:
        return False
    i = i+1
    j = j-1
```

Agora quando executo o programa novamente, recebo mais informação:

```python
>>> is_reverse('pots', 'stop')
0 4
...
IndexError: string index out of range
```

Na primeira vez que o programa passar pelo loop, o valor de j é 4, que está fora do intervalo da string 'pots'. O [índice](12-glossario.md#índice) do último caractere é 3, então o valor inicial de j deve ser len(word2)-1.

Se corrigir esse erro e executar o programa novamente, recebo:

```python
>>> is_reverse('pots', 'stop')
0 3
1 2
2 1
True
```

Desta vez, recebemos a resposta certa, mas parece que o loop só foi executado três vezes, o que é suspeito. Para ter uma ideia melhor do que está acontecendo, é útil desenhar um diagrama de estado. Durante a primeira iteração, o frame de `is_reverse` é mostrado na Figura 8.2.

![Figura 8.2 – Diagrama de estado de is_reverse](/fig/tnkp_0802.png).
<br>_8.2 – Diagrama de estado de is_reverse._

Tomei a liberdade de arrumar as variáveis no frame e acrescentei linhas pontilhadas para mostrar que os valores de i e j indicam caracteres em word1 e word2.

Começando com este diagrama, execute o programa em papel, alterando os valores de i e j durante cada iteração. Encontre e corrija o segundo erro desta função.
