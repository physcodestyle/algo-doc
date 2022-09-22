---
 title: Поиск в ширину
 date: Last Modified
 permalink: /article/breadth_first_search/index.html
 eleventyNavigation:
   key: breadth_first_search
   order: 1
   title: Поиск в ширину
 ---

 ### Область применения:

 Поиск в ширину (англ. Breadth-First Search, BFS) позволяет вычислить кратчайшие расстояния (в терминах количества рёбер) от выделенной вершины ориентированного графа до всех остальных вершин, и/или построить корневое направленное дерево, расстояния в котором совпадают с расстояниями в исходном графе. Кроме того, поиск в ширину позволяет решать задачу проверки достижимости (существуют ли пути между вершиной источником и остальными вершинами графа).

 ### Историческая справка и информация об авторах:

 Точно неизвестно, когда поиск в ширину впервые появился. Однако известно, что Эдвард Форест Мур, американский професссор математики и информатики, изобретатель автомата Мура, предложил его в контексте поиска пути в лабиринте.
 Ли Е, китайский математик времен империй Цзинь и Юань, независимо открыл тот же алгоритм в контексте разводки проводников на печатных платах. 

 ### Идея (рисунки / анимации / видео):

 Поиск в ширину работает путём последовательного просмотра отдельных уровней графа, начиная с узла-источника u.

Рассмотрим все рёбра (u,v), выходящие из узла u. Если очередной узел v является целевым узлом, то поиск завершается; в противном случае узел v добавляется в очередь. После того, как будут проверены все рёбра, выходящие из узла u, из очереди извлекается следующий узел u, и процесс повторяется. 

#### Неформальное описание 

 1. Поместить узел, с которого начинается поиск, в изначально пустую очередь.
 2. Извлечь из начала очереди узел u и пометить его как развёрнутый.
 Примечание: Если узел u является целевым узлом, то завершить поиск с результатом "успех". В противном случае, в конец очереди добавляются все преемники узла u, которые еще не развернуты и не находятся в очереди.
 3. Если очередь пуста, то все узлы связного графа были просмотрены, следовательно, целевой узел недостижим из начального; завершается поиск с результатом "неудача". 
 4. Вернуться к пункту 2.
 Примечание: Деление вершин на развернутые и не развернутые необходимо для произвольного графа (т.к. в нем могут быть циклы). Для дерева эта операция не нужна, так как каждая вершина будет выбрана один-единственный раз.


 ### Блок-схема / Псевдокод 

Ниже приведён псевдокод алгоритма для случая, когда необходимо лишь найти целевой узел. В зависимости от конкретного применения алгоритма, может потребоваться дополнительный код, обеспечивающий сохранение нужной информации (расстояние от начального узла, узел-родитель и т. п.)

#### Рекурсивная формулировка: 

 ```sh
 BFS(start_node, goal_node) {
  return BFS'({start_node}, ∅, goal_node);
}
BFS'(fringe, visited, goal_node) {
  if(fringe == ∅) {
    // Целевой узел не найден
    return false; 
  }
  if (goal_node ∈ fringe) {
    return true;
  }
  return BFS'({child | x ∈ fringe, child ∈ expand(x)} \ visited, visited ∪ fringe, goal_node);
}
```

#### Итеративная формулировка:

```sh
BFS(start_node, goal_node) {
 for(all nodes i) visited[i] = false; // изначально список посещённых узлов пуст
 queue.push(start_node);              // начиная с узла-источника
 visited[start_node] = true;
 while(! queue.empty() ) {            // пока очередь не пуста
  node = queue.pop();                 // извлечь первый элемент в очереди
  if(node == goal_node) {
   return true;                       // проверить, не является ли текущий узел целевым
  }
  foreach(child in expand(node)) {    // все преемники текущего узла, ...
   if(visited[child] == false) {      // ... которые ещё не были посещены ...
    queue.push(child);                // ... добавить в конец очереди...
    visited[child] = true;            // ... и пометить как посещённые
   }
  }
 }
 return false;                        // Целевой узел недостижим
}
```

 ### Реализации на разных языках:

 Код был упрощен, поэтому можно сосредоточиться на алгоритме, а не на других деталях.
 #### Python

 ```sh

 import collections
def bfs(graph, root): 
    visited, queue = set(), collections.deque([root])
    visited.add(root)
    while queue: 
        vertex = queue.popleft()
        for neighbour in graph[vertex]: 
            if neighbour not in visited: 
                visited.add(neighbour) 
                queue.append(neighbour) 
if __name__ == '__main__':
    graph = {0: [1, 2], 1: [2], 2: [3], 3: [1,2]} 
    breadth_first_search(graph, 0)

 ```

 #### C++

 ```sh

 #include <iostream>
#include <list>
 
using namespace std;
 
class Graph
{
    int numVertices;
    list *adjLists;
    bool* visited;
public:
    Graph(int vertices);  
    void addEdge(int src, int dest); 
    void BFS(int startVertex);
};
 
Graph::Graph(int vertices)
{
    numVertices = vertices;
    adjLists = new list[vertices];
}
 
void Graph::addEdge(int src, int dest)
{
    adjLists[src].push_back(dest);
    adjLists[dest].push_back(src);
}
 
void Graph::BFS(int startVertex)
{
    visited = new bool[numVertices];
    for(int i = 0; i < numVertices; i++)
        visited[i] = false;
 
    list queue;
 
    visited[startVertex] = true;
    queue.push_back(startVertex);
 
    list::iterator i;
 
    while(!queue.empty())
    {
        int currVertex = queue.front();
        cout << "Visited " << currVertex << " ";
        queue.pop_front();
 
        for(i = adjLists[currVertex].begin(); i != adjLists[currVertex].end(); ++i)
        {
            int adjVertex = *i;
            if(!visited[adjVertex])
            {
                visited[adjVertex] = true;
                queue.push_back(adjVertex);
            }
        }
    }
}
 
int main()
{
    Graph g(4);
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 2);
    g.addEdge(2, 0);
    g.addEdge(2, 3);
    g.addEdge(3, 3);
    g.BFS(2);
 
    return 0;
}

 ```

 #### Java

 ```sh

import java.io.*;
import java.util.*;
 
class Graph
{
    private int numVertices;
    private LinkedList<Integer> adjLists[];
    private boolean visited[];
 
    Graph(int v)
    {
        numVertices = v;
        visited = new boolean[numVertices];
        adjLists = new LinkedList[numVertices];
        for (int i=0; i i = adjLists[currVertex].listIterator();
            while (i.hasNext())
            {
                int adjVertex = i.next();
                if (!visited[adjVertex])
                {
                    visited[adjVertex] = true;
                    queue.add(adjVertex);
                }
            }
        }
    }
 
    public static void main(String args[])
    {
        Graph g = new Graph(4);
 
        g.addEdge(0, 1);
        g.addEdge(0, 2);
        g.addEdge(1, 2);
        g.addEdge(2, 0);
        g.addEdge(2, 3);
        g.addEdge(3, 3);
 
        System.out.println("Following is Breadth First Traversal "+
                           "(starting from vertex 2)");
 
        g.BFS(2);
    }

 ```

 #### JavaScript
 
 ```javascript
 
function dfs(adj, v, t) {
	
	if(v === t) return true
	if(v.visited) return false

	v.visited = true
	
	for(let neighbor of adj[v]) {
		if(!neighbor.visited) {
			let reached = dfs(adj, neighbor, t)
			if(reached) return true
		}
	}
	return false
}

 ```

 ### Список источников:

1. Томас Х. Кормен, Чарльз И. Лейзерсон, Рональд Л. Ривест, Клиффорд Штайн. "Алгоритмы. Построение и анализ"
2. Левитин А. В. "Алгоритмы. Введение в разработку и анализ" 
