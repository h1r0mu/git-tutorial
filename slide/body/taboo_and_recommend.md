## Gitでやってはいけないこと 

---

### sizeのでかいファイルをcommitに含める

cloneするのにすごく時間がかかるようになります.

やっちゃったら[Gitの内側 - メインテナンスとデータリカバリ](https://git-scm.com/book/ja/v1/Git%E3%81%AE%E5%86%85%E5%81%B4-%E3%83%A1%E3%82%A4%E3%83%B3%E3%83%86%E3%83%8A%E3%83%B3%E3%82%B9%E3%81%A8%E3%83%87%E3%83%BC%E3%82%BF%E3%83%AA%E3%82%AB%E3%83%90%E3%83%AA)を参考に当該のファイルを削除する(失敗するとCommitツリーを破壊するリスクがあるのでしないにこしたことはない)

Tips: [Git LFS](https://git-lfs.github.com/) という大容量バイナリファイルを扱う方法も提供されている

---

### developやmasterでrebaseする

feature**で**developやmaster**を**rebaseするのはおk


---

### developやmasterで`git push -f`する

Githubでは,push -fできないようにbranchをprotectすることも可


やってしまっても大概なんとかなるけど,他の人にすごく迷惑がかかるので,喧嘩売りたいとき以外はやめましょう.


## Gitで積極的にやるべきこと 

---

### 即Issueを起てる,即Branchを切る 

何かしようと思ったらとりあえずIssue起てる.


---

### Commit messageにIssue番号を含める

`Added some files. #1` のようにすると1番のIssueのページから当該のCommitが見れるようになりトラッキングが楽.


---

### 労う

よいPRには,LGTM (Looks Good To Me) を送りましょう.

<div class="column-right">
<div align="center">
<img src="http://i.imgur.com/JNx7GVS.gif" width="450px">
</div>
</div>
<div class="column-left">
<div align="center">
<img src="https://66.media.tumblr.com/8ab4ccac32040179410ea99509e43d58/tumblr_inline_pk03u2iESS1qa5y5a_500.gif" width="450px">
</div>
</div>


