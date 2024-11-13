＜問題＞
https://leetcode.com/problems/number-of-islands/description/

# step1

5分程度答えを見ずに考えて、手が止まるまでやってみる。
何も思いつかなければ、答えを見て解く。
ただし、コードを書くときは答えを見ないこと。
正解したら一旦OK。
思考過程もメモしてみる。

```c++
class Solution {
private:
    int row_size,column_size;
    int num_island=0;
    vector<vector<char>> recognized_island; //既に島として認識されている点を1とする。
  
//start_pointと同じ島である点を調べ上げる。
    void check_same_island (pair<int, int> start_point, vector<vector<char>>& grid) {
        queue<pair<int, int>> seen;
        vector<pair<int ,int >> connected_point = {{1,0},　{-1,0},　{0,1},　{0,1}};

        seen.push(start_point);
        while (!seen.empty()) {
            int x_connected, y_connected;
            tie(x_connected, y_connected) = seen.front();
            recognized_island[x_connected][y_connected] = '1';
            seen.pop();
            for (int i　=　0; i　<　size(connected_point); i++) {
                int x,y;
                x = x_connected + connected_point[i].first;
                y = y_connected + connected_point[i].second;
                if( x < 0 || y < 0 || x >= row_size || y >= column_size){
                    continue;
                }
                if (grid[x][y] == '0' || recognized_island[x][y] == '1'){
                    continue;
                }
                seen.push({x,y});
            }
        }
    }
public:
    int numIslands(vector<vector<char>>& grid) {
        row_size = size(grid);
        column_size = size(grid[0]);
        recognized_island.assign(row_size, vector<char>(column_size, 0));
      
        for (int i = 0; i < row_size; i++) {
            for (int j = 0; j < column_size; j++){
                if (grid[i][j] == '0' || recognized_island[i][j] == '1') {
                    continue;
                }
                check_same_island ({i,j}, grid);
                ++num_island;
            }
        }
        return num_island;
    }
};
```


【考えたこと】
- (i,j)からスタートして、幅優先探索で縦横繋がっている点を追っていく。
- (i,j)をfor文で回す。
- 通った点をわかるようにするテーブルを用意した方がよさそう。
  これにより一度通った点かどうかがわかり、(i,j)スタートで幅優先探索する必要があるかわかる。
  →最初にどういう変数を作ると良いかや、必要な処理を言語化してからコーディングを始めるとよさそうな気がする。


- アルゴリズムは頭なのかでどうやって実施したらいいかわかっていても、コーディングに落とすのが難しく感じている。
- vectorを動的に作成する必要あり.配列の初期値を作る方法を調べるのに時間がかかること多い。
https://qiita.com/alchemist/items/6cd2a86db7377ad8d236

# step2
他の方が描いたコードを見て、参考にしてコードを書き直してみる。
参考にしたコードのリンクは貼っておく。
読みやすいことを意識する。
他の解法も考えみる。

Nがノード数(ここではgridの数)、Mがエッジ数(端を除いて4N)
計算量：O(N)


```c++
class Solution {
private:
    int row_size,column_size;
    int num_island=0;
    vector<vector<char>> recognized_island; //既に島として認識されている点を1とする。
  
//start_pointと同じ島である点を調べ上げる。
    void check_same_island (pair<int, int> start_point, vector<vector<char>>& grid) {
        queue<pair<int, int>> seen;
        vector<pair<int ,int >> connected_point = {{1,0},　{-1,0},　{0,1},　{0,1}};

        seen.push(start_point);
        while (!seen.empty()) {
            int x_connected, y_connected;
            tie(x_connected, y_connected) = seen.front();
            recognized_island[x_connected][y_connected] = '1';
            seen.pop();
            for (int i　=　0; i　<　size(connected_point); i++) {
                int x,y;
                x = x_connected + connected_point[i].first;
                y = y_connected + connected_point[i].second;
                if( x < 0 || y < 0 || x >= row_size || y >= column_size){
                    continue;
                }
                if (grid[x][y] == '0' || recognized_island[x][y] == '1'){
                    continue;
                }
                seen.push({x,y});
            }
        }
    }
public:
    int numIslands(vector<vector<char>>& grid) {
        row_size = size(grid);
        column_size = size(grid[0]);
        recognized_island.assign(row_size, vector<char>(column_size, 0));
      
        for (int i = 0; i < row_size; i++) {
            for (int j = 0; j < column_size; j++){
                if (grid[i][j] == '0' || recognized_island[i][j] == '1') {
                    continue;
                }
                check_same_island ({i,j}, grid);
                ++num_island;
            }
        }
        return num_island;
    }
};
```

dscordやネットなどを調べてみましたが、step1から変更点はありません。


【以前に解かれた方のコメントから学んだこと。】

参照:https://github.com/hroc135/leetcode/pull/17#pullrequestreview-2305316186

