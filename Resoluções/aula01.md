
# Resolução dos Problemas da Prova 01
- Eletiva: Programação Competitiva I

## Missing Number

Inicialmente, a ideia que vem à cabeça é armazenar tais números num vetor e preenchê-lo.

Isso daria uma complexidade $O(n)$ e, como $n \leq 2 \cdot 10^5$, o programa passaria em um limite de tempo de 1000 ms.

1) Solução com array:

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
- Memória: $O(n)$

Caso o problema fosse mais rigoroso com o limite de tempo ou de memória, teríamos que ter uma **sacada** para resolver tal problema.

Vamos considerar $n = 5$ para construir nosso raciocínio.

Os números que podem estar na nossa lista são:

$$
\{1, 2, 3, 4, 5\}
$$

Como esses elementos formam uma PA de razão 1, sabemos que:

$$
S_{\text{total}} = \frac{n(n+1)}{2}
$$

Caso o elemento 2 fosse o elemento que falta, teríamos:

$$
\{1, x, 3, 4, 5\}
$$

Que, como vimos, possui soma:

$$
S_{\text{atual}} = 1 + x + 3 + 4 + 5
$$

Seja Soma_atual a soma de todos os elementos que estão presentes na lista. Então:

$$
S_{\text{total}} = S_{\text{atual}} + x
$$

Logo,

$$
\boxed{x = S_{\text{total}} - S_{\text{atual}}}
$$

Portanto, podemos resolver tal problema pensando somente na soma de todos os elementos até $n$ e na soma de todos os elementos presentes.

2) Solução ideal:

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
- Memória: $O(1)$

## Increasing Array

Para o problema do array crescente, devemos comparar os elementos dois a dois e ver se eles satisfazem a seguinte propriedade:

$$
v[i + 1] \geq v[i]
$$

Que é a definição de um array crescente. Então, se encontrarmos o contrário, devemos ajustar somando o mínimo possível para garantir que o array fique crescente. Ou seja, se

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

Vejamos a codificação:

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;

    vector<long long> v(n);

    for(int i = 0; i < n - 1; i++){
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
- Memória: $O(n)$

Agora, vamos codificar analisando a condição de array crescente durante as entradas!

Vamos começar recebendo o valor de $n$ (tamanho do array). Em seguida, vamos receber o primeiro valor do array em uma variável chamada $anterior$. Depois, receber o próximo valor numa variável chamada $atual$.

Para cada iteração no loop, o valor $atual$ vira o valor $anterior$ e recebemos um novo valor $atual$ para compararmos.

Vamos ver isso em código!

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
- Memória: $O(1)$

## Number Spiral

Para essa questão, percorrer um for loop aninhado é inviável computacionalmente.

Devemos partir para a ideia de, apenas com as informações de linha ($i$) e coluna ($j$), calcularmos o elemento em tal posição.

Vamos estudar padrões dessa matriz em espiral.

Note que a diagonal principal é uma PA de segunda ordem:

$$
b_n = \{1, 3, 7, 13, 21, \ldots\}
$$

E a razão é a seguinte PA:

$$
a_n = \{2, 4, 6, 8, 10, \ldots\}
$$

Então, se quisermos o elemento $b_n$, temos que fazer a seguinte conta:

$$
b_n = b_1 + \sum_{i=1}^{n-1} a_i
$$

E, por sua vez, $\sum_{i=1}^{n-1} a_i$ é a soma de uma PA de razão 2, que é dada por:

$$
\sum_{i=1}^{n-1} a_i = 2\frac{n(n+1)}{2}
$$

Logo, o termo geral da diagonal em espiral apresentada é:

$$
b_n = 1 + n(n-1)
$$

Agora, temos que pensar em como relacionar o elemento da diagonal principal com o elemento escolhido.

* Vamos pegar o elemento $m_{33} = 7$ (**em linhas ímpares**), veja que:

1) Ao percorrermos a mesma linha $i = 3$ para a esquerda, subtraímos $1$ a cada coluna.

2) Ao percorrermos a mesma coluna $j = 3$ para cima, somamos $1$ a cada linha.

* Vamos pegar o elemento $m_{44} = 13$ (**em linhas pares**), veja que:

1) Ao percorrermos a mesma linha $i = 4$ para a esquerda, somamos $1$ a cada coluna.

2) Ao percorrermos a mesma coluna $j = 4$ para cima, subtraímos $1$ a cada linha.

**Conclusão:** Dependendo da paridade do índice do elemento da diagonal, fazemos ações diferentes.

Finalmente, vamos relacionar um elemento $m_{ij}$ qualquer com o elemento da diagonal principal:

- Se $i > j$:
escolhemos o elemento $m_{ii}$.

- Se $i < j$:
escolhemos o elemento $m_{jj}$.

Por fim, para chegar ao valor de $m_{ij}$, verificamos a paridade e somamos o número de linhas ou colunas que faltam para chegar ao elemento pedido.

Exemplo:
Queremos saber qual é o elemento $m_{24}$.

Como $2 < 4$, vamos pegar o elemento da diagonal principal:

$$
m_{44}
$$

Pelo termo geral da PA de segunda ordem, temos:

$$
m_{44} = b_4 = 1 + 4(4-1) = 13
$$

Como se trata de linhas pares, temos:

$$
m_{24} = m_{44} + (4 - 2) = 11
$$

De fato, observando a matriz dada pelo enunciado, $m_{24} = 11$.

Encontramos uma maneira de achar qualquer elemento $m_{ij}$ sem percorrer a matriz.

Vamos à codificação:

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
- Memória: $O(1)$

## Mex Grid Construction

A ideia é criar uma função que percorre todos os números à esquerda e acima do elemento $m_{ij}$, marcá-los como `visitado` em um vetor booleano. Em seguida, percorrer esse vetor e encontrar o primeiro elemento NÃO visitado. O índice desse elemento será o número que entra na posição de $m_{ij}$.

Vamos codificar!

```cpp
#include<bits/stdc++.h>
using namespace std;

int check(vector<vector<int>> &m, int x, int y, int n){
    int melhor = -1;

    bool visto[10001] = {false};

    //linha esquerda
    for(int j = y - 1; j >= 0; j-- ){
        if(!visto[m[x][j]]){
            visto[m[x][j]] = true;
        }
    }

    //coluna cima
    for(int i = x - 1; i >= 0; i--){
        if(!visto[m[i][y]]){
            visto[m[i][y]] = true;
        }
    }

    for(int k = 0; k < 10001; k++){
        if(!visto[k]){
            melhor = k;
            break;
        }
    }

    return melhor;
}

int main(){
    int n;
    cin >> n;

    vector<vector<int>> m(n, vector<int>(n, 0));


    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){

            m[i][j] = check(m, i, j, n);
        }
    }

    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            cout << m[i][j] << ' ';
        }
        cout << '\n';
    }


    return 0;
}
```

Sendo assim, resolvemos o problema com:
- Tempo: $O(n^{3})$
- Memória: $O(n^{2})$

**Obs:** Nessa questão, o código passou, pois $n$ é limitado a 100, mas uma solução com complexidade cúbica nem sempre passa. Vi uma solução com complexidade temporal $O(n^2)$, mas não entendi. Tenho que estudar mais para entendê-la e passar pra cá.
