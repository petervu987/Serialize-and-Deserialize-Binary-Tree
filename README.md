# Serialize-and-Deserialize-Binary-Tree

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "null";

        string result;
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            TreeNode* curr = q.front();
            q.pop();

            if (curr) {
                result += to_string(curr->val) + ",";
                q.push(curr->left);
                q.push(curr->right);
            } else {
                result += "null,";
            }
        }

        return result;
    }

    TreeNode* deserialize(string data) {
        if (data == "null") {
            return nullptr;
        }

        vector<string> n;
        stringstream ss(data);
        string token;

        while (getline(ss, token, ',')) {
            n.push_back(token);
        }

        TreeNode* root = new TreeNode(stoi(n[0]));
        queue<TreeNode*> q;
        q.push(root);

        int index = 1;

        while (!q.empty() && index < n.size()) {
            TreeNode* parent = q.front();
            q.pop();

            if (n[index] != "null") {
                parent->left = new TreeNode(stoi(n[index]));
                q.push(parent->left);
            }
            index++;

            if (index < n.size() && n[index] != "null") {
                parent->right = new TreeNode(stoi(n[index]));
                q.push(parent->right);
            }
            index++;
        }

        return root;    
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
