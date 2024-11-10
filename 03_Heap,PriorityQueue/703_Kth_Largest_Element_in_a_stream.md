＜問題＞
https://leetcode.com/problems/kth-largest-element-in-a-stream/description/

# step1

5分程度答えを見ずに考えて、手が止まるまでやってみる。
何も思いつかなければ、答えを見て解く。
ただし、コードを書くときは答えを見ないこと。
正解したら一旦OK。
思考過程もメモしてみる。

```c++
class KthLargest {
public:

    vector<int> scores;
    int kth;

    KthLargest(int k, vector<int>& nums) {
        scores=nums;
        sort(scores.rbegin(),scores.rend());
        kth=k;
    }
    
    int add(int val) {
        scores.push_back(val);
        sort(scores.rbegin(),scores.rend());
        if((int)size(scores)>kth){
            return scores[kth-1];    
        }else{
            return scores[kth-1];
        }
    }
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

【考えたこと】
- k番目を取り出すとのことなどで、要素くわえるごとにソートすればいいと思った。
- classのメンバ変数とメンバ関数、外部の関数との変数などのやりとりに慣れておらず、変数をpublicに用意しないといけないことなど少し戸惑う。もっとclassになれないといけない。
- k番目の値がないときは場合分けした。（想定しない値が入ったときの例外処理を書くことが自然にできないので、練習が必要。
- はじめのsortでO（NlogN）、追加ごとにO(n)をクエリk回分だけ実施する。

# step2
他の方が描いたコードを見て、参考にしてコードを書き直してみる。
参考にしたコードのリンクは貼っておく。
読みやすいことを意識する。
他の解法も考えみる。

計算量：O(N)
N:sの文字サイズ

```c++
class KthLargest {
public:

    map<int,int> canditates;
    int kth;

    KthLargest(int k, vector<int>& nums) {
        for(int i=0;i<k;i++){
            if(canditates.count(nums[i])!=0){
                canditates[nums[i]]++;
            }else{
                canditates[nums[i]]=1;
            }
        }

        for(int i=k;i<(int) size(nums);i++){
            if(canditates.count(nums[i])!=0){
                canditates[nums[i]]++;
            }else{
                canditates[nums[i]]=1;
            }

            if(canditates.begin()->second==1){
                canditates.erase(canditates.begin());
            }else{
                canditates.begin()->second--;
            }
        }

        kth=k;
    }
    
    int add(int val) {
        if(canditates.count(val)!=0){
            canditates[val]++;
        }else{
            canditates[val]=1;
        }

        if(canditates.begin()->second==1){
            canditates.erase(canditates.begin());
        }else{
            canditates.begin()->second--;
        }

        auto iter=canditates.begin();
        int ans= iter->first;
        return ans;
    }
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

まず(大きほうから)k番目以降を保持する必要がない。
すなわち、k個のデータ構造に1個追加されたときに、
一番小さいものを削除すればよい→min heapを使える。
https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0703.Kth%20Largest%20Element%20in%20a%20Stream/README_EN.md

priority queueは一度実装しておいたほうがよい。
https://discord.com/channels/1084280443945353267/1192736784354918470/1194613857046503444

priority queueの前にmapを使った実装を考えるべき
step2ではmapをつかって実装したが、いまいちわかりにくいコードな気がする。
mapのキーを与えられた値として、値の個数を管理する。
mapはk個の値を持つ。

https://discord.com/channels/1084280443945353267/1183683738635346001/1185264362508795984
https://discord.com/channels/1084280443945353267/1200089668901937312/1202182322229882920

重複ありのmapもある
https://cpprefjp.github.io/reference/map/multimap.html

C++はmapが平衡2分木
https://pyteyon.hatenablog.com/entry/2019/01/01/220850#map-%E3%81%AE%E3%83%87%E3%83%BC%E3%82%BF%E6%A7%8B%E9%80%A02%E5%88%86%E6%9C%A8


選択肢が見えなさすぎる感じがする。
一つ見つけたら、思考停止してしまう。

- 参考

https://github.com/kazukiii/leetcode/pull/9/files
https://github.com/cheeseNA/leetcode/pull/12/files
https://github.com/TORUS0818/leetcode/pull/10/files
https://github.com/nittoco/leetcode/pull/11/files
https://github.com/Ryotaro25/leetcode_first60/pull/9/files
https://github.com/goto-untrapped/Arai60/pull/23/files

# step3

今度は、時間を測りながら、もう一回、書きましょう。書いてアクセプトされたら文字消してもう一回書く。これを10分以内に一回もエラーを出さずに書ける状態になるまで続ける。3回続けてそれができたらその問題はOK。

実施しました。

# step4_1

レビューを受けて、コードを修正する。
再度3回連続acceptされるまで続ける。

```c++
class KthLargest {
private:
    map<int,int> canditates;
    int kth;

public:
    KthLargest(int k, vector<int>& nums) {
        kth=k;

        for (int i=0; i<k; i++) {
            canditates[nums[i]]++;
        }
        for (int i=k; i<size(nums); i++) {
            canditates[nums[i]]++;
            canditates.begin()->second--;
            if (canditates.begin()->second<=0) {
                canditates.erase(canditates.begin());
            }
        }
    }
    int add(int val) {
        canditates[val]++;
        canditates.begin()->second--;
        if (canditates.begin()->second<=0) {
            canditates.erase(canditates.begin());
        }
        return canditates.begin()->first;
    }
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

# step4_2
関数に処理をまとめました。

```C++
class KthLargest {
private:
    map<int,int> candidates;
    int kth;
    void add_num_to_candidates(int nums_add){
        candidates[nums_add]++;
        candidates.begin()->second--;
        if (candidates.begin()->second<=0) {
            candidates.erase(candidates.begin());
        }
    }
public:
    KthLargest(int k, vector<int>& nums) {
        kth=k;

        for (int i=0; i<k; i++) candidates[nums[i]]++;
        for (int i=k; i<size(nums); i++) add_num_to_candidates(nums[i]);
    }
    int add(int val) {
        add_num_to_candidates(val);
        return candidates.begin()->first;
    }
};
```

# step4_3
・初期配列が空の時を対処。
・配列がkth以下の時は削除処理をしないように修正。
```c++
class KthLargest {
private:
    map<int,int> candidates;
    int candidates_counter=0;
    int kth;
    void add_num_to_candidates(int nums_add){
        candidates[nums_add]++;
        candidates_counter++;
        if (candidates_counter>kth) {
            candidates.begin()->second--;
            if (candidates.begin()->second<=0) {
                candidates.erase(candidates.begin());
            }
        }
    }
public:
    KthLargest(int k, vector<int>& nums) {
        kth=k;

        for (int i=0; i<k && i<size(nums); i++){
            candidates[nums[i]]++;
            candidates_counter++;
        }
        for (int i=k; i<size(nums); i++) add_num_to_candidates(nums[i]);
    }
    int add(int val) {
        add_num_to_candidates(val);
        return candidates.begin()->first;
    }
};
```