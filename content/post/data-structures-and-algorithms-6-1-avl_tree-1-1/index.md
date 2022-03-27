---
title: Data Structures and Algorithms 6-1. AVL_Tree (1/1)
date: 2022-03-27T16:16:45.213Z
summary: "***Binary Search Tree, AVL Tree***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. binary_search_tree

```cpp
#include <iostream>
using namespace std;
typedef struct Node{
	Node(int key = 0, Node* lchild = nullptr, Node* rchild = nullptr) 
	: key(key), lchild(lchild), rchild(rchild) {}
	int key;
	Node *lchild, *rchild;
} Node;

inline Node* getNewNode(int key) {
	return new Node(key);
}

Node* insertNode(Node* root, int key) {
	if (root == nullptr) return getNewNode(key);
	if (root->key == key) return root;
	if (key < root->key) {
		root->lchild = insertNode(root->lchild, key);
	} else {
		root->rchild = insertNode(root->rchild, key);
	}
	return root;
}

int predecessor(Node* root) {
	Node* temp = root->lchild;
	while (temp->rchild) temp = temp->rchild;
	return temp->key;
}

Node* eraseNode(Node* root, int key) {
	if (root == nullptr) return root;
	if (key < root->key) {
		root->lchild = eraseNode(root->lchild, key);
	} else if (key > root->key) {
		root->rchild = eraseNode(root->rchild, key);
	} else {
		if (root->lchild == nullptr && root->rchild == nullptr) {
			delete root;
			return nullptr;
		} else if (root->lchild == nullptr || root->rchild == nullptr) {
			Node* temp = root->lchild ? root->lchild : root->rchild;
			delete root;
			return temp;
		} else {
			int preval = predecessor(root);
			root->key = preval;
			root->lchild = eraseNode(root->lchild, preval);
		}
	}
	return root;
}


void clear(Node* root) {
	if (root == nullptr) return ;
	clear(root->lchild);
	clear(root->rchild);
	delete root;
	return ;
}

void output(Node* root) {
	if (root == nullptr) return ;
	output(root->lchild);
	cout << root->key << " ";
	output(root->rchild);
	return ;
}

int main() {
	Node* root = nullptr;
	int op, key;
	while (cin >> op >> key) {
		switch (op) {
			case 0 : {
				root = insertNode(root, key);
				break;
			}
			case 1 : {
				root = eraseNode(root, key);
				break;
			}
		}
		output(root);
		putchar(10);
	}
	clear(root);
	return 0;
}
```

## 2. AVL Tree

```cpp
#include <iostream>
using namespace std;
#define NIL (&Node::_NIL)

typedef struct Node{
	Node(int key = 0, int height = 0, Node* lchild = NIL, Node* rchild = NIL) 
	: key(key), lchild(lchild), rchild(rchild), height(height) {}
	int key, height;
	Node *lchild, *rchild;
	static Node _NIL;
} Node;

Node Node::_NIL;

inline Node* getNewNode(int key) {
	return new Node(key, 1);
}

void update_height(Node* root) {
	root->height = max(root->lchild->height, root->rchild->height) + 1;
	return ;
}

Node* left_rotate(Node* root) {
	Node* new_root = root->rchild;
	root->rchild = new_root->lchild;
	new_root->lchild = root;
	update_height(root);
	update_height(new_root);
	return new_root;
}

Node* right_rotate(Node* root) {
	Node* new_root = root->lchild;
	root->lchild = new_root->rchild;
	new_root->rchild = root;
	update_height(root);
	update_height(new_root);
	return new_root;
}

Node* maintain(Node* root) {
	if (abs(root->lchild->height - root->rchild->height) < 2) return root;
	if (root->lchild->height > root->rchild->height) {
		if (root->lchild->rchild->height > root->lchild->lchild->height) { //LR
			root->lchild = left_rotate(root->lchild);
			cout << "LR" << endl;
		}
		root = right_rotate(root);//LL
		cout << "LL" << endl;
	} else {
		if (root->rchild->lchild->height > root->rchild->rchild->height) { //RL
			root->rchild = right_rotate(root->rchild);
			cout << "RL" << endl;
		}
		root = left_rotate(root);//RR
		cout << "RR" << endl;
	}
	return root;
}

Node* insertNode(Node* root, int key) {
	if (root == NIL) return getNewNode(key);
	if (root->key == key) return root;
	if (key < root->key) {
		root->lchild = insertNode(root->lchild, key);
	} else {
		root->rchild = insertNode(root->rchild, key);
	}
	update_height(root);
	return maintain(root);
}

int predecessor(Node* root) {
	Node* temp = root->lchild;
	while (temp->rchild != NIL) temp = temp->rchild;
	return temp->key;
}

Node* eraseNode(Node* root, int key) {
	if (root == NIL) return root;
	if (key < root->key) {
		root->lchild = eraseNode(root->lchild, key);
	} else if (key > root->key) {
		root->rchild = eraseNode(root->rchild, key);
	} else {
		if (root->lchild == NIL || root->rchild == NIL) {
			Node* temp = root->lchild ? root->lchild : root->rchild;
			delete root;
			return temp;
		} else {
			int preval = predecessor(root);
			root->key = preval;
			root->lchild = eraseNode(root->lchild, preval);
		}
	}
	update_height(root);
	return maintain(root);
}


void clear(Node* root) {
	if (root == NIL) return ;
	clear(root->lchild);
	clear(root->rchild);
	delete root;
	return ;
}

void output(Node* root) {
	if (root == NIL) return ;
	output(root->lchild);
	cout << root->key << " ";
	output(root->rchild);
	return ;
}

int main() {
	Node* root = NIL;
	int op, key;
	while (cin >> op >> key) {
		switch (op) {
			case 0 : {
				root = insertNode(root, key);
				break;
			}
			case 1 : {
				root = eraseNode(root, key);
				break;
			}
		}
		output(root);
		putchar(10);
	}
	clear(root);
	return 0;
}
```