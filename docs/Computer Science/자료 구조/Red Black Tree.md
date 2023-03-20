---  
category: Computer Science/자료구조  
share: true  
---  
  
### Red / black tree  
> 참고 자료  
> - [자바로 구현하고 배우는 자료구조 > 2-1. 규칙 : 부스트코스](https://www.boostcourse.org/cs204/lecture/626067?isDesc=false)  
> - [Red-Black Trees : Properties, Black Height and Proof of its height](https://www.codesdope.com/course/data-structures-red-black-trees/)  
> - [Red-Black Tree (Fully Explained, with Java Code)](https://www.happycoders.eu/algorithms/red-black-tree-java/)  
> - [Red-Black Tree - wikipedia](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)  
> - [An Introduction To Binary Search And Red Black Tree](https://www.topcoder.com/thrive/articles/An%20Introduction%20to%20Binary%20Search%20and%20Red-Black%20Trees)  
> [Introduction to Red-Black Tree - GeeksforGeeks](https://www.geeksforgeeks.org/introduction-to-red-black-tree/)  
  
  
- Why Red-Black Trees?  
Most of the BST operations (e.g., search, max, min, insert, delete.. etc) take O(h) time where h is the height of the BST. The cost of these operations may become O(n) for a skewed Binary tree. If we make sure that the height of the tree remains O(log n) after every insertion and deletion, then we can guarantee an upper bound of O(log n) for all these operations. The height of a Red-Black tree is always O(log n) where n is the number of nodes in the tree.   
  
![Pasted image 20221220152814.png](../../attachments/Pasted%20image%2020221220152814.png)  
  
  
  
  
- 실제로 어떻게 쓰이는지 궁금해서 구글링  
1. DB 에서는 인덱스 걸 때 씁니다.  
읽어보면 재미있는 연관 글: [데이터베이스 인덱스는 왜 'B-Tree'를 선택하였는가 :: 이뇽의세상](https://helloinyong.tistory.com/296)  
  
Red-black trees are often used in database management systems (DBMSs) to implement index structures, which can be used to improve the performance of queries. An index is a data structure that allows efficient access to rows in a table based on the values of one or more columns.  
  
In a DBMS, red-black trees can be used to implement a B-tree index, which is a type of balanced tree index that can be used to support efficient insertion, deletion, and search operations. B-trees are well-suited for use in a DBMS because they can be used to index large amounts of data that may not fit in main memory.  
  
Red-black trees can also be used to implement other types of index structures, such as R-trees, which are used to index spatial data, and B+-trees, which are used to index data in a database.  
  
Overall, red-black trees are useful in DBMSs because they provide fast search, insertion, and deletion times, which can help improve the performance of queries. They are often used in the implementation of index structures, which can be used to speed up the execution of queries by allowing the DBMS to quickly locate the rows that match a given search condition.  
  
2. arrays, maps, and sets 객체를 구현할 때 사용합니다.   
   They are commonly used in the implementation of associative arrays, maps, and sets in programming languages.  
  Red-black trees can be implemented in a variety of programming languages, including C, C++, Java, and Python. They are often used in the implementation of the Standard Template Library (STL) in C++, as well as in the java.util package in Java.  
  
- <mark class="yellow">Red Black Tree의 규칙</mark>  
레드 블랙 트리는 자가 균형 이진 탐색 트리로서, AVL 트리처럼 스스로 균형을 잡는 트리입니다.   
레드 블랙 트리에서는 다음의 규칙에 따라 정렬하여 균형을 맞춥니다.  
  
1. 모든 노드는 빨간색이나 검은색입니다.  
2. <font style="color:olivedrab">루트</font>는 항상 **검은색**입니다.  
3. <font style="color:olivedrab">새로 추가되는 노드</font>는 항상 <font style="color:indianred">빨간색</font>입니다.  
4. 루트에서 잎 노드로 가는 <font style="color:olivedrab">모든 경로</font>에는 같은 수의 **검은색 노드**가 있어야 합니다.  
5. 어떤 경로에서도 <font style="color:indianred">빨간색 노드</font> 2개가 <font style="color:olivedrab">연속</font>으로 있어서는 안 됩니다.  
6. <font style="color:olivedrab">모든 빈 노드</font>는 **검은색**이라고 가정합니다.  
  
- **균형을 맞추는 방법**  
![Pasted image 20221219110520.png](../../attachments/Pasted%20image%2020221219110520.png)  
1) 이모 노드가 검은색일 경우  
회전을 합니다. 회전을 하고 나면 부모 노드는 검은색, 두 자식 노드는 빨간색이 되어야 합니다.  
   
2) 이모 노드가 빨간색일 경우  
색상 전환을 합니다. 색상 전환을 하고 나면 부모 노드는 빨간색, 두 자식 노드는 검은색이 되어야 합니다.  
  
노드, 루트 포인터, 현재 사이즈 (상수시간 안에 트리 크기 알 수 있음)  
#### Red Black Tree 구현하기  
동작 방식은 AVL 트리와 거의 동일합니다. 단, isLeftChild가 추가되기 때문에 이를 고려해주어야 합니다.  
  
- **클래스**  
레드 블랙 트리 클래스는 불리언 값을 가진 black로 참이면 검은색, 거짓이면 빨간색을 표시합니다.   
그리고 이모 노드를 알아내기 위해 left, right, parent 노드를 가리키는 포인터뿐만 아니라 불리언 값을 가진 isLeftChild를 사용합니다.  
  
생성자 클래스에서는 루트 값을 초기화하고 크기는 0이 되도록 초기화합니다.  
각각의 노드는 키와 값으로 이루어지고 이를 처리할 수 있도록 내부 클래스를 만듭니다.  
  
  
```java  
public class RedBlackTree<K,V> implements RedBlackI<K,V> {  
	// 2개의 전역 변수가 있다. root, size  
	Node<K,V> root;   
	int size;  
	  
	class Node<K,V> {  
		K key;  
		V value;  
		Node<K,V> left, right, parent;  
		// black 값이 참이면 노드는 검은색, 거짓이면 빨간색  
		// isLeftChild: 규칙을 망가뜨린 노드의 이모 노드가 무슨 색인지 알기 위함  
		boolean isLeftChild, black;   
		  
		public Node (K key, V value) {  
			this.key = key;  
			this.value = value;  
			left = right = parent = null;  
			black = false; // 새로운 노드는 빨간색  
			isLeftChild = false;   
		}  
	}  
}  
```  
  
마지막으로 각각의 노드에 왼쪽 자식인지 오른쪽 자식인지에 대한 정보를 저장함 (규칙을 망가뜨린 노드의 이모 노드가 무슨 색인지 알기 위함입니다.)   
→ 예: 만약 부모가 왼쪽 자식이라면 이모 노드는 오른쪽에 있습니다.  
  
- **Adding Nodes**    
  
add 메소드에 대한 코드는 다음과 같습니다. 트리가 비어있으면 노드를 추가하고 비어있지 않다면 재귀 add 메소드를 호출합니다.  
```java  
// AVL 클래스의 생성자  
public AVLTree () {  
	// AVL Tree 생성  
	root = null;  
	currentSize = 0;  
}  
  
  
public void add (E obj) {  
	Node<E> node = new Node<E>(obj); // 새로운 노드 생성  
	// 트리가 비어있을 경우 처음 요소를 루트 노드로 넣음  
	if (root == null) {  
		root = node;  
		currentSize++;  
		return;  
	}  
	// 트리에 노드가 있을 경우 add 메소드를 재귀로 호출  
	// 2개의 인자를 인자로 받음   
	// (새로운 요소를 넣을지 확인할 위치의 노드, 새롭게 추가할 노드)  
	add(root, node); // Recursive Add method  
}  
```  
  
- **Recursive Add method**  
```java  
public void add(Node<E> parent, Node<E> newNode){  
	// new Node의 data가 parent의 data보다 크면 트리의 오른쪽에 추가합니다.  
	if (((Comparable<E>)newNode.data.compareTo(parent.data) > 0) {  
		if (parent.right == null) {  
			parent.right = newNode;  
			newNode.parent = parent;  
			currentSize++;  
		} else {  
			add(parent.right, newNode);  
		}  
	// newNode의 data가 parent의 data보다 작거나 같으면 트리의 왼쪽에 추가하면 됩니다.  
	else {  
		if (parent.left == null){  
			parent.left = newNode;  
			newNode.parent = parent;  
			currentSize++;  
		}  
		else {  
			add(parent.left, newNode);  
		}  
	// AVL 트리가 규칙에 맞게 잘 되어있는지 확인합니다.  
	checkBalance(newNode);  
}  
```  
  
  
  
  
- these are a translation of a 2-3 tree (see below)  
- In practice:  
	Red–black trees offer worst-case guarantees for insertion time, deletion time, and search time.  
	Not only does this make them valuable in time-sensitive applications such as real-time applications,  
	but it makes them valuable building blocks in other data structures which provide worst-case guarantees;  
	for example, many data structures used in computational geometry can be based on red–black trees, and the Completely Fair Scheduler used in current Linux kernels uses red–black trees. In the version 8 of Java, the Collection HashMap has been modified such that instead of using a LinkedList to store identical elements with poor hashcodes, a Red-Black tree is used.  
  
  
