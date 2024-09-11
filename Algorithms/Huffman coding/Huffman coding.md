![[Huffman_huff_demo.gif]]

Huffman encoding is a widely used algorithm for **lossless data compression**. It was developed by **David A. Huffman** while he was a Ph.D. student at MIT, and it is known for its efficiency in minimizing the average code length used to represent symbols in a message. Huffman encoding assigns **variable-length codes** to input characters, with shorter codes assigned to more frequent characters. This method ensures that no code is a prefix of any other code, allowing for **unique and unambiguous decoding**.

In this guide, we will dive deep into how Huffman encoding works, why it's used, and explore its practical implementation using C#. We'll also cover an example of its application and evaluate the overhead associated with Huffman encoding to highlight when it may not be the best choice.

---

## Why Use Huffman Encoding?

### Advantages

- **Efficiency**: Huffman encoding can significantly reduce the size of data files, making it ideal for applications like text compression, image compression, and more.
- **Lossless Compression**: It preserves the original data without any loss, which is crucial for applications where accuracy is paramount (e.g., text files, binary files).

### Drawbacks

- **Overhead**: While Huffman encoding can reduce the data size, there is some overhead in storing the Huffman tree metadata. For smaller data, this overhead might outweigh the benefits.
  
This brings us to an important question: **Is Huffman encoding always the best choice?** Let's find out through a detailed example and step-by-step breakdown of how the algorithm works.

---

## How Huffman Encoding Works

### Step-by-Step Process

1. **Frequency Analysis**: Calculate the frequency of each character in the input data.
2. **Build a Priority Queue**: Create a priority queue (min-heap) where each node represents a character and its frequency.
3. **Build the Huffman Tree**:
   - Extract the two nodes with the lowest frequency from the priority queue.
   - Create a new node with these two nodes as children, with a frequency equal to the sum of their frequencies.
   - Insert the new node back into the priority queue.
   - Repeat until there is only one node left, which will be the root of the Huffman tree.
4. **Generate Huffman Codes**: Traverse the Huffman tree to assign binary codes to each character. The path from the root to a character node determines its code.

---

## Huffman Encoding Example: Step-by-Step Breakdown

Consider the string `"beep boop beer!"`.

### Frequency Analysis:

| Character | Frequency |
| --------- | --------- |
| b         | 3         |
| e         | 4         |
| p         | 2         |
| o         | 2         |
| r         | 1         |
| !         | 1         |

### Build Priority Queue:

Initial priority queue:

```plaintext
[('r', 1), ('!', 1), ('p', 2), ('o', 2), ('b', 3), ('e', 4)]
```

### Build Huffman Tree (B-Tree):

```plaintext
        (*, 13)
       /       \
   (*, 5)      e(4)
  /     \
(*, 2)  b(3)
 /  \
r(1)  !(1)
```

### Generate Huffman Codes:

- `0` typically represents a move to the left child node.
- `1` typically represents a move to the right child node.

| Character | Code  |
| --------- | ----- |
| b         | 10    |
| e         | 0     |
| p         | 110   |
| o         | 111   |
| r         | 1000  |
| !         | 1001  |

### Total Bits Calculation Including Metadata

#### Bits for Encoded Data:

Total Bits = (3 × 2) + (4 × 1) + (2 × 3) + (2 × 3) + (1 × 4) + (1 × 4)  
Total Bits = 6 + 4 + 6 + 6 + 4 + 4 = **30 bits**

#### Bits for Storing the Huffman Tree:

Each unique character and its corresponding code need to be stored so that the Huffman tree can be reconstructed during decoding. For simplicity, we assume that each character and its code are stored explicitly.

### Example Huffman Codes and Frequencies

| Character | Code  | Bit Length | Frequency | Total Bits for Characters |
| --------- | ----- | ---------- | --------- | ------------------------ |
| b         | 10    | 2          | 3         | 6                        |
| e         | 0     | 1          | 4         | 4                        |
| p         | 110   | 3          | 2         | 6                        |
| o         | 111   | 3          | 2         | 6                        |
| r         | 1000  | 4          | 1         | 4                        |
| !         | 1001  | 4          | 1         | 4                        |

#### Bits for Storing the Huffman Tree

Summing up all these values:  
`13 + 12 + 14 + 14 + 15 + 15 = 83 bits`

### Grand Total

Combining the bits for the encoded data and the metadata:  
**Total Bits** = 30 (encoded data) + 83 (Huffman tree) = **113 bits**

---

## Efficiency: Does Huffman Encoding Always Save Space?

