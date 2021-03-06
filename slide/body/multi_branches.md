
# Multi branches (branch, checkout, merge, rebase)

## まえおき

次に,branchingによりversionが分岐**する**場合,作業環境の管理toolとしてgitを扱う方法を体験しましょう.

gitのbranchingは,プログラミング言語における`**env`のように,仮想環境を作って実行環境を分離するのと似た効果をもたらします.

## 学んだことからできること 

- 複数人で一つのProjectを変更する (Remoteとの連携がMust) 
- Taskごとに作業環境を分離する

## git log 

基本の呪文
Commit treeを表示する

 `git log --oneline --graph --all`

---

defaultの`git log`は見にくいので,いい感じで表示してくれる[wrapper](https://github.com/takaaki-kasai/git-foresta)もある.

```{sh, eval=F}
$ git-foresta --all | less -RSX
99d7024c 2019-02-19 10:31  ○ ameono (master) Added file003
2f98c925 2019-02-19 10:31  ● ameono Added file001 and file002.
bf1b31ad 2019-02-19 10:31  │ ○ ameono (HEAD) Added gomi.
fb7cc7b0 2019-02-19 10:31  │ ● ameono 再帰で検索して「もしかして：再帰」をクリックすると…。
469a6426 2019-02-19 10:31  │ ● ameono Added file002.
f8c8773e 2019-02-19 10:31  │ ● ameono Added file001.
                           ├─┘
2312317f 2019-02-19 10:30  ■ ameono Added file000.
```

---

logの表示だけでなくCommitとかを便利にできるWrapperもある

[tig](https://github.com/jonas/tig)

---

### 思い出す 

保存されているCommit (=Version)の親子関係を表すtree (一番重要).

- CommitはdirectoryのスナップショットにIDとしてhash値を振ったもの.
- Commitには親となるCommitが記録されている (👉 tree構造)
- 1度作ったcommitは基本的になくならない(探せなくはなるので注意) 
- Hash値だけで管理すると人間が使うとき不便なのでHEADとかmasterみたいな看板(refs)を使う
- master, HEAD, branchなどは特定のCommitを指しているrefsの名前(**重要**)

<div align="center">
<img src="https://git-scm.com/book/en/v2/images/advance-master.png" width="650px">
</div>


これからやる作業は基本的にrefsを作ったり移動したりする作業

## git branch

<branch-name>という名前の新たなrefsを作り,<start-point>を指すようにする

`git branch [-d|-D] <branch-name> [<start-point>]`

---

### やってみる

```{sh, eval=F}
# 準備
cd path/to/workspace
mkdir repo005 # 新しいGit repositoeryを作る
cd repo005
git init
echo "ROW000" > file000
git add file000 
git commit -m "Added file000."
```

---


```{sh, eval=F}
git branch feature/update_file000 master
```

feature/branch000という名前のrefを作る(指す先はmasterの指しているところ)

## git branchをなかったことにする

branchを削除する.

```{sh, eval=F}
git branch -d feature/update_file000
```

未mergeのbranchは削除しようとするエラーが出る(二度と見つけられなくなるため)ので強制削除する必要がある.

強制的に削除する場合.
```{sh, eval=F}
git branch -D feature/update_file000
```


## branchの削除をなかったことにする 

```{sh, eval=F}
$ git branch -d feature/update_file000
Deleted branch feature/update_file000 (was e170b24). # <-- ここのhashを覚えておく 
```

```{sh, eval=F}
git checkout e170b24 
git checkout -b feature/update_file000
```

## git checkout

HEADの指す先を<branch> OR <commit>に変える.

`git checkout [-b] [<branch>|<commit>]`


---

### やってみる

```{sh, eval=F}
git checkout feature/update_file000
```

HEADの指す先をfeature/branch000へ変える

branchの作成とcheckoutをいっぺんにやる

```{sh, eval=F}
git checkout -b feature/update_file000
```

--- 

### checkoutで起こること

gitを使ったことのある人は,checkoutに他の枝(branch)に移るというイメージを持っているかもしれないが,実際にはHEADの指す先のrefsが変わるだけ.

👉 e.g., masterもdevelopも同じCommitを指していたら,どちらにcheckoutしてもWorking directoryは変化しない. 同じCommitをmasterというrefs越しに見るかdevelopというrefs越しに見るかが違うだけ.

---

```{sh, eval=F}
git branch testing 
```

<img src="https://git-scm.com/book/en/v2/images/head-to-master.png" width="400px">

```{sh, eval=F}
git checkout testing 
```


<img src="https://git-scm.com/book/en/v2/images/head-to-testing.png" width="400px">

--- 

CLIで見てみる

```{sh, eval=F}
$ git branch feature/update_file000
$ git log --graph --all
* commit e170b24890ca2685d9414b15f3862a28b16b3828 (HEAD -> master, feature/branch000)
  Author: ameono <4m3on0@gmail.com>
  Date:   Sat Feb 16 21:28:33 2019 +0900

      Added file000.
```

```{sh, eval=F}
$ git checkout feature/update_file000
$ git log --graph --all
* commit e170b24890ca2685d9414b15f3862a28b16b3828 (HEAD -> feature/branch000, master)
  Author: ameono <4m3on0@gmail.com>
  Date:   Sat Feb 16 21:28:33 2019 +0900

      Added file000.
```
HEAD -> の指す先に注目.

---

`<branch>`にcheckoutした状態でCommitすると`<branch>`refsの位置はHEADに連動して移動する.


--- 

### Commitしてみる 

```{sh, eval=F}
echo "ROW001" >> file000
git add file000 
git commit -m "Updated file000 (added the second row)." 
```

masterと違うCommitを指していることを確認
```{sh, eval=F}
git log --oneline --graph --all
```

---

### branchを変えて再度commitしてみる 

```{sh, eval=F}
git checkout master
cat file000 # update_file000ブランチの変更が反映されていないことを確認 
cp file000 file001 
git add file001
git commit -m "Added file001."
```

分岐したことを確認
```{sh, eval=F}
git log --oneline --graph --all
```

---

このようにしてTaskごとに作業環境を切り分けることができる.

## 詳説 checkout

厳密には,checkoutのやっていることは,ただHEADの指す先を変えるのとは少し違う.

checkoutは特定のbranch(もちろんcommitでも良い)からfileをWorking directoryに取り出してくるcommandである.

---

```{sh, eval=F}
git checkout <commit|branch> <path>
```

と書くことで,特定の`<branch>`(or `<commit>`)の`<path>`にあるfileを現在のWorking directoryに持ってくることができる.

`<path>`を省略すると,その`<branch>`の全ファイルをWorking directoryに持ってくることになる.

全部持ってきたらWorking directoryの中身は,その`<branch>`と全く同じになる.

👉 HEADを移動したのと同じ.

---

このため,未commitのfileがあるとcheckoutは失敗する

(まだcommitされていないfileがcheckout先のfileで上書きされてしまう!).


## git merge 

他のbranchと今いるbranchを統合するcommitを作る

`git merge [<branch>]`

## git merge 

Githubを使うのであればこの作業はPRをMergeしたときに勝手にやってくれる.

GithubでPRをmergeするときはこんな感じのことをしている.

<div align="center">
<img src="./fig/merge.png" width="550px">
</div>

--- 

PRのMergeで問題が起きたときに対処できるようになるために,GithubがPRをMergeするときの動作を自分でやってみる.

---

### やってみる

```{sh, eval=F}
git checkout feature/update_file000
git merge master 
```

```{sh, eval=F}
$ git log --oneline --graph --all
*   0b45d32 (HEAD -> feature/update_file000) Merge branch 'master' into feature/update_file000
|\
| * 179a3bc (master) Added file001.
* | cf7cb13 Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

Merge commitができている.

--- 

```{sh, eval=F}
git checkout master 
git merge --no-ff feature/update_file000
```

```{sh, eval=F}
$ git log --oneline --graph --all
*   500adbb (HEAD -> master) Merge branch 'feature/update_file000'
|\
| *   0b45d32 (feature/update_file000) Merge branch 'master' into feature/update_file000
| |\
| |/
|/|
* | 179a3bc Added file001.
| * cf7cb13 Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

Merge commitがまたできている.

## git mergeをなかったことにする 

```{sh, eval=F}
$ git log --oneline --graph --all
*   500adbb (HEAD -> master) Merge branch 'feature/update_file000'
|\
| *   0b45d32 (feature/update_file000) Merge branch 'master' into feature/update_file000
| |\
| |/
|/|
* | 179a3bc Added file001. # <-- Merge前のmaterの位置
| * cf7cb13 Updated file000 (added the second row). # <-- Merge前のfeature/update_file000の位置
|/
* e170b24 Added file000.
```

master tagとupdate_file000 tagをそれぞれMerge前の位置に戻せば良い.

## (Interruption) HEADについて 

(復習) HEAD^ = 1つ前の親Commit

Merge commitは親が2ついる. どうやって指定すれば?


---

親が2つ以上ある場合 `HEAD^N (N=親の番号)` のように指定できる.

```{sh, eval=F}
$ git show --name-only HEAD^
commit 179a3bce92c28cb9a6ac1ef4d8ff4002c83d2f7d
Author: ameono <4m3on0@gmail.com>
Date:   Sun Feb 17 00:57:33 2019 +0900

    Added file001.

$ git show --name-only HEAD^2
commit 0b45d32e5332152a5a44400d6394344199f4f968 (feature/update_file000)
Merge: cf7cb13 179a3bc
Author: ameono <4m3on0@gmail.com>
Date:   Sun Feb 17 14:51:11 2019 +0900

    Merge branch 'master' into feature/update_file000
```

--- 

HEAD~2と似てるけど,全然意味が違う.

```{sh, eval=F}
$ git show --name-only HEAD~2
commit e170b24890ca2685d9414b15f3862a28b16b3828
Author: ameono <4m3on0@gmail.com>
Date:   Sat Feb 16 21:28:33 2019 +0900

    Added file000.
```

---

### HEADの表記法まとめ

<img src="https://backlog.com/ja/git-tutorial/img/post/stepup/capture_stepup1_3_2.png" width="350px">

- HEAD~N = HEADのN個前の親Commit

- HEAD^N = HEADの1個前の親CommitのN番目

ex) 1個前の親の2番めの親Commit = HEAD~1^2

- HEAD@{N} = N個前にHEADが指していたCommit

## git mergeをなかったことにする (続き) 

masterのMergeをなかったことに

```{sh, eval=F}
git reset --hard HEAD^ 
```

```{sh, eval=F}
$ git log --oneline --graph --all
*   0b45d32 (feature/update_file000) Merge branch 'master' into feature/update_file000
|\
| * 179a3bc (HEAD -> master) Added file001.
* | cf7cb13 Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

--- 

update_file000のMergeをなかったことに

```{sh, eval=F}
git checkout feature/update_file000
git reset --hard HEAD^ 
```

```{sh, eval=F}
$ git log --oneline --graph --all
* 179a3bc (master) Added file001.
| * cf7cb13 (HEAD -> feature/update_file000) Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

簡単ですね?

今までの話が理解できていれば,mergeをなかったことにしたのをなかったことにもできるはず.

## git merge --no-ff について 

```{sh, eval=F}
(復習 GithubのPR Merge)
git checkout feature/update_file000
git merge master 
git checkout master
git merge --no-ff feature/update_file000 <-- --no-ffに注目
```

このoptionはなんぞや

---

## なしにしてやってみる

```{sh, eval=F}
git checkout feature/update_file000
git merge master 
git checkout master
git merge feature/update_file000
```

```{sh, eval=F}
$ git log --oneline --graph --all
*   e6de5fd (HEAD -> master, feature/update_file000) Merge branch 'master' into feature/update_file000
|\
| * 179a3bc Added file001.
* | cf7cb13 Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

2回MergeしたのにMerge commitが1つしかできていない.は?

--- 

💡  偉い人に聞きましょう(99ページくらいまで読み進めてください)

<iframe src="https://www.slideshare.net/slideshow/embed_code/key/us6QLyLgaRMCyy?startSlide=40" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/kotas/git-15276118" title="こわくない Git" target="_blank">こわくない Git</a> </strong> from <strong><a href="https://www.slideshare.net/kotas" target="_blank">Kota Saito</a></strong> </div>

## git rebase 

mergeと同じようなことをするcommandにrebaseがある.違いを見ましょう.

他のbranchの先端に今のbranchの根本から先端までのcommitをまとめて移植する

`git rebase [<branch>]`

`git rebase --continue | --skip | --abort | --quit | `


---

## やってみる

```{sh, eval=F}
# 準備
git branch -f feature/update_file000 HEAD^2 # Mergeをなかったことに
git reset --hard HEAD^
git checkout feature/update_file000
```

---

```{sh, eval=F}
git rebase master
```

```{sh, eval=F}
$ git log --oneline --graph --all
* a336ea3 (HEAD -> feature/update_file000) Added file001.
* cf7cb13 (master) Updated file000 (added the second row).
* e170b24 Added file000.
```

--- 

```{sh, eval=F}
$ git checkout master
$ git rebase feature/update_file000
First, rewinding head to replay your work on top of it...
Fast-forwarded master to feature/update_file000.
```

```{sh, eval=F}
$ git log --oneline --graph --all
* a336ea3 (HEAD -> master, feature/update_file000) Added file001.
* cf7cb13 Updated file000 (added the second row).
* e170b24 Added file000.
```

masterとfeatureは統合されたが,Merge commitが一個もできていない

---

**merge**: Merge Commitを作って2つのbranchを統合する

**rebase**: 2つのbranchのどちらかをもう片方の先端に移植することで統合する

## git rebaseをなかったことにする 

まずはmasterを戻す.

```{sh, eval=F}
$ git reset --hard HEAD^ 
* a336ea3 (feature/update_file000) Added file001.
* cf7cb13 (HEAD -> master) Updated file000 (added the second row).
* e170b24 Added file000.
```

--- 

次にupdate_file000を戻す


```{sh, eval=F}
$ git reflog
a336ea3 (HEAD -> feature/update_file000) HEAD@{2}: checkout: moving from master to feature/update_file000
cf7cb13 (master) HEAD@{3}: reset: moving to HEAD^
a336ea3 (HEAD -> feature/update_file000) HEAD@{4}: rebase finished: returning to refs/heads/master
a336ea3 (HEAD -> feature/update_file000) HEAD@{5}: rebase: checkout feature/update_file000
cf7cb13 (master) HEAD@{6}: checkout: moving from feature/update_file000 to master
a336ea3 (HEAD -> feature/update_file000) HEAD@{7}: rebase finished: returning to refs/heads/feature/update_file000
a336ea3 (HEAD -> feature/update_file000) HEAD@{8}: rebase: Added file001.
cf7cb13 (master) HEAD@{9}: rebase: checkout master
179a3bc HEAD@{10}: checkout: moving from master to feature/update_file000 # <-- これ
```

```{sh, eval=F}
$ git reset --hard HEAD@{10} 
$ git log --oneline --graph --all
* 179a3bc (HEAD -> feature/update_file000) Added file001.
| * cf7cb13 (master) Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

Mergeより戻すのが面倒(reflog使わないかん)ですね.

このことから,rebaseとmergeならmergeを使うべきという人もいます.

---

公式曰く,

> 公開リポジトリにプッシュしたコミットをリベースしてはいけない

> この指針に従っている限り、すべてはうまく進みます。もしこれを守らなければ、あなたは嫌われ者となり、友人や家族からも軽蔑されることになるでしょう。(Pro Git)

## 詳説 rebase

💡  偉い人に聞きましょう(181ページくらいまで読み進めてください)

<iframe src="https://www.slideshare.net/slideshow/embed_code/key/us6QLyLgaRMCyy?startSlide=100" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/kotas/git-15276118" title="こわくない Git" target="_blank">こわくない Git</a> </strong> from <strong><a href="https://www.slideshare.net/kotas" target="_blank">Kota Saito</a></strong> </div>

## 歴史改変のrebaseとbranchのrebase

歴史改変のrebase(左)とbranchのrebase(右)
やっていることは同じ.

<div class="column-left">
<div align="center">
<img src="./fig/rebase_single.svg" width="300px">
</div>
</div>
<div class="column-right">
<div align="center">
<img src="./fig/rebase_multi.svg" width="500px">
</div>
</div>

## git cherry-pick 

他のbranchの特定のcommitを今のbranchの先端に移植する

`git cherry-pick <commit>...`

---

### やってみる

```{sh, eval=F}
$ git log --oneline --graph --all
* 179a3bc (HEAD -> feature/update_file000) Added file001.
| * cf7cb13 (master) Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

---

```{sh, eval=F}
$ git cherry-pick cf7cb13
```

---

```{sh, eval=F}
$ git log --oneline --graph --all
* 8a1eae4 (HEAD -> feature/update_file000) Updated file000 (added the second row).
* 179a3bc Added file001.
| * cf7cb13 (master) Updated file000 (added the second row).
|/
* e170b24 Added file000.
```

 `master`のcommitが`feature/update_file000`へcopyされた

 rebaseはcherry-pickを複数まとめてやっているだけとも言える.

## Conflict 

統合する複数のbranchで同じfileが変更されていると,CONFLICTが起こる

これは,Gitがどちらの歴史(Branch)が正しいか判断できないため.

userは自分で正しい歴史を選択する必要がある.

---


## Conflictを起こしてみる

```{sh, eval=F}
# 準備
git reset --hard HEAD^
echo "ROW002 <- update_file000" >> file000 
```

masterとfeatureの両方でfile000が編集されている状態を作り出す.

---

```{sh, eval=F}
$ git merge master
Auto-merging file000
CONFLICT (content): Merge conflict in file000
Automatic merge failed; fix conflicts and then commit the result.
```

🎉 CONFLCIT!!!

---

Conflictしたfileの中身

```{txt, eval=F}
ROW000
<<<<<<< HEAD
ROW002 <- update_file000
=======
ROW001
>>>>>>> master
```

## Conflictの解消法 

1. ConfilitしたファイルをEditerで編集する
1. `git checkout [--theirs|--ours] <paths>` を使う
1. merge-toolを使う 

## 1. ConfilitしたファイルをEditerで編集する

そのまんま. Vimとかでひらいて↓こんな感じに編集して保存,AddしてCommit

```{txt, eval=F}
ROW000
ROW002 <- update_file000 
```

## 2. checkoutを使う方法 

```{sh, eval=F}
git checkout --theirs file000
```

```{txt, eval=F}
ROW000
<<<<<<< HEAD # = --ours 
ROW002 <- update_file000 
=======
ROW001
>>>>>>> master # = --theris 
```

merge元のbranch(=--theirs)の変更かmerge先(=HEAD, --ours)の変更かのどちらかを採用する.

**利点**: pdfや画像などテキストとして編集できないものがConflictしたときに便利

PDFをVimで開いて「この行はmerge先を採用して,こっちはbranchを採用...」とかやってるやつがいたら病気...

**欠点**: どういう結果になるか予想がつきにくい. 


## 3. merge-toolを使う 

merge-toolには[色々ある](https://japan.blogs.atlassian.com/2016/02/tips-tools-to-solve-git-conflicts/).

```{}
git mergetool --tool=vimdiff
```

<img src="./fig/merge_tool_vimdiff.png" width="650px">

DiffがあるChunk(=色が変わっている部分)にカーソルを合わせ,
1do, 2do, 3doと入力することで,それぞれ上段の左から1番目,2番目,3番目が今後の姿に取り込まれる.

--- 

PyCharmのようにConflictを解消するツールが付いているIDEもある.

<img src="https://www.jetbrains.com/help/img/idea/2018.3/resolveConflict.png" width="850px">


---

Conflictを解消したら当該のfileをaddしてcommit.

```{sh, eval=F}
git add file000
git commit  
```

❗Conflict解消のときは自動的にmessageが入力されるのでmオプションはいらない

成功すればMerge(or rebase, cherry-pickなど)が完了する.
