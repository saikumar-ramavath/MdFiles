## Structures

#### Linked List
```
class Node {
public:
    int data;
    Node* next;
  
    // Default constructor
    Node()
    {
        data = 0;
        next = NULL;
    }
  
    // Parameterised Constructor
    Node(int data)
    {
        this->data = data;
        this->next = NULL;
    }
};
```

#### Tree
```
class TreeNode
    {
    public:
        T data;
        TreeNode<T> *left;
        TreeNode<T> *right;

        TreeNode(T data)
        {
            this -> data = data;
            left = NULL;
            right = NULL;
        }

        ~TreeNode()
        {
            if(left)
                delete left;
            if(right)
                delete right;
        }
    };
```


#### Trie

```
class Node {
    Node *links[26];
    bool flag = false;
public:

    bool containsKey(char ch) {
        return (links[ch-'a'] != NULL);
    }

    void put(char ch, Node *node) {
        links[ch-'a'] = node;
    }

    Node* get(char ch) {
        return links[ch-'a'];
    }

    void setEnd() {
        flag = true;
    }

    bool isEnd() {
        return flag;
    }

};

class Trie {

    Node* root;

public:

    /** Initialize your data structure here. */
    Trie() {
        root = new Node();
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        Node *node = root;
        for(int i=0; i<word.length(); ++i) {
            if(!node->containsKey(word[i])) {
                node->put(word[i], new Node());
            }
            node = node->get(word[i]);
        }
        node->setEnd();
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node *node = root;
        for(char ch: word) {
            if(!node->containsKey(ch))
                return false;
            node = node->get(ch);
        }
        return node->isEnd();
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node *node = root;
        for(char ch: prefix) {
              if(!node->containsKey(ch))
                return false;
             node=node->get(ch);
        }
        return true;
    }
};

```