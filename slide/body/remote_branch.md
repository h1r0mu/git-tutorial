
# Remote branch (clone, fetch, pull, push)

## まえおき

最後に,remote repositoryと連携する方法を体験しましょう.

remoteとの連携はlocalのcommit treeをremoteに同期することと同義です.

最も単純なケースでは,localで更新されたtreeをそのままremoteにcopyして,remoteで更新されたtreeをlocalにcopyすることの繰り返しに近い動作になります.

---

Gitは分散型なので,remoteと連携する場合でもほとんどの作業はlocalで完結するようにできています.

localだけの作業と変わる部分は,`<remote>/<branch>`という特殊なrefsが追加されることだけです.

このrefsは`<remote>` repositoryにおいて`<branch>`が指しているcommitを指します.


---

remoteでこのbranchに新しいcommitが追加されれば,当然この`<remote>/<branch>`の指す先もその新しいcommitに更新されます.

この新しいcommitを取り込みたければ,`<remote>/<branch>`をlocalの同じ名前の`<branch>`にmergeするだけですみます.

つまり,remoteとはただのbranchの一つ(ただし,自分以外の誰かの手によって勝手に更新される)だと考えることができます.

この基本原則を忘れなければ,localの操作の延長線上でremoteの操作も可能です.

---

なお,特に設定なければ`<remote>`にはoriginという名前が使われます.

これは,master branchが勝手にmasterという名前になっているのと同じで,ただのデフォルト値であり,特別な意味はありません.

## この章で学べること 

- localの変更をremoteに同期する方法 
- その逆 

## 学んだことからできること 

- remote repository経由で複数人と共同作業する 
- 複数のPCで同じrepositoryをUbiquitous(死語)に操作する 


## git clone

remote repositoryのcopyをlocalに作る.(= Download)

`git clone <repository>`

---

### なにが起きるか

<div align="center">
<img src="./fig/clone.svg" width="750px">
</div>

最初の一回しか使わない(あとはpullやfetch).

## git cloneをなかったことにする

```{sh, eval=F}
rm -R path/to/repository
```

## git fetch

現在のremoteの`<branch>` refsの位置に合わせてlocalの`origin/<branch>` refsの位置を更新する.

`git fetch <remote> <branch>`

<div align="center">
<img src="./fig/fetch.svg" width="750px">
</div>

## git fetch

このままではremoteのmaster(origin/master)がlocalのmasterに反映されていないので,masterをorigin/masterにmergeする.

```{sh, eval=F}
git merge origin/master
```

<div align="center">
<img src="./fig/merge_after_fetch.svg" width="750px">
</div>

## git fetchをなかったことにする 

remoteに合わせてlocalのrefsとcommitを更新するだけなので,`git commit`や`git branch`をしたのと起こることは同じ.

よって, git commit OR git branchをなかったことにするときと同じ方法でなかったことにできる.

## git pull

remote branchの変更をlocalのbranchに取り込む (Downloadに近い)

`git pull [--rebase|--no-ff] <remote> <branch>`

`git fetch` と `git merge origin/master` をまとめてやる(`--rebase`つけると`git rebase origin/master`になる)

<div align="center">
<img src="./fig/pull.svg" width="700px">
</div>

Mergeの作業が入っているため,pullではConflictが発生する可能性がある.

ちなみに,GithubのPRという名前はGithubのPRのMergeにpull(fetch+merge)コマンドが含まれることに由来する

## git pullをなかったことにする

git mergeをなかったことにする方法 + git fetchをなかったことにする方法.

## git push

local branchの変更をremote branchに反映させる (Uploadに近い)

`git push <remote> <branch>`

git pullのremoteとlocalを逆にしたもの(pullと違いConflictする場合は実行できない= --ff-only).

<div align="center">
<img src="./fig/push.svg" width="750px">
</div>


## git pushできないとき 

何れかの原因でConflictする場合,pushはできないので適切な方法で対処する

### 自分に原因がある場合
- **原因となる作業**: rebase, commit --amend, resetなどremoteとconflictする作業をした
- **対処法**: `git push -f <remote> <branch>`, `git push --force-with-lease <remote> <branch>` で強制pushする

`git push -f` は強制的にremoteを上書きするので,ミスると**remoteからすべての歴史を吹き飛ばす**ので(頭真っ白になる),
吹き飛ばしても人に迷惑のかからないfeature branchのみで使うこと.

`--force-with-lease` オプションはローカルのrefsがremoteと一致している場合のみ(i.e., localが最新の場合)強制的にremoteを上書きするのでやや安全

---

### 他人に原因がある場合
- **原因となる作業**: 同じbranchに他の人がpushした, remote repository上でCommitした
- **対処法**: `git pull <remote> <branch>` してConflictを解消してからpushする  (pullはconflictが起こっても実行できるため)

## git pushをなかったことにする

pushをなかったことにするには,さらにpushするしかない.

`git push -f` をして,push前の状態を強制上書きする.

```{sh, eval=F}
git checkout <push前に<remote>/<branch>が指していたcommit> 
git push -f <remote> HEAD:<branch>  # localのHEADの位置にremoteの<branch>の位置を移動する
```

もちろん,`reset --hard`で,localのcommitをなかったことにしてから`push -f` してもよい.

---

あれ,さっきpush -f するなって言わなかったっけ?

そうです.なかったことにしなくて良いように,よく考えてからpushしましょう.

## git fetch VS git pull

- 他のbranchにcheckoutせずにremoteのCommitを取り込みたい場合fetch
- それ以外の場合pull

最新のmasterにfeatureをrebaseしたい場合の例

```{sh, eval=F}
# pull version (HEAD -> feature)
git checkout master
git pull origin master
git checkout feature
git rebase master
```

```{sh, eval=F}
# fetch version (HEAD -> feature)
git fetch origin 
git rebase origin/master 
```

4 command -> 2 commmand

特に,HEADに未commitのfileがあるときにこれをやりたくなるとpullでは非常に面倒(stashを使えばいいのだが更に2 command増える)