In the example above, the total number of bits required to store the string `"beep boop beer!"` using Huffman encoding, including the Huffman tree metadata, is **113 bits**. However, the original uncompressed message in ASCII requires only **104 bits** (13 characters x 8 bits/character).

This highlights that **Huffman encoding may not always be beneficial** for short strings, as the overhead of encoding the Huffman tree can negate the benefits of compression. For larger datasets, though, Huffman encoding typically leads to significant space savings.

---

## C# Implementation of Huffman Encoding

To fully grasp how Huffman encoding works, here's a practical implementation in **C#**. The following code defines the Huffman tree, encoding, and decoding logic.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;

public class HuffmanNode {
    public char Character { get; set; }
    public int Frequency { get; set; }
    public HuffmanNode Left { get; set; }
    public HuffmanNode Right { get; set; }

    public List<bool> Traverse(char character, List<bool> data) {
        if (Right == null && Left == null) {
            if (character.Equals(this.Character))
                return data;
            return null;
        }
        else {
            List<bool> leftPath = null;
            List<bool> rightPath = null;

            if (Left != null) {
                var leftList = new List<bool>(data) { false };
                leftPath = Left.Traverse(character, leftList);
            }

            if (Right != null) {
                var rightList = new List<bool>(data) { true };
                rightPath = Right.Traverse(character, rightList);
            }

            return leftPath ?? rightPath;
        }
    }
}

public class HuffmanTree {
    private List<HuffmanNode> nodes = new List<HuffmanNode>();
    public HuffmanNode Root { get; set; }
    public Dictionary<char, int> Frequencies = new Dictionary<char, int>();

    public void Build(string source) {
        foreach (char ch in source) {
            if (!Frequencies.ContainsKey(ch))
                Frequencies.Add(ch, 0);

            Frequencies[ch]++;
        }

        foreach (var symbol in Frequencies) {
            nodes.Add(new HuffmanNode() { Character = symbol.Key, Frequency = symbol.Value });
        }

        while (nodes.Count > 1) {
            var orderedNodes = nodes.OrderBy(node => node.Frequency).ToList();

            if (orderedNodes.Count >= 2) {
                var taken = orderedNodes.Take(2).ToList();

                var parent = new HuffmanNode() {
                    Character = '*',
                    Frequency = taken[0].Frequency + taken[1].Frequency,
                    Left = taken[0],
                    Right = taken[1]
                };

                nodes.Remove(taken[0]);
                nodes.Remove(taken[1]);
                nodes.Add(parent);
            }

            Root = nodes.FirstOrDefault();
        }
    }

    public BitArray Encode(string source) {
        var encodedSource = new List<bool>();

        foreach (char ch in source) {
            var encodedSymbol = Root.Traverse(ch, new List<bool>());
            encodedSource.AddRange(encodedSymbol);
        }

        return new BitArray(encodedSource.ToArray());
    }

    public string Decode(BitArray bits) {
        var current = Root;
        var decoded = "";

        foreach (bool bit in bits) {
            current = bit ? current.Right : current.Left;

            if (IsLeaf(current)) {
                decoded += current.Character;
                current = Root;
            }
        }

        return decoded;
    }

    public bool IsLeaf(HuffmanNode node) { 
        return (node.Left == null && node.Right == null); 
    }
}

public class Program {
    public static void Main() {
        var huffmanTree = new HuffmanTree();

        Console.WriteLine("Enter the string to encode:");
        string source = Console.ReadLine();

        huffmanTree.Build(source);

        Console.WriteLine("Encoding...");
       

 BitArray encoded = huffmanTree.Encode(source);

        Console.WriteLine("Encoded bit array:");
        foreach (bool bit in encoded) {
            Console.Write(bit ? '1' : '0');
        }

        Console.WriteLine();

        Console.WriteLine("Decoding...");
        string decoded = huffmanTree.Decode(encoded);

        Console.WriteLine("Decoded string: " + decoded);
    }
}
```

---

## Conclusion

Huffman encoding is a highly efficient and well-known **lossless compression algorithm**. While it reduces the size of data, it is important to consider the overhead required to store the Huffman tree. In smaller datasets, the metadata can offset the benefits of encoding. For large data sets, however, Huffman coding offers significant compression advantages.

**Key Takeaway**: Huffman encoding is optimal when the data has a high variance in character frequencies and the dataset size justifies the tree overhead. If the dataset is too small, alternatives like Run-Length Encoding (RLE) or simply storing the data uncompressed might be more efficient.