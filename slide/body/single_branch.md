
# Single branch 1 (add, commit, reset)

## まえおき

ここから先の作業はSource treeのHistory(履歴)画面でCommit treeを表示しながらやることをオススメします.

まずは,versionが分岐しない場合,ただの便利なbackup toolとしてgitを扱う方法を体験しましょう.

## この章で学べること 

- Working directoryにおけるfileの追加や変更を記録したCommitを作る方法
- Commitに含めるファイルを選ぶ方法
- これらをなかったことにする方法 

## 学んだことからできること 

- Directoryの変更履歴を管理する(≒gitでdirectoryのbackupを取る)

## git status 

異世界に行ったら最初に唱える呪文.

File statusが4つのどの状態にあるかの確認.
 

```{sh, eval=F}
git status 
```

これから何かする度になるべく`git status`しましょう.


---

### 思い出す

<div align="center">
<img src="https://git-scm.com/book/en/v2/images/lifecycle.png" width="650px">
</div>


## git init 

current directoryにgit repositoryを作る

---

### やってみる 

```{sh, eval=F}
# 準備 
cd path/to/workspace
mkdir repo000
cd repo000
```

---

### やってみる 

```{sh, eval=F}
git init 
git status # どうなったか見てみる
```

`ls -a` すると`.git` という隠しdirectoryが作られているはず.

これがgit repositoryであり,すべての変更履歴の情報はここに記録されていく(もっとも,直接見てわかるような形にはなっていない).

## git initをなかったことにする

current directoryからgit repositoryを削除する

❗削除したrepositoryは元に戻せないので注意

```{sh, eval=F}
rm -R .git
```

## git add 

fileをStaging areaに追加する.

- 新規ファイルの場合
    - tracked(gitの追跡対象指定)にする
    - staged(commitに含める対象指定)にする.
- 既存ファイルの場合
    - staged(commitに含める対象指定)にする.

`git add [path ...]`

--- 

### やってみる

```{sh, eval=F}
echo "ROW000" > file000 # 準備
git add file000
```

## git addをなかったことにする 

- 新規ファイルの場合
    - untracked(gitの追跡対象外)にする
    - unstaged(commitに含める対象外)にする.
- 既存ファイルの場合
    - unstaged(commitに含める対象外)にする.

`git reset [--soft | --hard] [<commit>] [path ...]`

---

### やってみる 

```{sh, eval=F}
git reset HEAD file000
```

stage(git add)されたすべてのファイルを対象にしたい場合

```{sh, eval=F}
git reset HEAD
```

---

### HEADについて

HEADは現在参照しているCommitを指すrefs(看板)

- HEADが指しているCommitとことなるfileはmodified状態になる
- HEADの指すCommitを変える=Working direcotryのfileをHEADの指すCommitに記録されているものに置き換える
- HEAD^ = HEADの1つ前のCommit

## git commit 

stage (git add)されたすべてのfileの現在の状態をGit Directoryへ記録する

---

### やってみる

```{sh, eval=F}
# 準備
git add file000
```

`git commit [--amend] [-m <msg>]` 

```{sh, eval=F}
git commit -m "Added file000."
```

## git commitをなかったことにする

Git Directoryから最後のCommitを削除する

--- 

### やってみる

```{sh, eval=F}
# 準備
cp file000 file001
git add file001
git commit -m "Added file001."
```

---

### やってみる

```{sh, eval=F}
git reset --soft HEAD^ # HEAD^ = 1つ前の親Commit
```

addもなかったことに

```{sh, eval=F}
# 復習
git reset HEAD 
```

commitとaddの2つをまとめてなかったことに

```{sh, eval=F}
git reset HEAD^ 
```

## git commitをなかったことにしたのをなかったことに

復習: *1度作ったcommitは基本的になくならない(探せなくはなるので注意)*

さっきなかったことにしたcommitは実は消えていない(みえなくなっただけ).

見えなくなったcommitを探すcommand = reflogを使う

---

### やってみる

```{sh, eval=F}
$ git reflog
e170b24 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
953913d HEAD@{1}: commit: Added file001. <-- resetするまえのHEADの位置
...
```

```{sh, eval=F}
git reset HEAD@{1} # HEAD@{1} = 1つ前にHEADの指していたcommit
```

## git reset 

既に何度か使っている`git reset` これの役割は?

---

💡  偉い人に聞きましょう(54ページくらいまで読み進めてください)

<iframe src="https://www.slideshare.net/slideshow/embed_code/key/3BCgttHAboVzqg?startSlide=30" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/TomohikoHimura/git-22237343" title="やりなおせる Git 入門" target="_blank">やりなおせる Git 入門</a> </strong> from <strong><a href="https://www.slideshare.net/TomohikoHimura" target="_blank">Tomohiko Himura</a></strong> </div>

