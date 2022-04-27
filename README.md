# merkle_tree

# **Introduction to merkle tree**

The Merkle tree, also known as the hash tree, is a data structure that is used to verify and synchronise data.
It's a data structure in which each non-leaf node is a hash of its children. All of the leaf nodes are the same depth and are positioned as far to the left as possible.
It uses hash functions to maintain data integrity.

![image](https://user-images.githubusercontent.com/88684012/165547043-57c04913-97d1-40bc-8e96-fbf19e49ac05.png)

# The top hash of this binary merkel tree is a hash of the entire tree.


- The tree's structure enables for rapid mapping of large amounts of data, and slight changes to the data can be easily spotted.
- If we want to find out where data has changed, we can check if the data is consistent with the root hash, which means we won't have to traverse the entire structure but only a tiny portion of it.
- The fingerprint for the entire data is the root hash.

# Applications: 

- Merkle trees come be handy in distributed systems when the same data needs to be stored in several locations.
- Inconsistencies can be checked using Merkle trees.
- Merkle trees are used by Apache Cassandra to discover inconsistencies between database replicas.
- Bitcoin and blockchain both use it.

# Algorithm of remove function.
- It will take tree and key as parameters.
- If the tree is null then return null.
- If the key is smaller than the tree->key then tree->left is equal to remove(tree->left, key) and return tree.
- If the key is greater than the tree->key then tree->right is equal to remove(tree->right, key) and return tree.
- else if the tree->left is equal to null and the tree->right is equal to null then decrement the size and return tree->left.
- else if the tree->left is not equal to null and the tree->right is equal to null then decrement the size and return tree->left.
- else if tree->left is equal to null and tree->right is not equal to null then decrement the size and return tree->right.
- else assign tree->left to a pointer called left of data type node.
- While left->right is not equal to null, left is equal to left->right.
- tree->key is equal to left->key.
- tree->value is equal to left->value.
- tree->left is equal to remove(tree->left, tree->key).
- Return tree.
