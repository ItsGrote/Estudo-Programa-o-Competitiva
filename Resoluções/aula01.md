## Resolucao dos Problemas da Prova 01
- Eletiva: Programacao Competitiva I

## Missing Number

Inicialmente, a ideia que vem a vem a cabeca eh armazenar tais numeros num vetor e preenche-lo.

Isso daria uma complexidade $O(n)$, e como $n \leq 2*10^5$, o programa passaria em um limite de tempo 1000ms.

1) Solucao com array:

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;

    vector<int> v(n + 1, 0);

    for(int i = 1; i < n; i++){
        int x;
        cin >> x;

        v[x]++;
    }

    int ans;

    for(int i = 1; i <= n; i++){
        if(v[i] == 0){
            ans = i;
            break;
        }
    }

    cout << ans << '\n';

    return 0;
}
```

Sendo assim, resolvemos o problema com:
- Tempo: $O(n)$
- Memoria: $O(n)$

Caso o problema fosse mais rigoroso com o limite de tempo ou de memoria, teriamos que ter uma **sacada** para resover tal problema.

Vamos considerar n = 5 para construir nosso raciocinio.

Os numeros que podem estar na nossa lista sao:

$$
\{1, 2, 3, 4, 5}\
$$

Como esses elementos formam uma PA de razao 1, sabemos que:

$$
S_{\text{total}} = \frac{n(n+1)}{2}
$$

Caso o elemento 2 fosse o elemento que falta, teriamos:

$$
\{1, x, 3, 4, 5}\
$$

Que, como vimos, possui soma:

$$
S_{\text{atual}} = 1 + x + 3 + 4 + 5
$$

Seja Soma_atual a soma de todos os elemento que estao presentes na lista, entao:

$$
S_{\text{total}} = S_{\text{atual}} + x
$$

Logo,

$$
\boxed{x = S_{\text{total}} - S_{\text{atual}}}
$$

Potantos, podemos resolver tal problema pensando somente nas somas de todos os ellementos ate n e na soma de todos os elementos presentes.

2) Solucao ideal:

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;

    long long Soma_total = 1LL * n*(n + 1)/2;
    long long Soma_atual = 0;

    for(int i = 0; i < n - 1; i++){
        int x;
        cin >> x;

        Soma_atual += x;
    }

    cout << Soma_total - Soma_atual << '\n';

    return 0;
}
```

Sendo assim, resolvemos o problema com:
- Tempo: $O(n)$
- Memoria: $O(1)$

## Increasing Array

Para o problema do array crescente devemos comparar os elementos dois a dois e vermos se eles satisfazem a seguinte propriedade:

$$
v[i + 1] \geq v[i]
$$

Que e a definicao de um array crescente. Entao, se encotrarmos o o contrarios, devemos ajustar somando o minimo possivel para garantir que o array fique crescente. Ou seja, se

$$
v[i + 1] < v[i]
$$

Devemos fazer um incremento em $v[i + 1]$ de:

$$
inc = v[i] - v[i + 1]
$$

E atualizaremos o array com o novo valor de $v[i + 1]$:

$$
v[i + 1] = v[i]
$$

Vejamos a codificao:

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;

    vector<long long> v(n);

    for(int i = 0; i < n; i++){
        cin >> v[i];
    }

    long long inc = 0;

    for(int i = 0; i < n; i++){
        if(v[i + 1] < v[i]){
            inc += (v[i] - v[i + 1]);
            v[i + 1] = v[i];
        }   
    }

    cout << inc << '\n';

    return 0;
}
```
Sendo assim, resolvemos o problema com:
- Tempo: $O(n)$
- Memoria: $O(n)$

Agora, vamos codificar  analisando a condicao de array crescente durante as entradas!

Vamos comecar recebendo o valor  de n (tamanho do array), em seguida, vamos receber o primeiro valor do array em um variavel chamada $anterior$. Depois, receber  o proximo valor numa variavel chamada $atual$.

Para cara iteracao no loop, o valor $atual$ vira o valor $anterior$ e recebemos um novo valor $atual$ para compararmos.

Vamos ver isso em codigo!

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){

    int n;
    cin >> n;

    long long anterior;
    cin >> anterior;

    long long inc = 0;

    for(int i = 1; i < n; i++){
        long long atual;
        cin >> atual;

        if(atual < anterior){
            inc += (anterior - atual);
        } else {
            anterior = atual;
        }
    }

    cout << inc << '\n';

    return 0;
}
```

