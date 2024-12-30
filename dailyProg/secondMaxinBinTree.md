Given the root to a binary search tree, find the second largest node in the tree.







=========================================================================================================


To find the second largest node in a binary search tree (BST), we can leverage the properties of the BST:

1. The largest node in a BST is the rightmost node.
2. The second largest node is either:
   - The parent of the largest node if the largest node does not have a left subtree.
   - The largest node in the left subtree of the largest node if it has a left subtree.

Here’s how we can implement this in Python:

```python
class TreeNode:
    def __init__(self, value=0, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

def find_second_largest(root):
    if not root or (not root.left and not root.right):
        raise ValueError("Tree must have at least two nodes")

    current = root

    while current:
        # Case 1: Current is the largest, and it has a left subtree.
        # The second largest is the largest in the left subtree.
        if current.left and not current.right:
            return find_largest(current.left)

        # Case 2: Current is the parent of the largest node,
        # and the largest node has no children.
        if current.right and not current.right.left and not current.right.right:
            return current.value

        # Move to the right in the tree.
        current = current.right

def find_largest(node):
    while node.right:
        node = node.right
    return node.value

# Example Usage
if __name__ == "__main__":
    # Create a sample BST:
    #       10
    #      /  \
    #     5    15
    #         /  \
    #        12   20
    #                \
    #                 25

    root = TreeNode(10)
    root.left = TreeNode(5)
    root.right = TreeNode(15)
    root.right.left = TreeNode(12)
    root.right.right = TreeNode(20)
    root.right.right.right = TreeNode(25)

    print("Second largest node:", find_second_largest(root))  # Output: 20
```

### **Explanation of the Code:**

1. **Base Case:**
   - If the tree has less than two nodes, it’s not possible to find the second largest node.

2. **Case Analysis:**
   - If the largest node (rightmost node) has a left subtree, the second largest node is the largest in that subtree.
   - Otherwise, the second largest is the parent of the largest node.

3. **Traversal:**
   - Traverse the tree to find the rightmost node while keeping track of the conditions for the second largest.

This approach ensures an \(O(h)\) time complexity, where \(h\) is the height of the tree.
