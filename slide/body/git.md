## そもそもGitってなんだよ

プログラムのソースコードなどの変更履歴を記録・追跡するための分散型**バージョン管理システム**だよ.

## 作った人 

<div class="column-left">
<div align="center">
<img src="https://media.giphy.com/media/xndHaRIcvge5y/giphy.gif" width="450px">
</div>
</div>

<div class="column-right">
  
**リーナス・トーバルズ**

- Linux作った人(GitはLinux開発のためのツールとして作ったらしい)
- Linuxは修論で作ったらしい
- Gitは2週間くらいで作ったらしい 
- 開発当時,有名なOSの教授とお互いのOSを貶し合ってめちゃくちゃ喧嘩したのが伝説になっている
- 優しい終身の独裁者(OSSコミュニティのリーダをこう呼ぶ)の一人
- 優しい...
</div>

## バージョン管理システムでできること 

- file変更履歴(version)を記録する
- 特定のversionを復元する
- 複数人が同じファイルを同時に編集する
- version間の差分を見る 

などなど...

## バージョン管理システムが使える場面 

- source codeの管理 (メイン)
- Shellなどの設定ファイルの管理(いわゆるDotfiles)
- 小説や論文などの原稿の管理

などなど...

version(版)という概念のあるものすべてに応用可能


## なんだか複雑そうだな...Google driveでよくない? 

よくない.

**Google driveでできないこと**

- 複数人で同じdirectoryの中身を編集できない(=分岐に対応していない)
- RemoteとSyncするタイミングを制御できない(変更した瞬間*自動的*にSync)
- revisionに含めるfileを制御できない(*自動的*にrevisionとして保存する対象に追加される)

---

- これらができないがためにこうなりがち

<div align="center">
<img src="./fig/backup_hell.png" width="650px">
</div>

---

恣意的な表

  | 機能 | Google Drive | Git + Github |
  | ---- | ---- | ---- |
  | 変更履歴(revision)の記録 | ○ | ○ |
  | 記録するタイミングの制御 | × | ○ |
  | 記録するファイルの選択 | × | ○ |
  | 復元 | ○ | ○ |
  | 分岐 | × | ○ |
  | 差分の表示 | × | ○ |
  | 履歴の共有 | ○ | ○ |



## 今日の目標

- 何かやらかしても元に戻せるようになる 

Gitは分散型Version管理ツール

👉 local元に戻せればremoteのも元に戻せる!

## やること 

1. gitのコンセプトを理解する (頭で)
1. 呪文を覚える (身体で)
1. 呪文を打ち消す呪文を覚える (身体で)

2と3をひたすら繰り返す.


<div style="font-size:xx-small;">

>	重犯罪人に自分の罪を思い知らせるなら、土を掘り移動させ、それからまた埋めて元に戻す、という作業を延々と繰り返させればよい。それは究極の拷問であり、最後には精神に異常をきたすであろう。(ドフトエフスキー?)

</div>

## 今日扱うcommandたち

<div class="container">
<div class="left">

##### 1つのbranchの操作
revesion管理基礎

- git status
- git init
- git add 
- git commit 
- git reset 
- git rm
- git mv
- git commit --amend
- git rebase -i


</div>
<div class="contents">

##### 複数のbranchの操作
分岐する場合を扱う

- git log
- git branch
- git checkout
- git merge
- git rebase

</div>
<div class="right">

##### remote branchの操作
Cloudを利用

- git clone
- git fetch
- git pull
- git push

</div>
</div>


## Gitのコンセプトを理解する 

Gitには3つの視点がある

- Commit tree 
- Three areas 
- File status 

これらを理解すればGit master.

## Commit tree 

保存されているCommit (=Version)の親子関係を表すtree (一番重要).

- CommitはdirectoryのスナップショットにIDとしてhash値を振ったもの.
- Commitには親となるCommitが記録されている (👉 tree構造)
- 1度作ったcommitは基本的になくならない(探せなくはなるので注意) 
- Hash値だけで管理すると人間が使うとき不便なのでHEADとかmasterみたいな看板(refs)を使う
- master, HEAD, branchなどは特定のCommitを指しているrefsの名前(**重要**)

<div align="center">
<img src="https://git-scm.com/book/en/v2/images/advance-master.png" width="450px">
</div>


--- 

### refsのイメージ

<div align="center">
<img src="https://pbs.twimg.com/media/CpvqRmiUAAAPlLm.jpg" width="650px">
</div>

- 最後尾のCommit(人)は新しい人が並ぶたびに変わるが,最後尾は常に看板の指す先にある.

---

### どういうときに思い出すべき?

- 特定のversionを復元するとき (git reset, git checkout)
- versionの分岐を扱うとき (git branch, git merge, git rebase)
- remote branchと同期するとき (git push, git pull)

## Three areas 

1つのCommitの裏には3つのareaがある

1. Working directory: 実際のdirectory (lsコマンドとかで見える風景)
1. Staging Area (Index): Working direcoryのファイルのうち次のCommitに含まれる予定のファイルを一時保管する場所 (実際には,次のコミットに何が含まれるかに関しての情報を蓄えたファイル)
1. git directory: Staging areaの情報を元に作られたCommitが保存される場所 (commit treeで見えているのはここの風景) 

<div align="center">
<img src="https://git-scm.com/book/en/v2/images/areas.png" width="450px">
</div>

Staging areaとgit directoryはユーザから直接見えない!

--- 

### どういうときに思い出すべき?

- Commitを作るとき (git add, git commit)
- version管理するファイルを選択するとき (git rm --cached, git add, .gitignore)
- 一部のファイルのversionだけを戻すとき (git reset <file>, git checkout <file>)

## File status 

fileには4つの状態がある(three areasをfile側から見たもの)

1. Untracked: gitがversion管理**しない**file
1. Unmodified: gitの管理対象かつ最新のversionから変更されていないfile
1. Modified: gitの管理対象かつ最新のversionから変更されたfile(≒次Commitすべきもの)
1. Staged: gitの管理対象かつ変更されたfileかつStaging areaに追加済みのもの(=次Commitされる!)

<div align="center">
<img src="https://git-scm.com/book/en/v2/images/lifecycle.png" width="450px">
</div>


--- 

### どういうときに思い出すべき?

- gitの管理対象のディレクトリ内で,fileを生成,編集,削除したとき
- fileを管理対象に入れたり外したりするとき (git add, git rm, git mv)

## おめでとう 

あなたは,gitマスターになりました.