Sendo assim, resolvemos o problema com:
- Tempo: $O(n)$
- Memoria: $O(1)$

## Number Spiral

Para essa questao, percorrer um for loop aninhado e inviavel computacionalmente.

Devemos partir para a ideia de, apenas com as informacoes de linha ($i$) e coluna ($j$) calcularmos o elemento em tal posicao.

Vamos estudar paroes dessa matriz em espiral.


Note que a diagonal principal eh uma PA de segunda ordem:

$$
b_{\text{n}} = \{1, 3, 7, 13, 21, ...}\
$$

E a razao e a seguinte PA:

$$
a_{\text{n}} = \{2, 4, 6, 8, 10, ...}\
$$

Entao, se quisermos o elemento $b_{\text{n}}$ temos que fazer a seguinte conta:

$$
{b_{\text{n}} = b_{1} + \sum_{i=1}^{n-1} a_{\text{i}}}
$$

E, por sua vez, $\sum_{i=1}^{n-1} a_{\text{i}}$ e a soma de uma PA de razao 2, que e dada por:

$$
\sum_{i=1}^{n-1} a_{\text{i}} = 2\frac{n(n+1)}{2}
$$

Logo, o termo gera da diagonal em espiral apresentada e:

$$
{b_{\text{n}} = 1 + n(n-1)} 
$$

Agora, temos que pensar em como relacionar o elemento da diganol principal com o elemento escolhido.

* vamos pegar o elemento $m_{33} = 7$ (*em linhas impares*), veja que:

- Ao percorrermos a mesma linha $i = 3$ para a esquerda, subtraimos $1$ a cada coluna.

- Ao percorrermos a mesma coluna $j = 3$ para cima, somamos $1$ a cada linha.

* vamos pegar o elemento $m_{44} = 13$ (*em linhas pares*), veja que:

- Ao percorrermos a mesma linha $i = 4$ para a esquerda, somamos $1$ a cada coluna.

- Ao percorrermos a mesma coluna $j = 4$ para cima, subtraimos $1$ a cada linha.

**Conlcusao:** Dependendo da paridade do indice do elemento diagnal, fazemos acoes diferentes. 

Finalmente, vamos relacionar uma elemento $m_{\text{ij}}$ qualquer com o elemnteto da diagonall principal:

- Se $i > j$:
escolhemos o elemento $m_{/text{ii}}$

- Se $i < j$:
escolhemos o elemento $m_{/text{jj}}$

Por fim, para chegar no valor de $m_{\text{ij}}$ verificamos a paridade e somamos o numero de linhas ou colunas que faltam para chegar no elemento pedido.

Exemplo:
Queremos saber qual o elemento $m_{24}$.

Como $2 < 4$, vamos pegar o elemento da digonal principal:

$$
m_{44}
$$

Pelo termo geral da PA de segunda ordem, temos:

$$
m_{44} = b_{4} = 1 + \frac{4(4-1)} = 13
$$

Como se trata de linhas pares, temos:

$$
m_{24} = m_{44} + (4 - 2) = 11
$$

De fato, observando a matriz dada pelo enunciado, $m_{24} = 11$

Encontramos uma maneira de achar qualquer elemento $m_{\text{ij}}$ sem percorrer a matriz.

Vamos a codificacao:

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){

    int t;
    cin >> t;

    for(int k = 0; k < t; k++){
        long long i, j, diag, ans;
        cin >> i >> j;

        if(i > j){
            diag = 1 + i*(i-1);

            if(i % 2 == 0){
                ans = diag + (i - j);
            } else {
                ans = diag -(i - j);
            }
        } else {
            diag = 1 + j*(j-1);

            if(j % 2 == 0){
                ans = diag - (j - i);
            } else {
                ans = diag + (j - i);
            }
        }

        cout << ans << '\n';
    }


    return 0;
}
```

Sendo assim, resolvemos o problema com:
- Tempo: $O(1)$ (por consulta)
- Memoria: $O(1)$