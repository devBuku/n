1. Often called a node-based data structure.

2. **Insertion:** Can be done in constant time, no matter the length of the list, as long as we have a reference to the insertion position and the necessary nodes.

3. **Deletion:** Can be done in constant time, no matter the length of the list, as long as we have a reference to the node and the necessary surrounding nodes.

4. ==I am not considering the time taken for traversal in the above statements.==

5. **Get head / tail:** Getting the head is constant time. Getting the tail is also constant time as long as we have a reference to both the head and the tail.

6. **Deletion in the middle and at the end:**
   - Singly linked list: Deleting a node requires access to the previous node. If we only have a reference to the node being deleted, we need to traverse the list to find the previous node.
   - Doubly linked list: We can access the previous node directly using `prev`, so deletion is constant time if we have a reference to the node being deleted.
   - Deleting the tail in a singly linked list requires traversal to find the previous node. In a doubly linked list, we can access the previous node directly using `tail.prev`.

7. **Prepend and append:** Can be constant time if we maintain references to the head and tail.
   - Singly linked list: Prepend is constant time. Append is also constant time if we maintain a tail reference.
   - Doubly linked list: Prepend and append are constant time if we maintain references to both the head and tail.

1. The ordering of operations matters because if we do `c.next = undefined` before accessing `d`, we can never get access to `d` again, unless we have another reference to it.