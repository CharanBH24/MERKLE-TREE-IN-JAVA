## Introduction to Merkle tree :

The Merkle Tree is an integral part of blockchain technology. It is one of the tree data structures that contains a tree full of hash and various data blocks that give an overview of everything done in the session. Creates a Hash key for all operations, to enable effective and secure content authentication with large amounts of data. Merkle Trees are used by both Bitcoin and Cryptocurrency. It also helps ensure data accuracy and quality. Since there is a unique hash key involved in everything that is done, the Merkle Tree is sometimes called the Hash Tree.

## Architecture of Merkle tree:

In blockchain, there will be separate blocks. All of these blocks are connected to the hash keys. And again each block contains a different transaction. For each function, it produces different hash keys.
For example, suppose there is one block containing 4 transactions. Each action has a unique hash key. So in all of this, we need to find one hash key for that block. Here first calculate the Hash for each function namely, Hash1, HAsh2, Hash3, Hash4 performance1, 2, 3, 4 respectively. Each of them is accelerated again like a pair of two horses. It means Hashing1 for Hash1 and Hash2, which leads to Hash12, then Hash 3 and Hash4, which leads to Hash34. Two hashes (Hash12 and Hash34) are then accelerated again to produce a Root Hash or Merkle Root. That is the root of the Merkle root.
As it is a binary tree it should have 2 leaf nodes in each node of each parent. If there is a single leaf node repeat the same hash key and make 2 leaf nodes.

![image info](https://miro.medium.com/max/447/1*1e-wyMbvf8-u7Le1LUxTBA.png)

Merkle Root is stored in the block head. The title of the block is the bitcoin block section that is accelerated throughout the mining process. At Merkle Tree, it contains the previous block hash, Nonce, and Root Hash for all activity on the current block. As a result, the combination of Merkle root on the block head makes the work more secure. Because this Root Hash contains hashes of everything that is done inside the block.

![image info](https://www.simplilearn.com/ice9/free_resources_article_thumb/Merkle_Tree_In_Blockchain_5.png)

## Algorithm

A Merkle tree is constructed by recursively hashing pairs of nodes until there is only one hash.
a, b, c, and d are some data elements (files, JSON, etc) and H is a hash function.
a hash function acts as a “digital fingerprint” of some piece of data by mapping it to a simple string with a low probability that any other piece of data will map to the same string.
Each node is created by hashing the concatenation of its “parents” in the tree.
Note: **Merkle tree are mostly a binary tree but there are also Trees. Platforms like Ethereum use non binary tree.**

## Illustration of an Algorithm:

![image info](https://media-exp1.licdn.com/dms/image/C5112AQEehgC6XD-20Q/article-inline_image-shrink_1000_1488/0/1537618459684?e=2147483647&v=beta&t=b-wCVjSoSaez-ZLGbLKhsje1X-3USDBKnsMM0SuXAWo)

## Implementation using java:

**MerkTrees.java(LOGIC)**

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;

public class MerkleTree {

    public static Node generateTree(ArrayList<String> dataBlocks) {
        ArrayList<Node> childNodes = new ArrayList<>();

        for (String message : dataBlocks) {
            childNodes.add(new Node(null, null, HashAlgorithm.generateHash(message)));
        }

        return buildTree(childNodes);
    }

    private static Node buildTree(ArrayList<Node> children) {
        ArrayList<Node> parents = new ArrayList<>();

        while (children.size() != 1) {
            int index = 0, length = children.size();
            while (index < length) {
                Node leftChild = children.get(index);
                Node rightChild = null;

                if ((index + 1) < length) {
                    rightChild = children.get(index + 1);
                } else {
                    rightChild = new Node(null, null, leftChild.getHash());
                }

                String parentHash = HashAlgorithm.generateHash(leftChild.getHash() + rightChild.getHash());
                parents.add(new Node(leftChild, rightChild, parentHash));
                index += 2;
            }
            children = parents;
            parents = new ArrayList<>();
        }
        return children.get(0);
    }

    private static void printLevelOrderTraversal(Node root) {
        if (root == null) {
            return;
        }

        if ((root.getLeft() == null && root.getRight() == null)) {
            System.out.println(root.getHash());
        }
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        queue.add(null);

        while (!queue.isEmpty()) {
            Node node = queue.poll();
            if (node != null) {
                System.out.println(node.getHash());
            } else {
                System.out.println();
                if (!queue.isEmpty()) {
                    queue.add(null);
                }
            }

            if (node != null && node.getLeft() != null) {
                queue.add(node.getLeft());
            }

            if (node != null && node.getRight() != null) {
                queue.add(node.getRight());
            }

        }

    }

    public static void main(String[] args) {
        ArrayList<String> dataBlocks = new ArrayList<>();
        dataBlocks.add("Captain America");
        dataBlocks.add("Iron Man");
        dataBlocks.add("God of thunder");
        dataBlocks.add("Doctor strange");
        Node root = generateTree(dataBlocks);
        printLevelOrderTraversal(root);
    }
}
```

## WORKING OF ALOGRITHM (OUTPUT)

Actual Output
![image info](https://1.bp.blogspot.com/-qel_meQ_-WU/YKk7LOWIabI/AAAAAAAAt-k/0AnWT3LpgM8xD-kbEznTlhLizUEZczF3QCLcBGAsYHQ/s16000/Screenshot%2B2021-05-22%2Bat%2B10.40.38%2BPM.png)

Now, let's change the s in Doctor strange to capital letter. Like Doctor Strange.
Attaching the input below for more clarity.
![image info](https://1.bp.blogspot.com/-YJcGTy1uGvI/YKk7y8-N5qI/AAAAAAAAt-s/djE5ry8AaqkEbobp-PYncTPw0IYW1KzCwCLcBGAsYHQ/s16000/Screenshot%2B2021-05-22%2Bat%2B10.41.41%2BPM.png)

We will get complete different output for that data block. So, it means, its parent hash changes and all its ancestors hash value changes in the tree like below.
![image info](https://1.bp.blogspot.com/-cqwOaUDK_h0/YKk8PcgoFzI/AAAAAAAAt-0/4froByM9UeofivSzHck5wvYxhwv7wpIKQCLcBGAsYHQ/s16000/Screenshot%2B2021-05-22%2Bat%2B10.45.07%2BPM.png)

## References:

1. [Introduction to Merkle Tree](https://www.investopedia.com/terms/m/merkle-tree.asp)
2. [Understanding Basic Merkel Algorithm](https://www.linkedin.com/pulse/merkle-tree-its-implementation-java-nikhil-goyal/)
3. [Code Implementation in Java](https://www.pranaybathini.com/2021/05/merkle-tree.html)
