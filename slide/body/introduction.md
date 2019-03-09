## はじめに 

<div align="center">
<img src="./fig/background.png" width="650px">
</div>

- 論文執筆作業の効率化が急務
- 論文執筆における作業の大半は添削と修正
- 添削の効率化が必要(手書きは論外=研究者の字は汚い)

## Adobe reader 

修正箇所にハイライト(OR 取り消し線)を引いてコメントを書ける.

<div align="center">
<img src="./fig/adobe.png" width="750px">
</div>


## Adobe readerの問題点

- **version管理ができない**
  - *復元できない*: どの時点の原稿かを管理するのが面倒,あとから探せない
  - *分岐に対応していない*: コメントをもらった原稿と最新の原稿が食い違ったときに面倒
  - *差分が見れない*: 以前の原稿から差分が見れない
  - *共有できない*: 添削履歴を一元管理できない

- **コメントを本文に反映するのが面倒**
  - ただコピペするだけなのにPDFのコメント開いて,コピーして,原稿の該当箇所探して,削除して,貼り付けて...
- **生のtexファイルが見えない**
  - ここマクロ使えば楽だよとかアドバイスできない

## version(revision)管理ツールを使う

version管理ができない問題は解決

### version管理ツールでできること

- *管理できる*: Revisionを管理できる(保存できる,探せる,**復元できる**)
- *差分が見れる*: Revision間の**差分が見れる**
- *Mergeできる*: 複数に**分岐したRevisionを一つにまとめられる**
  - e.g., 完成原稿.pdf -> 先生コメント.pdf, 先輩コメント.pdf, Typoの修正.pdf -> 完成原稿_修正版.pdf
- *履歴の一元管理ができる*: Revisionをクラウドと同期できる

最も有名なversion管理ツール -> Git

## Git + Github 

Gitに加えてGithubのIssue,**Pull request(PR)**のレビュー機能を用いることで,
**コメントを本文に反映するのが面倒**問題と**生のtexが見れない**問題も解決

- PRのsuggestion機能を使えば,**コメントをワンクリックで本文に反映**してくれる(=コメントの内容に集中できる)
- texに直接コメントできる(しかも見やすい)

<div align="center">
<img src="./fig/pr_reviewing.png" width="750px">
</div>

---

さらにTODOを管理するIssueを使うことで,楽にタスクを振れる(振られた側もGoalが明確に示されるため楽)

<div align="center">
<img src="./fig/issue_opened.png" width="750px">
</div>

---

  - 一連の流れ
    1. (先輩) Issueを起てる (ゴールを明示する)
    1. (後輩) Issueに対応するBranchをmasterから切る
    1. (後輩) Issueを解決するCommitを作りPush,Pull request(PR)をmasterに出す
    1. (先輩) Pull requestをレビューする
    1. (後輩) レビューで指摘された箇所を修正するCommitを作りPushする (指摘がなければ6.へ進む)
    1. (先輩) 指摘箇所が修正されていることを確認して,PRをmasterへmergeする
    1. (先輩) いいね(e.g., 👍)を送って労う.


---

もちろん,原稿執筆だけでなく,ソースコードやVimの設定ファイルとかの管理にも使える.

Git使おう.

## git pushでやらかして頭が真っ白になった話 

[git pushでやらかして頭が真っ白になった話](https://helen.hatenablog.com/entry/2015/11/03/001910)
<div align="center">
<img src="./fig/white.png" width="750px">
</div>

ちゃんと勉強してから使おう.