---

<table class="tableblock frame-all grid-all spread">
<colgroup>
<col style="width: 42.8571%;">
<col style="width: 14.2857%;">
<col style="width: 14.2857%;">
<col style="width: 14.2857%;">
<col style="width: 14.2858%;">
</colgroup>
<thead>
<tr>
<th class="tableblock halign-left valign-top"></th>
<th class="tableblock halign-left valign-top">HEAD</th>
<th class="tableblock halign-left valign-top">インデックス</th>
<th class="tableblock halign-left valign-top">作業ディレクトリ</th>
<th class="tableblock halign-left valign-top">作業ディレクトリ保護の有無</th>
</tr>
</thead>
<tbody>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><strong>Commit Level</strong></p></td>
<td class="tableblock halign-left valign-top"></td>
<td class="tableblock halign-left valign-top"></td>
<td class="tableblock halign-left valign-top"></td>
<td class="tableblock halign-left valign-top"></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>reset --soft [commit]</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">REF</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">いいえ</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">いいえ</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>reset [commit]</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">REF</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">いいえ</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>reset --hard [commit]</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">REF</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock"><strong>いいえ</strong></p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>checkout [commit]</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">HEAD</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><strong>File Level</strong></p></td>
<td class="tableblock halign-left valign-top"></td>
<td class="tableblock halign-left valign-top"></td>
<td class="tableblock halign-left valign-top"></td>
<td class="tableblock halign-left valign-top"></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>reset (commit) [file]</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">いいえ</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">いいえ</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>checkout (commit) [file]</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">いいえ</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">はい</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock"><strong>いいえ</strong></p></td>
</tr>
</tbody>
</table>

つまり?

`git reset --hard, git checkout <file>` (上表で太字の**いいえ**になっているやつ) するときは気をつけよう.


# Single branch 2 (rm, mv)

## この章で学べること 

- fileの削除や移動をgit repositoryへ記録する方法 
- 特定のfileをcommitに含めなくする方法 

## git rm --cached

fileをStaged areaとGit directoryから削除する

(=untracked(gitの追跡対象外)にする)


`git rm [--cached] [path ...]`

---

### やってみる

```{sh, eval=F}
# 準備: 新しいgit repositoryを作る
cd path/to/workspace 
mkdir repo002 
cd repo002
git init
echo "ROW000" > file000
git add file000 
git commit -m "Added file000."
```

---

```{sh, eval=F}
git rm --cached file000
git status # file000がgit repositoryから削除された
ls # Working directoryにはfile000が残っている
```

実際のfileは削除せずにgitからfile000を削除(追跡対象外に)できた!

## .gitignore

一生untrackedにしておきたいfileやdirectoryは`,.gitignore`というfileを作ってそこに書く.

.gitignoreしたdirectory内の一部のfileのみをtrackedにしたい場合は,`.gitkeep`に書いてignoreされないようにする.

.gitignore(.gitkeep)については,自分でググってね. (i.e., あまり興味ない)


<div align="center">
<img src="http://slazebni.cs.illinois.edu/fall18/orly.jpg" width="650px">
</div>

## git rm

ファイルをWorking Directory, Staged area, Git directoryのすべてから削除する.

---

### やってみる

```{sh, eval=F}
# 準備  
git add file000 # さっき削除したファイルを再び追加する
```

--- 

```{sh, eval=F}
rm file000
git add file000 # git rm --cached file000でも良い
git commit -m "Removed file000."
```

上の2つをまとめてやることもできる

```{sh, eval=F}
git rm file000
git commit -m "Removed file000."
```

## git mv

ファイルをWorking Directory, Staged area, Git directoryのすべてにおいてmoveまたはrenameする. 

*Note: mvとrenameは本質的に同じ*

`git mv  <source> <destination>`

```{sh, eval=F}
# 準備  
git reset --hard HEAD^
```


```{sh, eval=F}
git mv file000 file001
git commit -m "Renamed file000 to file001."
```

## これらをなかったことにする方法

commitをなかったことにするのと同じ

# Single branch 3 (commit --amend, rebase)

## この章で学べること 

- 作成済みのcommitを改変する方法
- 改変をなかったことにする方法 


## 学んだことからできること 

- gitに記録されているcommit(変更の歴史)を自由に改変する (≒single branchならなにをするのも怖くなくなる) 

## git commit --amend

最後のCommitを変更する

ファイルをaddし忘れたときとか,コメントを修正したい時とかに便利

---

### やってみる

