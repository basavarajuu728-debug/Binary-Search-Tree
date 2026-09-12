# Binary-Search-Tree
#include <iostream>
using namespace std;

// =====================================================
// BST NODE
// =====================================================

template <typename T>
struct Node
{
    T data;
    Node<T>* left;
    Node<T>* right;

    Node(T value)
    {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};


// =====================================================
// BINARY SEARCH TREE CLASS
// =====================================================

template <typename T>
class BST
{
private:
    Node<T>* root;

    // -------------------------------------------------
    // INSERT
    // -------------------------------------------------
    Node<T>* insert(Node<T>* node, T value)
    {
        if (node == nullptr)
        {
            return new Node<T>(value);
        }

        if (value < node->data)
        {
            node->left = insert(node->left, value);
        }
        else if (value > node->data)
        {
            node->right = insert(node->right, value);
        }
        else
        {
            cout << "Duplicate value not allowed: "
                 << value << endl;
        }

        return node;
    }

    // -------------------------------------------------
    // SEARCH
    // -------------------------------------------------
    bool search(Node<T>* node, T value)
    {
        if (node == nullptr)
        {
            return false;
        }

        if (node->data == value)
        {
            return true;
        }

        if (value < node->data)
        {
            return search(node->left, value);
        }

        return search(node->right, value);
    }

    // -------------------------------------------------
    // FIND MINIMUM NODE
    // -------------------------------------------------
    Node<T>* findMin(Node<T>* node)
    {
        Node<T>* current = node;

        while (current != nullptr &&
               current->left != nullptr)
        {
            current = current->left;
        }

        return current;
    }

    // -------------------------------------------------
    // DELETE
    // -------------------------------------------------
    Node<T>* remove(Node<T>* node, T value)
    {
        if (node == nullptr)
        {
            return nullptr;
        }

        // Search in left subtree
        if (value < node->data)
        {
            node->left = remove(node->left, value);
        }

        // Search in right subtree
        else if (value > node->data)
        {
            node->right = remove(node->right, value);
        }

        // Node found
        else
        {
            // Case 1: No child
            if (node->left == nullptr &&
                node->right == nullptr)
            {
                delete node;
                return nullptr;
            }

            // Case 2: Only right child
            else if (node->left == nullptr)
            {
                Node<T>* temp = node->right;
                delete node;
                return temp;
            }

            // Case 3: Only left child
            else if (node->right == nullptr)
            {
                Node<T>* temp = node->left;
                delete node;
                return temp;
            }

            // Case 4: Two children
            else
            {
                Node<T>* temp = findMin(node->right);

                node->data = temp->data;

                node->right =
                    remove(node->right, temp->data);
            }
        }

        return node;
    }

    // -------------------------------------------------
    // IN-ORDER TRAVERSAL
    // -------------------------------------------------
    void inOrder(Node<T>* node)
    {
        if (node == nullptr)
        {
            return;
        }

        inOrder(node->left);
        cout << node->data << " ";
        inOrder(node->right);
    }

    // -------------------------------------------------
    // PRE-ORDER TRAVERSAL
    // -------------------------------------------------
    void preOrder(Node<T>* node)
    {
        if (node == nullptr)
        {
            return;
        }

        cout << node->data << " ";
        preOrder(node->left);
        preOrder(node->right);
    }

    // -------------------------------------------------
    // POST-ORDER TRAVERSAL
    // -------------------------------------------------
    void postOrder(Node<T>* node)
    {
        if (node == nullptr)
        {
            return;
        }

        postOrder(node->left);
        postOrder(node->right);
        cout << node->data << " ";
    }

    // -------------------------------------------------
    // DESTROY TREE
    // -------------------------------------------------
    void destroyTree(Node<T>* node)
    {
        if (node == nullptr)
        {
            return;
        }

        destroyTree(node->left);
        destroyTree(node->right);

        delete node;
    }

public:

    // -------------------------------------------------
    // CONSTRUCTOR
    // -------------------------------------------------
    BST()
    {
        root = nullptr;
    }

    // -------------------------------------------------
    // DESTRUCTOR
    // -------------------------------------------------
    ~BST()
    {
        destroyTree(root);
        root = nullptr;
    }

    // -------------------------------------------------
    // PUBLIC INSERT
    // -------------------------------------------------
    void insert(T value)
    {
        root = insert(root, value);
    }

    // -------------------------------------------------
    // PUBLIC SEARCH
    // -------------------------------------------------
    bool search(T value)
    {
        return search(root, value);
    }

    // -------------------------------------------------
    // PUBLIC DELETE
    // -------------------------------------------------
    void remove(T value)
    {
        if (!search(value))
        {
            cout << "Value " << value
                 << " not found in BST.\n";
            return;
        }

        root = remove(root, value);

        cout << "Value " << value
             << " deleted successfully.\n";
    }

    // -------------------------------------------------
    // PUBLIC IN-ORDER
    // -------------------------------------------------
    void inOrder()
    {
        cout << "In-Order: ";
        inOrder(root);
        cout << endl;
    }

    // -------------------------------------------------
    // PUBLIC PRE-ORDER
    // -------------------------------------------------
    void preOrder()
    {
        cout << "Pre-Order: ";
        preOrder(root);
        cout << endl;
    }

    // -------------------------------------------------
    // PUBLIC POST-ORDER
    // -------------------------------------------------
    void postOrder()
    {
        cout << "Post-Order: ";
        postOrder(root);
        cout << endl;
    }
};


// =====================================================
// MAIN FUNCTION
// =====================================================

int main()
{
    BST<int> tree;

    int choice;
    int value;

    do
    {
        cout << "\n";
        cout << "=====================================\n";
        cout << "       BINARY SEARCH TREE\n";
        cout << "=====================================\n";
        cout << "1. Insert\n";
        cout << "2. Search\n";
        cout << "3. Delete\n";
        cout << "4. In-Order Traversal\n";
        cout << "5. Pre-Order Traversal\n";
        cout << "6. Post-Order Traversal\n";
        cout << "7. Exit\n";
        cout << "=====================================\n";

        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            cout << "Enter value to insert: ";
            cin >> value;

            tree.insert(value);

            cout << "Value inserted successfully.\n";
            break;

        case 2:
            cout << "Enter value to search: ";
            cin >> value;

            if (tree.search(value))
            {
                cout << "Value " << value
                     << " found in BST.\n";
            }
            else
            {
                cout << "Value " << value
                     << " not found in BST.\n";
            }
            break;

        case 3:
            cout << "Enter value to delete: ";
            cin >> value;

            tree.remove(value);
            break;

        case 4:
            tree.inOrder();
            break;

        case 5:
            tree.preOrder();
            break;

        case 6:
            tree.postOrder();
            break;

        case 7:
            cout << "\nExiting program...\n";
            cout << "Memory cleaned successfully.\n";
            break;

        default:
            cout << "Invalid choice! Please try again.\n";
        }

    } while (choice != 7);

    return 0;
}