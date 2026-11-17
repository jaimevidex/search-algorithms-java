# IATPC — Classical Search Algorithms in Java

A compact, dependency-free implementation of the classic search algorithms, all sharing one problem interface and one graph representation. The demo solves route-finding on the Romania map from *Artificial Intelligence: A Modern Approach*.

Built for the *Inteligência Artificial e Tecnologias* course at Instituto Superior Técnico.

## Algorithms

| Algorithm | Type | Frontier |
|---|---|---|
| `BreadthFirstSearch` | Uninformed | FIFO queue |
| `DepthFirstSearch` | Uninformed | LIFO stack |
| `UniformCostSearch` | Uninformed, cost-aware | Priority queue on `g(n)` |
| `GreedySearch` | Informed | Priority queue on `h(n)` |
| `AStarSearch` | Informed | Priority queue on `g(n) + h(n)` |

All of them extend a common `GraphSearch` base, so the difference between them is the frontier policy and the evaluation function — not five copies of the same loop.

## Design

```
searchalgorithm/    the algorithms, Node, and the Algorithms registry
searchproblem/      SearchProblem, State, Arc — the problem interface
undirectedgraph/    Graph, Vertex, VertexSet, Edge, Romenia (the demo map)
```

`Graph.searchSetSolution(origin, destination, algorithm)` builds the problem, runs the chosen algorithm and prints the path, the cost and the number of expanded nodes. Because the problem is expressed through interfaces, the same solvers work on any graph — the Romania map is just the demo.

`GraphSearch` keeps a visited set so cycles in the graph do not cause infinite loops, and `Node` stores parent pointers so the solution path is reconstructed by walking back from the goal.

## Run

```bash
javac $(find . -name '*.java') -d out
java -cp out Main
```

The default run searches **Arad → Bucharest** with A*. It prints the solution path step by step, the accumulated cost, and the search statistics that make the algorithms comparable:

```
******************* SOLUTION ********************
Node Expansions: …
Nodes Generated: …
State Repetitions: …
Runtime (ms): …
| Arad      |    0 | [Arad, Sibiu] -> 140
| Sibiu     |  140 | [Sibiu, Rimnicu Vilcea] -> 80
| …
*************************************************
```

To try another algorithm, edit the last argument in `Main.java`:

```java
graph.searchSetSolution("Arad", "Bucharest", Algorithms.GreedySearch);
```

## Notes

- Pure Java, no build tool and no dependencies.
- Heuristic values are straight-line distances, so A* is admissible.
- The graph data lives in `undirectedgraph/Romenia.java` and is trivial to replace.