```{sh, eval=F}
# 準備
cd path/to/workspace
mkdir repo003 # 新しいgit repositoryを作る
git init
echo "ROW000" > file000
cp file000 file001
git add file000
git commit -m "Added file000."
```

---

```{sh, eval=F}
git add file001 
git commit --amend -m "Added file000 and file001."
```

## git commit --amendをなかったことにする

復習: *1度作ったcommitは基本的になくならない(探せなくはなるので注意)*

amendは,一見直前のCommitを改変したように見えるが,実は新しくCommitを作って元々のCommitを見えなくしただけ.

そのため,見えなくなったcommitを探すcommand = reflogでamend前のCommitを復元することができる.

---

## やってみる

```{sh, eval=F}
$ git reflog
0cb7577 (HEAD -> master) HEAD@{0}: commit (amend): Added file000 and file001.
8dcd8b5 HEAD@{1}: commit (initial): Added file000. # <- amend前のCommit
```

---

HEADの指す位置をamend前のCommitに変える.

```{sh, eval=F}
git reset --hard HEAD@{1}
```

## git rebase 

直前だけじゃなく,もっと前まで遡って自分の都合の良いように歴史を改変したい

`git rebase -i <commit>`

`<commit>`から`HEAD`までのコミットをまとめて変更する

---

### やってみる

```{sh, eval=F}
# 準備
for f in file001 file002 file003 gomi; do cp file000 $f; done 
# file001, file002, file003, gomiができれば何でも良い
git add file001
git commit -m 'Added file001.'
git add file002
git commit -m 'Added file002.'
git add file003
git commit -m '「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される'
git add gomi 
git commit -m 'Added gomi.'
```

--- 

`git rebase -i <commit>`

```{sh, eval=F}
* bf1b31a (HEAD -> master) Added gomi. # 
* fb7cc7b 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される # <-- HEAD~1 or HEAD^
* 469a642 Added file002. # <-- HEAD~2 or HEAD^^
* f8c8773 Added file001. # <-- HEAD~3 or HEAD^^^
* 2312317 Added file000. # <-- HEAD~4 or HEAD^^^^
```

`<commit>`には変更の根本となるcommitを指定する.

```{sh, eval=F}
git rebase -i HEAD~4 # HEAD~4 = 4つ前の親Commit
```

この場合変更対象は`HEAD` - `HEAD~3`

--- 

```{sh, eval=F}
pick 17b9da7 Added file001.
pick 98ee317 Added file002.
pick 9a7d9db 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
pick 00805cf Added gomi.

# Rebase e170b24..00805cf onto e170b24 (4 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```
1番目と2番目はまとめたくて,3番目のメッセージを書き直したくて,4番目は消したい.

--- 

```{sh, eval=F}
pick 17b9da7 Added file001.
s 98ee317 Added file002.  
e 9a7d9db 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
d 00805cf Added gomi. 

# Rebase e170b24..00805cf onto e170b24 (4 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

--- 

```{sh, eval=F}
# This is a combination of 2 commits.
# This is the 1st commit message:

Added file001.

# This is the commit message #2:

Added file002.

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Feb 16 22:50:41 2019 +0900
#
# interactive rebase in progress; onto e170b24
# Last commands done (2 commands done):
#    pick 5b6d77f Added file001.
#    squash 21c25a0 Added file002.
# Next commands to do (2 remaining commands):
#    edit 2f734eb 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
#    drop cf942f2 Added gomi.
# You are currently rebasing branch 'master' on 'e170b24'.
#
# Changes to be committed:
#       new file:   file001
#       new file:   file002
```

--- 

```{sh, eval=F}
# This is a combination of 2 commits.
# This is the 1st commit message:

Added file001 and file002.

# This is the commit message #2:

# Added file002.

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Feb 16 22:50:41 2019 +0900
#
# interactive rebase in progress; onto e170b24
# Last commands done (2 commands done):
#    pick 5b6d77f Added file001.
#    squash 21c25a0 Added file002.
# Next commands to do (2 remaining commands):
#    edit 2f734eb 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
#    drop cf942f2 Added gomi.
# You are currently rebasing branch 'master' on 'e170b24'.
#
# Changes to be committed:
#       new file:   file001
#       new file:   file002
```

--- 

```{sh, eval=F}
[detached HEAD 0136ee2] Added file001 and file002.
 Date: Sat Feb 16 22:50:41 2019 +0900
 2 files changed, 2 insertions(+)
 create mode 100644 file001
 create mode 100644 file002
Stopped at 2f734eb...  「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
You can amend the commit now, with

  git commit --amend # <-- commitを編集したいのでまずは,こっち