- 参照透過性：関数を呼び出す位置によって、挙動が変わらないこと。
  https://web.sfc.keio.ac.jp/~hattori/prog-theory/ja/functional.html
- 関数名は動詞にするべき。
- 変数名の英語は省略しない。

参照:https://github.com/seal-azarashi/leetcode/pull/17#pullrequestreview-2276239814

> 不等号の式の左右の方向は、比較されるほうを左に持ってくる派と、数直線上に一直線上に並べる派がいるように思います。実際の現場においては、チームのやり方に合わせることをお勧めします。

>また、一般に、変数には肯定的な意味合いを持たせ、式の中で ! を使って否定したほうが読みやすくなると思います。今回の場合とは違うのですが、否定的な意味合いの変数を ! を使って否定すると、二重否定による肯定になり、認知負荷が上がり、読みにくく感じます。

>dfs という名前は、内部実装の分類で名前をつけているわけですが、関数の呼び出し元を読んだ人は、その内部実装を知りたいと思うことは考えにくいのです。
電源コンセントがあったら、100V 15A なのか 200V 20A なのかがまず知りたくて、石炭なのか水力なのかだけ書いてあってもそこじゃないでしょう。
だいたいの場合、関数名は中身を見なくてもだいたい何をしているか分かるようにして、変数名は何に使うかが分かればいいのです。上から読んでいって明らかで、そして、すぐに忘れて良い場合には一文字でもよいです。たとえば、0からある配列の長さまで回していたら添字だろうと推測できますね。

- ヒープ領域
聞いたことはあるが、あまり詳しくないので調べてみた。
プログラムの中で、動的にメモリを確保したり、削除したりできる。
ポインタを使ってアドレスを管理するときにはこの領域を使う。
https://ja.wikipedia.org/wiki/%E3%83%92%E3%83%BC%E3%83%97%E9%A0%98%E5%9F%9F

- スタック領域
ヒープ領域を調べているとこちらも出てきた。
関数を実行したり、ローカル変数を定義するときに使う。
https://uquest.tktk.co.jp/embedded/learning/lecture07-1.html



参照:https://github.com/thonda28/leetcode/pull/15

- 再帰でも実装できる。


参照:https://github.com/TORUS0818/leetcode/pull/19

- 深さ優先探索でも実装しておきたい。
- union find木について名前だけ聞いたことがあるレベルなので、調べてみました。
  - Discord
  https://discord.com/channels/1084280443945353267/1183683738635346001/1197738650998415500
  - https://note.nkmk.me/python-union-find/
  - Union:iとjの大親分を同じにする。find:iの一番の大親分を見つける。
  - この問題なら、(i,j)に対して上下左右が陸なら、上下左右の親を(i,j)にするという操作を全i,jに対して行い、最後(i,j)の大親分を全て探索して、大親分の数を数える。

参考:
https://github.com/fhiyo/leetcode/pull/20/files




# step3

今度は、時間を測りながら、もう一回、書きましょう。書いてアクセプトされたら文字消してもう一回書く。これを10分以内に一回もエラーを出さずに書ける状態になるまで続ける。3回続けてそれができたらその問題はOK。

実施しました。


```c++
class Solution {
private:
    int row_size,column_size;
    int num_island=0;
    vector<vector<char>> recognized_island; //既に島として認識されている点を1とする。
  
//start_pointと同じ島である点を調べ上げる。
    void check_same_island (pair<int, int> start_point, vector<vector<char>>& grid) {
        queue<pair<int, int>> seen;
        vector<pair<int ,int >> connected_point = {{1,0},　{-1,0},　{0,1},　{0,1}};

        seen.push(start_point);
        while (!seen.empty()) {
            int x_connected, y_connected;
            tie(x_connected, y_connected) = seen.front();
            recognized_island[x_connected][y_connected] = '1';
            seen.pop();
            for (int i　=　0; i　<　size(connected_point); i++) {
                int x,y;
                x = x_connected + connected_point[i].first;
                y = y_connected + connected_point[i].second;
                if( x < 0 || y < 0 || x >= row_size || y >= column_size){
                    continue;
                }
                if (grid[x][y] == '0' || recognized_island[x][y] == '1'){
                    continue;
                }
                seen.push({x,y});
            }
        }
    }
public:
    int numIslands(vector<vector<char>>& grid) {
        row_size = size(grid);
        column_size = size(grid[0]);
        recognized_island.assign(row_size, vector<char>(column_size, 0));
      
        for (int i = 0; i < row_size; i++) {
            for (int j = 0; j < column_size; j++){
                if (grid[i][j] == '0' || recognized_island[i][j] == '1') {
                    continue;
                }
                check_same_island ({i,j}, grid);
                ++num_island;
            }
        }
        return num_island;
    }
};
```
