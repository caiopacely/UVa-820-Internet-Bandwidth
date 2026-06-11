# Atividade de Acompanhamento – Fluxo Máximo

## Problema G – UVa 820: Internet Bandwidth

### Integrantes

* Caio Pacely
* Catarina Garcia
* Paulo de Tarso

---

# 1. Resumo do Problema

O problema consiste em determinar a largura de banda máxima que pode ser transmitida entre dois computadores de uma rede. Cada conexão possui uma capacidade máxima de transmissão de dados e os dados podem ser enviados simultaneamente por diferentes caminhos.

O objetivo é descobrir a quantidade máxima de fluxo que pode sair de um nó de origem e chegar a um nó de destino, respeitando as capacidades de todas as conexões da rede.

Esse problema pode ser modelado diretamente como um problema de **Fluxo Máximo**.

---

# 2. Interpretação da Entrada e da Saída

## Entrada

A entrada descreve uma rede de computadores.

Primeiramente é fornecido:

* `n`: quantidade de nós da rede;
* `s`: nó de origem;
* `t`: nó de destino;
* `c`: quantidade de conexões.

Em seguida aparecem `c` linhas no formato:

```text
u v capacidade
```

onde:

* `u` = primeiro nó da conexão;
* `v` = segundo nó da conexão;
* `capacidade` = largura de banda da conexão.

As conexões são bidirecionais e podem existir múltiplas conexões entre o mesmo par de nós.

---

## Saída

Para cada rede deve ser exibida a largura de banda máxima entre o nó de origem e o nó de destino.

Formato:

```text
Network X
The bandwidth is Y.
```

onde:

* `X` é o número do caso de teste;
* `Y` é o fluxo máximo encontrado.

---

# 3. Modelagem da Rede de Fluxo

## Vértices

Cada computador da rede será representado por um vértice.

```text
Computador 1 → Vértice 1
Computador 2 → Vértice 2
Computador 3 → Vértice 3
Computador 4 → Vértice 4
```

---

## Origem

O vértice correspondente ao nó `s`.

Neste exemplo:

```text
Origem = 1
```

---

## Sorvedouro

O vértice correspondente ao nó `t`.

Neste exemplo:

```text
Destino = 4
```

---

## Arestas

Cada conexão da entrada gera uma aresta com capacidade igual à largura de banda informada.

Exemplo:

```text
1 2 20
```

representa uma conexão entre os nós 1 e 2 com capacidade 20.

---

## Capacidades

As capacidades representam exatamente a quantidade máxima de dados que pode trafegar por cada conexão.

Dessa forma, as restrições do problema são modeladas corretamente pela rede de fluxo.

---

# 4. Justificativa da Escolha do Algoritmo

Foi escolhido o algoritmo **Ford-Fulkerson**.

O Ford-Fulkerson encontra sucessivamente caminhos aumentantes entre a origem e o destino e envia fluxo através deles até que não seja mais possível aumentar o fluxo total.

A escolha foi feita porque:

* É o algoritmo estudado em sala para problemas de fluxo máximo;
* Resolve diretamente o problema proposto;
* Permite visualizar claramente o conceito de caminho aumentante;
* Facilita a compreensão do funcionamento do grafo residual.

A cada iteração:

1. Encontramos um caminho da origem ao destino;
2. Calculamos o gargalo do caminho;
3. Atualizamos o fluxo;
4. Atualizamos o grafo residual.

O processo termina quando não existe mais caminho aumentante.

---

# 5. Instância Pequena

Utilizaremos a rede apresentada no enunciado.

## Entrada

```text
4
1 4 5
1 2 20
1 3 10
2 3 5
2 4 10
3 4 20
```

## Representação da Rede

![Representação da Rede](https://github.com/user-attachments/assets/bcd1e962-7d96-451c-ae47-4f7ecacb6958)

---

# 6. Execução Manual do Ford-Fulkerson

## Estado Inicial

Fluxo total:

```text
0
```

---

## Caminho Aumentante 1

Escolhemos o caminho:

```text
1 → 2 → 4
```

Capacidades disponíveis:

```text
1→2 = 20
2→4 = 10
```

### Gargalo

```text
min(20,10) = 10
```

### Fluxo enviado

```text
10
```

### Fluxo acumulado

```text
10
```

### Grafo Residual

```text
1→2 = 10
2→4 = 0

2→1 = 10
4→2 = 10
```

---

## Caminho Aumentante 2

Escolhemos o caminho:

```text
1 → 3 → 4
```

Capacidades disponíveis:

```text
1→3 = 10
3→4 = 20
```

### Gargalo

```text
min(10,20) = 10
```

### Fluxo enviado

```text
10
```

### Fluxo acumulado

```text
20
```

### Grafo Residual

```text
1→3 = 0
3→4 = 10

3→1 = 10
4→3 = 10
```

---

## Caminho Aumentante 3

Escolhemos o caminho:

```text
1 → 2 → 3 → 4
```

Capacidades residuais:

```text
1→2 = 10
2→3 = 5
3→4 = 10
```

### Gargalo

```text
min(10,5,10) = 5
```

### Fluxo enviado

```text
5
```

### Fluxo acumulado

```text
25
```

### Grafo Residual

```text
1→2 = 5
2→3 = 0
3→4 = 5
```

---

## Encerramento

Após a terceira iteração não existe mais caminho aumentante que conecte o nó 1 ao nó 4.

Portanto, o algoritmo termina.

---

## Resumo das Iterações

| Caminho Aumentante | Gargalo | Fluxo Acumulado |
| ------------------ | ------- | --------------- |
| 1 → 2 → 4          | 10      | 10              |
| 1 → 3 → 4          | 10      | 20              |
| 1 → 2 → 3 → 4      | 5       | 25              |

---

# 7. Verificação da Resposta Final

Os fluxos enviados foram:

```text
1 → 2 → 4      = 10
1 → 3 → 4      = 10
1 → 2 → 3 → 4  = 5
```

Somando os fluxos:

```text
10 + 10 + 5 = 25
```

Logo:

```text
Fluxo Máximo = 25
```

Portanto, a largura de banda máxima entre os nós 1 e 4 é igual a **25**.

## Saída Esperada

```text
Network 1
The bandwidth is 25.
```

## Interpretação do Resultado

Isso significa que a rede consegue transmitir até 25 unidades de dados por unidade de tempo do nó 1 para o nó 4 utilizando simultaneamente diferentes caminhos da rede sem violar nenhuma restrição de capacidade.

---

# Conclusão

O problema Internet Bandwidth pode ser modelado diretamente como uma rede de fluxo. Os computadores são representados por vértices e as conexões por arestas com capacidades. Utilizando o algoritmo Ford-Fulkerson, encontramos sucessivos caminhos aumentantes até que não seja mais possível enviar fluxo adicional da origem para o destino. O valor acumulado ao final do processo corresponde à largura de banda máxima da rede.
