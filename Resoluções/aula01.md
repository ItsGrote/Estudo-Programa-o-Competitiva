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

Sendo assim, resolvemos o problema com:
- Tempo: $O(n)$
- Memoria: $O(1)$
