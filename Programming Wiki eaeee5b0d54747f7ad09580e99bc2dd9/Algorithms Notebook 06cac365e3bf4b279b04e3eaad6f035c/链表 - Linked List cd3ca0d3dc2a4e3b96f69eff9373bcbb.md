# 链表 - Linked List

[How to Implement a Linked List in JavaScript](https://www.freecodecamp.org/news/implementing-a-linked-list-in-javascript/)

![%E9%93%BE%E8%A1%A8%20-%20Linked%20List%20cd3ca0d3dc2a4e3b96f69eff9373bcbb/Untitled.png](%E9%93%BE%E8%A1%A8%20-%20Linked%20List%20cd3ca0d3dc2a4e3b96f69eff9373bcbb/Untitled.png)

- ***单向链表*** 就是只有next指针指向下一个 Singly Linked Lists

    ![%E9%93%BE%E8%A1%A8%20-%20Linked%20List%20cd3ca0d3dc2a4e3b96f69eff9373bcbb/Untitled%201.png](%E9%93%BE%E8%A1%A8%20-%20Linked%20List%20cd3ca0d3dc2a4e3b96f69eff9373bcbb/Untitled%201.png)

    In JavaScript, a linked list looks like this:

    ```jsx
    const list = {
        head: {
            value: 6
            next: {
                value: 10                                             
                next: {
                    value: 12
                    next: {
                        value: 3
                        next: null	
                        }
                    }
                }
            }
        }
    };
    ```

- ***双向链表*** 就是多一个pre指针指回前一个 - Doubly Linked Lists
- ***循环链表*** 就是tail node会指向第一个node - Circular Linked Lists

![%E9%93%BE%E8%A1%A8%20-%20Linked%20List%20cd3ca0d3dc2a4e3b96f69eff9373bcbb/Untitled%202.png](%E9%93%BE%E8%A1%A8%20-%20Linked%20List%20cd3ca0d3dc2a4e3b96f69eff9373bcbb/Untitled%202.png)

### JS Linked List

```jsx
class ListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class LinkedList {
  constructor(head = null) {
    this.head = head;
  }

  size() {
    let count = 0;
    let node = this.head;
    while (node) {
      count++;
      node = node.next;
    }
    return count;
  }

	

  clear() {
    this.head = null;
  }

  getFirst() {
    return this.head;
  }

  getLast() {
    let lastNode = this.head;
    if (lastNode) {
      while (lastNode.next) {
        lastNode = lastNode.next;
      }
    }
    return lastNode;
  }
}

let node1 = new ListNode(1);
let node2 = new ListNode(2);
let node3 = new ListNode(3);

node1.next = node2;
node2.next = node3;

let list = new LinkedList(node1);

console.log(list.head.next.data);
```