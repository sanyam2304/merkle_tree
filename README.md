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

# For example:

** Given a list of alphabets, create a merkel tree from it.

The bottom most layer of the tree would contain all the letters as the leaf nodes.

![image](https://user-images.githubusercontent.com/88684012/165562646-dc3889b9-16c3-4428-a0e9-2d7b35631285.png)
> Lowest layer of the tree would contain the data in each node

The layer above contains its hash values.
![image](https://user-images.githubusercontent.com/88684012/165562767-f09eb43e-3dc1-4913-86ae-726915fe891f.png)
> The layer above the leaf node has the hash values of leaf node data

The nodes in the layer after the second layer are contains the hash value of the child nodes. Generally we take two nodes from the second layer and combine them to form another node. We can take more than two nodes as well but binary merkel trees is the simplest of them all and increasing the degree of nodes only increase the computation and algorithms complexity.

If we have even number of nodes, we take 2 consecutive nodes and form the parent layer. But if we have odd number of nodes, we take two consecutive nodes until one is left to form the parent layer, and then we repeat the remaining node by copying the hash to the parent layer.

![image](https://user-images.githubusercontent.com/88684012/165562827-eeb2d0b2-cc9f-4410-a3d3-c0d6b0a897be.png)
> Layer 3 has the hash of the values of the 2 consecutive nodes of layer 2 and in case we have odd nodes in a layer the last node is repeated

Similarly the fourth layer is formed using the values of the third layer.
![image](https://user-images.githubusercontent.com/88684012/165563155-a83bd2a7-33eb-4775-9d63-2533dd4a6182.png)
>The fourth layer is formed by the hash of the values of the 2 consecutive nodes of layer 2
The final layer or the root of the Merkel Tree is formed by hash value of the last two nodes remaining in top most layer. In any case, odd or even leaf nodes, we will always have two nodes in the top most layer.

![image](https://user-images.githubusercontent.com/88684012/165563244-915196c2-3f07-45e7-9509-e6ef5bed5eaf.png)
> Merkel Tree formed by the five letters

# Verification

The importance Merkel Trees is in its ability to verify data with efficiency. Given any data from the list we can verify in O(h) time complexity that this data is valid or not. Moreover, we do not need the entire list for verification.

A much simpler form of Merkel Tree is a hash chain or simply a blockchain in which each node has the hash of the previous node’s value. If we tamper any node in between we can identify in O(n) time whether the node is tampered or not. Verification in hash chain can be performed by calculating the hash of all the nodes starting from the node in question and go till the end. In a situation where we have with multiple nodes to verify, we start with the node that is first among all the suspected nodes and calculate the hash of the last node from then on. Now that we have the hash of last node, we can compare and check if this hash matches or not. Hash chain seems simple but is not an efficient choice for large data objects. Since we need the entire chain physically present with us to verify the data, it makes hash chains space inefficient as well.

This is not the case with verification in Merkel Tree. To illustrate the verification process consider the example below.

** Suppose I received a data C from another server. Lets say this is C’. We want to verify C’ is not tampered. We have in out possession a merkel tree of all the data in the list.

In case of a hash chain we would need the entire list of data to verify that C’ is correct. In Merkel Tree we only need the hashes. Following diagram illustrates how we can verify, C’ without other data objects available with us.
![image](https://user-images.githubusercontent.com/88684012/165563632-7a4060ff-063a-47fc-9a80-0523075d8f6b.png)
>Verifying C’ by hashing all the nodes that lead us to the root
- Find the position of the C’ in the list. Probably by searching by id.
- Calculate the the hash of C’
- Calculate the value of the parent node by hashing the current node with its neighbor ( next if position is odd and previous if position in even) and set the parent as the current node.
- Repeat step 3 until we find the root
- Compare the root with the previous root, if they match then C’

Compare the new root with the existing root. If the new root matches then the C’ is essentially C and not tampered.

To verify a data in hash chain we need O(n) time since we would calculate n hashes in the worst case where as in case of Merkel Tree the same data can be verified in O(logn) time since we only calculate logn hashes.

# Creation

As already mentioned before, Merkel Tree are created by taking two nodes from each layer and hashing them to create the parent node. by representing the tree in matrix form we can mathematically write it as:
![image](https://user-images.githubusercontent.com/88684012/165563958-c06beac4-fbf1-4169-846b-710f0c2c199c.png)
> This makes the root of the tree available at tree[0][0]

# Verification

Verification is a bottom-up approach where we start from the data, find its hash and calculate the parent and continue this until we find the root. Mathematically, we can express it as follows:
![image](https://user-images.githubusercontent.com/88684012/165564202-788dcb87-989b-4632-bfe5-c7e874aa1243.png)


