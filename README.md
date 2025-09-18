# Programação Lógica – Prolog

## 📚 Parte Teórica

### ✅ Pontos compreendidos

---

**Variável anônima (`_`)**  
- Usada como *placeholder*, quando o valor em si não importa, apenas a posição.  
- Evita criar variáveis que não serão utilizadas.  

Exemplo:  
```prolog
?- casa(_, azul, _) = casa(bob, azul, gato).
true.
```

---

**Predicado `nextto/3`**  
- Usado para verificar se dois elementos são consecutivos em uma lista.  

Exemplo:  
```prolog
?- nextto(b,c,[a,b,c,d]).
true.

?- nextto(b,X,[a,b,1,a,b,2]).
X = 1 ;
X = 2 ;
false.
```

---

**Predicado `select/3` e `select/4`**  
- `select/3`: remove ou identifica um elemento de uma lista.  
- `select/4`: substitui um elemento antigo por um novo em uma lista.  

Exemplo:  
```prolog
% select/3 - remoção ou identificação de elemento
?- select(b, [a,b,c], R).
R = [a, c].

?- select(X, [a,b,c], [a,c]).
X = b.

?- select(X, [a,b,c], Y).
X = a, Y = [b, c] ;
X = b, Y = [a, c] ;
X = c, Y = [a, b].

% select/4 - substituição de elemento
?- select(b, [a,b,c], x, R).
R = [a, x, c].

?- select(X, [a,b,c], y, R).
X = a, R = [y, b, c] ;
X = b, R = [a, y, c] ;
X = c, R = [a, b, y].
```

---

### ❌ Pontos não compreendidos

**Predicado `findall/3`**  
Inicialmente confuso, mas o entendimento melhorou com este exemplo:  

```prolog
happening(monday,chemistry).
happening(monday,english).
happening(tuesday,chemistry).
happening(wednesday,maths).
happening(friday,chemistry).
happening(friday,maths).

find_lessons(Bag) :- findall(X, happening(X,chemistry), Bag).
```

Consulta:  
```prolog
?- find_lessons(Bag).
Bag = [monday, tuesday, friday].
```

Aqui, `findall/3` coleta todos os valores possíveis de `X` que satisfazem a condição `happening(X, chemistry)` e retorna a lista completa em `Bag`.  

---

**Uso de `trace`**  
- trace/0
   - Inicia o rastreador e o sinaliza, posterior traça o próximo objetivo.
- tracing/0
   - Mostra se o rastreador está atualmente ligado.
- notrace/0
   - Para o rastreador
- trace(+Pred)
   - Coloca um ponto de rastreamento em todos os predicados correspondentes.
- trace(+Pred, +Ports)
   - Coloca pontos de rastreamentos em predicados e portas específicas.

- As portas:
   - Call:  quando o Prolog tenta provar um objetivo.
   - Exit: quando o Prolog prova com sucesso um objetivo.
   - Redo: quando o Prolog volta atrás (backtracking) para buscar novas soluções.
   - Fail: quando o Prolog não consegue provar o objetivo.

Exemplo de saída:  
```prolog
?- trace, member(X,[a,b]).
   Call: (10) member(_G123,[a,b]) ?
   Exit: (10) member(a,[a,b]) ?
X = a ;
   Redo: (10) member(_G123,[a,b]) ?
   Exit: (10) member(b,[a,b]) ?
X = b.
``` 

---

**Predicado list_to_set/2**  
- Converte uma lista em um conjunto, removendo elementos duplicados.

Exemplo:  
```prolog
?- L = [a, s, d, a, f, s], list_to_set(L, S).
L = [a, s, d, a, f, s],
S = [a, s, d, f].
```  

---

## 🛠️ Parte Prática

### Exercícios realizados com `movies.pl`
1. **O filme schindlers_list é uma comédia?**

```prolog
?- movie(M, schindlers_list, _), genre(M, comedy).
false
```

![Consulta 1](ex1.gif)

2. **Quais os atores (masculinos) do filme inglorious_basterds?**

```prolog
?- movie(M, inglorious_basterds, _), actor(M, Actor).
M = 11,
Actor = brad_pitt;
M = 11,
Actor = eli_roth.
```

![Consulta 2](ex2.gif)

3. **Quais os filmes lançados na década de 80 (entre 1981 e 1990, inclusive)? Dica: veja operadores relacionais no material da aula passada.**

```prolog
?- movie(_, Title, Year), Year >= 1981, Year =< 1990.
Title = dead_poets_society,
Year = 1989;
false.
```

 ![Consulta 3](ex3.gif)

4. **Quais os atores ou atrizes do filme the_hunger_games?**
 -  Nesse fiz uma consulta prévia apenas para não ficar repetindo o número do filme.

```prolog
?- movie(M, the_hunger_games, _).
3.
?- movie(3, the_hunger_games, _), (actor(3, Person); actress(3, Person)).
Person = josh_hutcherson;
Person = liam_hemsworth;
Person = jennifer_lawrence.
```

 ![Consulta 4](ex4.gif)

5. **O ator brad_pitt é um ator de drama?**

```prolog
?-drama_actor(brad_pitt).
true.
```

 ![Consulta 5](ex5.gif)

6. **Há quantos anos foi lançado o filme big_fish? Consulte o material da aula passada para saber como fazer operações aritméticas em Prolog.**

```prolog
?-movie(_, big_fish, Year), YearsAgo is 2025 - Year.
Year = 2003,
YearsAgo = 22.
```

 ![Consulta 6](ex6.gif)

7. **O ator liam_neeson é um ator de comédia?**

```prolog
?- actor(M, liam_neeson), genre(M, comedy).
false.
```

 ![Consulta 7](ex7.gif)
 

---

### Regras adicionadas no arquivo `movies.pl`

- `drama_artist(A)` → verdadeiro se `A` for ator ou atriz de um filme de drama.  
- `movieAgeByName(M, Age)` → retorna a idade do filme `M` em anos, calculada a partir do ano atual.

```prolog
dram_artist(A) :- (actor(M, A); actress(M, A)), genre(M, drama).

movieAgeByName(M, Age) :- movie(_, M, Year), Age is 2025 - Year.
```

![Adição das regras](adicionaRegra12.gif)

---

### Consultas com listas

**Gêneros de filmes da base:**  
```prolog
?- allgenres(G).
G = [dark_comedy, psychological_thriller, drama ...].
```

**Primeiro gênero da lista:**  
```prolog
?- allgenres(G), G = [H | _].
H = dark_comedy.
```
- O símbolo | é o operador de separação entre o head (cabeça) e o tail (cauda) de uma lista e, neste caso, está dizendo: pegue o primeiro gênero em H e ignore o resto (_).

**Quantidade de gêneros distintos:**  
```prolog
?- countgenres(C).
C = 39.
```

![Consulta com Lista](consultaComLista.gif)

---

**Adicione regra com lista**

```prolog
count_users(Qtd) :- findall(U, likes(U, _), Users),
     list_to_set(Users, Uniq),
     length(Uniq, Qtd).
```

## 📖 Referências Bibliográficas

- https://stackoverflow.com/questions/64223921/how-do-i-write-findall-in-a-prolog-code-itself
- https://www.swi-prolog.org/pldoc/man?section=lists
- https://www.ic.unicamp.br/~meidanis/courses/mc600/200002/Manual-Prolog/sec-3.39.html
- https://www.swi-prolog.org/pldoc/man?predicate=list_to_set/2
