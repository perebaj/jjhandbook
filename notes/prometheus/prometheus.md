


## Sum x Count


A principal diferença entre as duas funções é que "sum" retorna a soma dos valores da métrica, enquanto "count" retorna o número de amostras da métrica. Em outras palavras, "sum" é uma função de agregação que lida com valores numéricos, enquanto "count" é uma função de agregação que lida com a contagem de amostras.

--- 

## Operadores de agreção e agrupamento
Os operadores de agregação podem ser usados

Esses operadores podem ser usados para agregar as métricas do Prometheus em todas as dimensões de rótulos ou preservar as dimensões distintas, incluindo uma cláusula "without" ou "by". Essas cláusulas podem ser usadas antes ou depois da expressão.

```sql
<aggr-op> [without|by (<label list>)] ([parameter,] <vector expression>)
or

<aggr-op>([parameter,] <vector expression>) [without|by (<label list>)]
```