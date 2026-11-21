## Resolucao dos Problemas da Prova 01
- Eletiva: Programacao Competitiva I

## Missing Number

Inicialmente, a ideia que vem a vem a cabeca eh armazenar tais numeros num vetor e preenche-lo.

Isso daria uma complexidade $O(n)$, e como n <= 2*10^5, o programa passaria em um limite de tempo 1000ms.

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

Sendo assim, resolvemos o problemas com:
- Tempo: $O(n)$
- Memoria $O(1)$

## Increasing Array

Paro o problema do array crescente devemos comparar os elementos dois a dois e vermos se eles satisfazem a seguinte propriedade:

    


