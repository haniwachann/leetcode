＜問題＞
https://leetcode.com/problems/maximum-depth-of-binary-tree/

# step1

5分程度答えを見ずに考えて、手が止まるまでやってみる。
何も思いつかなければ、答えを見て解く。
ただし、コードを書くときは答えを見ないこと。
正解したら一旦OK。
思考過程もメモしてみる。

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        TreeNode* current_node;
        TreeNode* left_node;
        TreeNode* right_node;
        map<TreeNode*, int> height;
        queue<TreeNode*> node_to_visit;

        current_node=root;
        height[current_node] = 1;
        node_to_visit.push(current_node);

        while (current_node != nullptr && !node_to_visit.empty()) {
            current_node = node_to_visit.front(); 
            if (current_node->left != nullptr) {
                left_node = current_node->left;
                node_to_visit.push(left_node);
                height[left_node] = height[current_node] + 1;
            }
            if (current_node->right != nullptr) {
                right_node = current_node->right;
                node_to_visit.push(right_node);
                height[right_node] = height[current_node] + 1;
            }
            node_to_visit.pop();
        }
        return height.rbegin()->second;
    }
};
```

【考えたこと】

- 幅優先探索、深さ優先探索で解けそう。
- 木を辿っていく際に、自分の高さを保管しておく。

　　次に辿った高さ＝今の高さ＋1

- mapのキーとして、ノードのアドレスを指定するのは自然なのか。
- コードを書く中で存在しないアドレスに何度もアクセスして苦労した。

# step2
他の方が描いたコードを見て、参考にしてコードを書き直してみる。
参考にしたコードのリンクは貼っておく。
読みやすいことを意識する。
他の解法も考えみる。
計算量：O(N)　N:ノード数


```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        TreeNode* current_node;
        map<TreeNode*, int> height;
        queue<TreeNode*> node_to_visit;
        if (root == nullptr){
            return 0;
        }
        current_node = root;
        node_to_visit.push(current_node);
        height[current_node] = 1;
        max_depth=0;
        while (!node_to_visit.empty()) {
            current_node = node_to_visit.front();
            node_to_visit.pop();
            max_depth = max(max_depth, height[current_node])
            if (current_node->left != nullptr) {
                node_to_visit.push(current_node->left);
                height[current_node->left] = height[current_node] + 1;
            }
            if (current_node->right != nullptr) {
                node_to_visit.push(current_node->right);
                height[current_node->right] = height[current_node] + 1;
            }
        }
        return max_depth;
    }
};
```

- 空の配列を渡された時にも対応できるように修正
- TreeNodeの変数でleft_nodeとright_nodeに分けていただが、変数までは作る必要がないかと判断。
- スペースを修正しました。
- frontとpopはなるべく近づけました
- 上記解き方は、幅優先探索。
- step1でのrbegin()では必ずしも最大深さにはならないので、たどるノードの最大値を出すように修正

- pairはあまり使わないほうが良い
https://github.com/rossy0213/leetcode/pull/10#discussion_r1564630314

- 深さ優先探索ならstackで深さとノードを保持して、その都度今までの最大深さと今の深さを比較して、深さの最大値を更新していけば良い。
https://github.com/Mike0121/LeetCode/pull/6/files

- 同じ高さのノードをwhileの中で全て調べ切れば、各ノードの深さをずっと覚えておく必要はない。

- もちろん再帰でも書ける。

参考：https://github.com/hayashi-ay/leetcode/pull/22

- 幅優先探索でdequeに、ノードと高さを組みで取り出しと入れていく。

https://github.com/fhiyo/leetcode/pull/23#discussion_r1675990961


# step3

今度は、時間を測りながら、もう一回、書きましょう。書いてアクセプトされたら文字消してもう一回書く。これを10分以内に一回もエラーを出さずに書ける状態になるまで続ける。3回続けてそれができたらその問題はOK。

実施しました。


```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        TreeNode* current_node;
        map<TreeNode*, int> height;
        queue<TreeNode*> node_to_visit;
        if (root == nullptr){
            return 0;
        }
        current_node = root;
        node_to_visit.push(current_node);
        height[current_node] = 1;
        max_depth=0;
        while (!node_to_visit.empty()) {
            current_node = node_to_visit.front();
            node_to_visit.pop();
            max_depth = max(max_depth, height[current_node])
            if (current_node->left != nullptr) {
                node_to_visit.push(current_node->left);
                height[current_node->left] = height[current_node] + 1;
            }
            if (current_node->right != nullptr) {
                node_to_visit.push(current_node->right);
                height[current_node->right] = height[current_node] + 1;
            }
        }
        return max_depth;
    }
};
```