Once you are satisfied with your changes, run

  git rebase --continue
```

git logを見てみると`<commit>`で指定した根本から枝分かれしていることがわかる

```{sh, eval=F}
$ git log --oneline --graph --all
* 87449ac (HEAD) 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
* 2f98c92 Added file001 and file002.
| * bf1b31a (master) Added gomi.
| * fb7cc7b 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
| * 469a642 Added file002.
| * f8c8773 Added file001.
|/
* 2312317 Added file000.
```

--- 

```{sh, eval=F}
git commit --amend
```

```{sh, eval=F}
「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Feb 16 22:50:41 2019 +0900
#
# interactive rebase in progress; onto e170b24
# Last commands done (3 commands done):
#    squash 21c25a0 Added file002.
#    edit 2f734eb 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
# Next command to do (1 remaining command):
#    drop cf942f2 Added gomi.
# You are currently editing a commit while rebasing branch 'master' on 'e170b24'.
#
# Changes to be committed:
#       new file:   file003
#
```

--- 

```{sh, eval=F}
Added file003.

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Feb 16 22:50:41 2019 +0900
#
# interactive rebase in progress; onto e170b24
# Last commands done (3 commands done):
#    squash 21c25a0 Added file002.
#    edit 2f734eb 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
# Next command to do (1 remaining command):
#    drop cf942f2 Added gomi.
# You are currently editing a commit while rebasing branch 'master' on 'e170b24'.
#
# Changes to be committed:
#       new file:   file003
#
```

終わったらcontinue

```{sh, eval=F}
git rebase --continue
```


## git rebaseをなかったことにする 

rebaseの途中でやっぱりやめたくなった場合
```{sh, eval=F}
git rebase --abort
```

rebaseを発動する前の状態に戻ることができる.

rebase対象が多すぎて途中でわけわからなくなったら,素直に--abortして最初からやり直そう.

## git rebaseをなかったことにする 

rebaseしたあと,やっぱりなかったことにしたくなった 

commit --amendと一緒で,rebaseは既存のcommitをコピーを作っているだけ

なので,元のcommitも残っている(見えなくなっているだけ).

```{sh, eval=F}
* 99d7024 (master) Added file003
* 2f98c92 Added file001 and file002.
| * bf1b31a (HEAD) Added gomi. # <-- 元のHEAD
| * fb7cc7b 再帰で検索して「もしかして：再帰」をクリックすると…。
| * 469a642 Added file002.
| * f8c8773 Added file001.
|/
* 2312317 Added file000.
```

---

### やってみる

見えないcommitをたどるにはreflog.

```{sh, eval=F}
$ git reflog
a264b19 (HEAD -> master) HEAD@{0}: rebase -i (finish): returning to refs/heads/master
a264b19 (HEAD -> master) HEAD@{1}: commit (amend): Added file003.
ecc3571 HEAD@{2}: rebase -i (edit): 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
9070db1 HEAD@{3}: rebase -i (squash): Added file001 and file002.
408797f HEAD@{4}: rebase -i (start): checkout HEAD~4
92b7c29 HEAD@{5}: commit: Added gomi.  # <-- rebase前のHEADの位置
...
```

```{sh, eval=F}
git reset --hard HEAD@{5} # HEAD@{5} = 5つ前にHEADの指していたcommit
```

## git rebaseをなかったことにしたのをなかったことにする 

一度はrebaseをなかったことにしたんだけど,よくよく考えるとrebaseした後の方が良い気がしてきた...(よく考えて**から**行動しましょう!)

同じ理屈でなかったことにしたのをなかったことにできる

---

### やってみる

```{sh, eval=F}
$ git reflog
92b7c29 (HEAD -> master) HEAD@{0}: reset: moving to HEAD@{5} # <-- さっきのreset
a264b19 (HEAD -> master) HEAD@{1}: rebase -i (finish): returning to refs/heads/master
a264b19 (HEAD -> master) HEAD@{2}: commit (amend): Added file003.
ecc3571 HEAD@{3}: rebase -i (edit): 「もしかして:再帰」をクリックすると「もしかして:再帰」が表示される
9070db1 HEAD@{4}: rebase -i (squash): Added file001 and file002.
408797f HEAD@{5}: rebase -i (start): checkout HEAD~4
92b7c29 HEAD@{6}: commit: Added gomi. 
...

```

```{sh, eval=F}
git reset --hard HEAD@{1} 
```

## git rebaseまとめ

- 指定したcommitを分岐点として改変された歴史作る
- 途中でわけわかんなくなったら--abort 

なんでもかんでもなかったことにできるようになった!

もう何も怖くない.
