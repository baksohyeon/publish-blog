---  
category: Computer Science 💻/Data Structure  
share: true  
date created: Monday, January 16th 2023, 3:20:49 pm  
date modified: Wednesday, January 18th 2023, 5:01:30 pm  
---  
# Basic Concept  
![Pasted image 20230110111557.png](../../attachments/Pasted%20image%2020230110111557.png)  
위 그림에서 각 점은 node 혹은 vertex라고 말하고 두 점을 잇는 선을 edge라고 한다. 그래프는 V(vertex 혹은 node), E(edge)의 집합이다. 따라서 G(V, E)로 표현하기도 한다.  
  
  
BFS stands for Breadth-First Search. It is an algorithm used to traverse or search through a graph or tree data structure.  
  
The basic idea behind BFS is to start at a specific node and explore all of its neighbors (nodes directly connected to it) before moving on to any other nodes. This is done by adding the neighbors of the current node to a queue, and then visiting them one by one. When all of the neighbors of the current node have been visited, the algorithm moves on to the next node in the queue and repeats the process.  
  
This continues until the entire graph or tree has been traversed, or until the desired node is found.  
  
BFS has a number of applications, including finding the shortest path between two nodes in a graph and testing whether a graph is bipartite. It can also be used to find connected components in a graph, and to solve puzzles such as the "maze" problem, where the goal is to find a path from a starting point to an ending point in a maze.  
  
  
Time complexity of the BFS algorithm: $O(V+E)$  
  
Space complexity of the BFS algorithm: $O(V)$  
  
# Code  
## The pseudocode of the BFS algorithm  
  
```python  
BFS(graph, startVertex)  
  let queue be a Queue  
  mark startVertex as visited and enqueue startVertex onto queue  
  while queue is not empty  
    dequeue the first vertex currentVertex from queue  
    for each edge (currentVertex, neighbor) do  
      if neighbor has not been visited  
        mark neighbor as visited  
        enqueue neighbor onto queue  
  
```  
Here, G is the graph and s is the starting vertex. The algorithm begins by marking the starting vertex as visited and adding it to a queue. It then iterates through the queue, taking the first vertex v and examining all of its edges. If one of the edges leads to a vertex w that has not yet been visited, the algorithm marks w as visited and adds it to the queue. This process continues until the queue is empty, at which point the algorithm has examined all of the vertices in the graph.  
  
  
The BFS algorithm starts at the startVertex and explores all of its neighbors before moving on to the next level of vertices.   
It does this by using a queue to store the vertices that need to be visited. The algorithm marks each vertex as it is visited, so that it does not revisit any vertices.  
  
  
  
```php  
procedure BFS_Algorithm(graph, initial_vertex):  
  let frontier be a queue  
  let visited_vertex be a list  
  add the initial_vertex in the frontier  
  while True:  
    if frontier is empty then  
      print("No Solution Found")  
      break  
  
  
selected_node = remove the first node of the frontier  
add the selected_node to the visited_vertex list  
  
// Check if the selected_node is the solution  
if selected_node is the solution then  
  print(selected_node)  
  break  
  
// Extend the node  
new_nodes = extend the selected_node  
// Add the extended nodes in the frontier  
for all nodes from new_nodes do  
  if node not in visited_vertex and node not in frontier then  
  add node at the end of the queue  
```  
  
## TypeScript  
```ts  
class Node<T> {  
    data: T;  
    adjNodes: Node<T>[];  
    comparator: (a: T, b: T) => number;  
  
    constructor(data: T, comparator: (a: T, b: T) => number) {  
        this.data = data;  
        this.adjNodes = new Array<Node<T>>();  
        this.comparator = comparator;  
    }  
}  
  
class Graph<T> {  
    nodes: Map<T, Node<T>> = new Map<T, Node<T>>();  
    comparator: (a: T, b: T) => number;  
    root: Node<T>;  
  
    constructor(comparator: (a: T, b: T) => number, data: T) {  
        this.comparator = comparator;  
        this.root = new Node<T>(data, comparator);  
    }  
}  
  
/**  
 * breadth-first search traversal of a node  
 * @param {Graph<T>} graph - the Graph to be traversed  
 * @param {Node<T>} startNode - the start node  
 * @returns void  
 */  
  
export function bfs<T> (graph: Graph<T>, startNode: Node<T>){  
    if (!startNode){  
        return;  
    }  
    const visited: Node<T>[] = [];  
    const queue: Node<T>[] = [];  
  
    queue.push(startNode);  
  
    while (queue.length !== 0){  
        let current: Node<T>|undefined = queue.shift();  
        if (!current){  
            continue;  
        }  
  
        console.log(current.data);  
  
        visited.push(current);  
        current.adjNodes.forEach(node => {  
            // if the node has not been visited  
            if (visited.indexOf(node) === -1){  
                visited.push(node);  
                queue.push(node);  
            }  
        })  
    }  
}  
  
```  
  
  
## C++  
- cpp  
```cpp  
#include<stdio.h>  
#include<conio.h>  
#include<stdlib.h>  
  
typedef struct tree  
{  
	int a;  
	struct tree *left;  
	struct tree *right;	  
}node;  
  
node *queue[100];  
int front = -1;  
int rear = 0;  
  
void enqueue(node *root)  
{	  
	queue[rear] = root;  
	rear++;		  
}  
  
node* dequeue()  
{  
	front++;  
	node *c = queue[front];  
	return c;  
}  
  
void levelorder(node *root)  
{  
	node *q;  
	printf("\n");  
	enqueue(root);  
	while(rear-1!=front)  
	{  
//		printf("\nrear = %d , front = %d",rear , front);  
		q = dequeue();		  
		printf("%d\t",q->a);  
		if(q->left)  
			enqueue(q->left);  
		if(q->right)  
			enqueue(q->right);		  
	  
	  
	}  
	  
	  
	  
}  
  
  
int main()  
{  
	int n , i;  
	node *p , *q , *root;  
	printf("Enter the number of nodes");  
	scanf("%d",&n);	  
	  
	for(i=0;i<n;i++)  
	{  
		if(i == 0)  
		{  
			p = (node*)malloc(sizeof(node));  
			scanf("%d",&p->a);  
			p->left = NULL;  
			p->right = NULL;  
			q = p;  
			root = p;  
		}  
		else  
		{	  
			p = (node*)malloc(sizeof(node));  
			scanf("%d",&p->a);  
			p->left = NULL;  
			p->right = NULL;  
			q = root;  
			while(1)  
			{				  
				if(p->a > q->a)  
				{  
					if(q->right == NULL)  
						{					  
						q->right = p;  
						break;  
						}  
					else  
						q = q->right;  
				}  
				else  
				{  
					if(q->left == NULL)  
						{						  
						q->left = p;  
						break;  
						}  
					else  
						q = q->left;										  
				}				  
			}  
			  
		}  
		  
	}  
	  
  
printf("Level order Traversal is :- ");  
levelorder(root);	  
	  
return 0;  
}  
  
```  
  
