---
title: Data Structures and Algorithms 6-2. Red_Black_Tree (1/1)
date: 2022-03-27T16:23:33.165Z
summary: "***Red Black Tree***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. implementation

```cpp
#include <bits/stdc++.h>
using namespace std;
#define NIL &(Node::_NIL)
struct Node{
	int color, key;
	Node* left, *right;
	Node(int key = 0, int color = 0, Node* left = NIL, Node* right = NIL) :
	key(key), color(color), left(left), right(right) {}
	static Node _NIL;
};
Node Node::_NIL(0, 1);

Node* getNewNode(int key) {
	return new Node(key);
}
bool hasRedChild(Node* root) {
	return root->left->color == 0 || root->right->color == 0;
}

Node* left_rotate(Node* root) {
	Node* temp = root->right;
	root->right = temp->left;
	temp->left = root;
	return temp;
}

Node* right_rotate(Node* root) {
	Node* temp = root->left;
	root->left = temp->right;
	temp->right = root;
	return temp;
}

Node* InsertAdjust(Node* root) {
	int flag = 0;
	if (root->left->color == 0 && hasRedChild(root->left)) flag = 1;
	if (root->right->color == 0 && hasRedChild(root->right)) flag = 2;
	if (flag == 0) return root;
	if (root->left->color == 0 && root->right->color == 0) {
		root->color = 0;
		root->left->color = root->right->color = 1;
		return root;
	}
	if (flag == 1) {
		if (root->left->right->color == 0) {
			root->left = left_rotate(root->left);
		}
		root = right_rotate(root);
	} else {
		if (root->right->left->color == 0) {
			root->right = right_rotate(root->right);
		}
		root = left_rotate(root);
	}
	root->color = 0;
	root->left->color = root->right->color = 1;
	return root;
}

Node* _insertNode(Node* root, int key) {
	if (root == NIL) return getNewNode(key);
	if (root->key == key) return root;
	if (key < root->key) {
		root->left = _insertNode(root->left, key);
	} else {
		root->right = _insertNode(root->right, key);
	}
	return InsertAdjust(root);
}

Node* insertNode(Node* root, int key) {
	root = _insertNode(root, key);
	root->color = 1;
	return root;
}

int predecessor(Node* root) {
	Node* temp = root->left;
	while (temp->right != NIL) temp = temp->right;
	return temp->key;
}

Node* eraseAdjust(Node* root) {
	int flag = 0;
	if (root->left->color != 2 && root->right->color != 2) return root;
	if (hasRedChild(root)) {
		root->color = 0;
		if (root->left->color == 0) {
			root = right_rotate(root->right);
			flag = 1;
		} else {
			root = left_rotate(root->left);
			flag = 2;
		}
		root->color = 1;
		if (flag == 1) {
			root->right = eraseAdjust(root->right);
		} else {
			root->left = eraseAdjust(root->left);
		}
	} else {
		if ((root->left->color == 1 && !hasRedChild(root->left)) 
		|| (root->right->color == 1 && !hasRedChild(root->right))) {
			root->color += 1;
			root->left->color -= 1;
			root->right->color -= 1;
			return root;
		} else {
			if (root->left->color == 1) { //L
				root->right->color = 1;
				if (root->left->left->color == 1) { //R
					root->left->color -= 1;
					root->left = left_rotate(root->left);
					root->left->color += 1;
				}
				root->left->color = root->color;
				root = right_rotate(root);
			} else { // R
				root->left->color = 1;
				if (root->right->right->color == 1) { //L
					root->right->color -= 1;
					root->right = right_rotate(root->right);
					root->right->color += 1;
				}
				root->right->color = root->color;
				root = left_rotate(root);
			}
			root->left->color = root->right->color = 1;
		}
	}
	return root;
}

Node* _eraseNode(Node* root, int key) {
	if (root == NIL) return root;
	if (key < root->key) {
		root->left = _eraseNode(root->left, key);
	} else if (key > root->key) {
		root->right = _eraseNode(root->right, key);
	} else {
		if (root->left == NIL || root->right == NIL) {
			Node* temp = root->left == NIL ? root->right : root->left;
			temp->color += root->color;
			delete(root);
			return temp;
		} else {
			int val = predecessor(root);
			root->key = val;
			root->left = _eraseNode(root->left, val);
		}
	}
	return eraseAdjust(root);
}

Node* eraseNode(Node* root, int key) {
	root = _eraseNode(root, key);
	root->color = 1;
	return root;
}

void clear(Node* root) {
	if (root == NIL) return ;
	clear(root->left);
	clear(root->right);
	delete(root);
	return ;
}


void print(Node* root) {
	printf("%d | %d (%d, %d)\n", root->color, root->key, root->left->key, root->right->key);
	return ;
}

void output(Node* root) {
	if (root == NIL) return ;
	print(root);
	output(root->left);
	output(root->right);
	return ;
}

int main() {
	Node* root = NIL;
	int op, key;
	while (cin >> op >> key) {
		cout << "=======rbtree begin======" << endl;
		if (op == 1) root = insertNode(root, key);
		if (op == 2) root = eraseNode(root, key);
		output(root);
		cout << "=======rbtree end======" << endl;
	}
	clear(root);
	return 0;
}
```