# Implement BFS to find the shortest route in maze  
  
  
![ The initial maze](Pasted%20image%2020230110112338.png)  
  
  
![ The Graph that represents the above maze](Pasted%20image%2020230110113728.png)  
  
  
  
  
![ The way that Breadth-First Search Algorithm searches for a solution in the search space](Pasted%20image%2020230110113758.png)  
  
  
  
![ The path from node S to node I](Pasted%20image%2020230110113853.png)  
  
The BFS algorithm is a way to traverse or search a graph. It works by starting at a given vertex (called the "start vertex") and exploring all of its neighbors before moving on to the next level of vertices. It does this using a queue to store the vertices that still need to be visited.  
  
To implement the BFS algorithm to find the shortest route in a maze, you can represent the maze as a graph, with each cell in the maze being a vertex in the graph and each path between cells being an edge. Then, you can use the BFS algorithm to search the graph for the shortest path from the start vertex (the starting cell in the maze) to the end vertex (the goal cell in the maze).  
  
  
the algorithm returns the solution that is closest to the root, so for problems that the transition from one node to its children nodes costs one, the BFS algorithm returns the best solution.   
  
```js  
shortestPath(maze, startCell, goalCell)  
  let graph be a graph representation of the maze, with each cell in the maze being a vertex and each path between cells being an edge  
  let startVertex be the vertex in the graph corresponding to the startCell  
  let goalVertex be the vertex in the graph corresponding to the goalCell  
  // Use the BFS algorithm to search the graph for the shortest path  
  BFS(graph, startVertex)  
  // Once the BFS algorithm has finished running, the goalVertex will have been marked as visited  
  // We can use this information to reconstruct the shortest path from the startVertex to the goalVertex  
  return reconstructPath(startVertex, goalVertex)  
  
reconstructPath(startVertex, goalVertex)  
  // Implement this function to traverse the graph from the goalVertex back to the startVertex, using the information stored in the graph about which vertices have been visited  
  // Return the shortest path as a list of cells in the maze  
  
```  
  
  
- **Why BFS algorithm ensure the shortest route in maze?**  
The BFS algorithm is guaranteed to find the shortest path in a graph if the edges have uniform weight (that is, if all of the edges have the same weight).   
This is because the BFS algorithm explores the vertices in the graph in a breadth-first manner, meaning that it will always visit the vertices that are closest to the start vertex first. Therefore, if the goal vertex is at a shorter distance from the start vertex (measured in terms of number of edges), the BFS algorithm will find it before it finds a goal vertex that is farther away.  
  
  
  
- **In what cases is the BFS algorithm not guaranteed to find the shortest path?**  
[Why is BFS not guaranteed to provide the shortest route in most cases? - Quora](https://www.quora.com/Why-is-BFS-not-guaranteed-to-provide-the-shortest-route-in-most-cases)  
  
![Pasted image 20230110114523.png](../../attachments/Pasted%20image%2020230110114523.png)  
  
If the edges in the graph do not have uniform weight (that is, if the edges have different weights), then the BFS algorithm is not guaranteed to find the shortest path.   
  
In this case, you would need to use a different algorithm, such as `Dijkstra's algorithm` or the `A* search algorithm`, which are designed to find the shortest path in a graph with weighted edges.   
  
  
  
# 참고 자료  
[Solve Maze Using Breadth-First Search (BFS) Algorithm in Python](https://plainenglish.io/blog/solve-maze-using-breadth-first-search-bfs-algorithm-in-python-7931acbe8a93)  
[Data Structures in TypeScript - Graph - Ricardo Borges](https://ricardoborges.dev/data-structures-in-typescript-graph)  
[[그래프]길찾기 bfs 알고리즘(maze algorithm)](https://jackpot53.tistory.com/123)  
[How to implement Breadth First Search (BFS)Algorithm in Typescript](https://ehizmantutored.hashnode.dev/breadth-first-search-algorithm)