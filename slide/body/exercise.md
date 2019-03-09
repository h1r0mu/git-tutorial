## Git golf 

なるべく少ないgit commandsでホールを回るのが目標.

模範解答のcommand数をPARxで表している(一部,バーディやイーグルも可).

---

### Rules:
- 同じことができれば使用するコマンドは自由にして良い
- ググるの禁止（gitのhelpを見るのは推奨）
- 次のコマンドはカウントされない
  - git status
  - git log
  - git diff
  - git show
  - gitで始まらないコマンド
  - rebase中のコマンド（i.e., rebateは1回とカウントされる）

---


### 1番ホール PAR3
- remoteをcloneする
- masterでfile000を編集してcommitする

---

### 2番ホール PAR4
- branch(feature/hoge)を切ってそのbranchにcheckoutする
- file001を削除してcommitする

---

### 3番ホール PAR2
- file000をfile001にrenameして先程のcommitをamendする

---

### 4番ホール PAR3
- masterをcloneしてきたときの状態にresetする

(branch切るの忘れて作業してたので,あとから取り繕った体. masterの指す位置がclone直後と同じになっていればよい)

(masterではなくfeature branchにcheckoutした状態で終わる)


---

### 5番ホール PAR5
- 誰かによって更新されたremoteのmasterをpullしてきて,現在のbranchに変更を取り込む(rebaseする)
- remoteにpushする


## 回答(パー)


---

### Hole 1

```{sh, eval=F}
git clone https://github.com/...
# ファイルを編集
git add file000
git commit -m “Updated file000. #1”
```

---

### Hole 2

```{sh, eval=F}
git branch feature/hoge
git checkout feature/hoge
git rm file001
git commit -m “Removed file001. #1”
```

---

### Hole 3

```{sh, eval=F}
git mv file000 file001
git commit —amend -m “Renamed file000 to file001. #1”
```

---

### Hole 4

```{sh, eval=F}
git checkout master
git reset —hard HEAD^
git checkout feature/hoge
```

---

### Hole 5

```{sh, eval=F}
git checkout master
git pull origin master
git checkout feature/hoge
git rebase master
git push origin feature/hoge
```

## 回答(アンダー)

---

### Hole 1

```{sh, eval=F}
git clone https://github.com/...
# ファイルを編集
git commit -a -m “Updated file000. #1”
```

---

### Hole 2

```{sh, eval=F}
git checkout -b feature/hoge
rm file001
git commit -a -m “Removed file001. #1”
```

---

### Hole 3

```{sh, eval=F}
git mv file000 file001
git commit —amend -m “Renamed file000 to file001. #1”
```

---

### Hole 4

```{sh, eval=F}
git branch -f master HEAD^^
```

---

### Hole 5

```{sh, eval=F}
git fetch
git rebase origin/master
git push origin feature/hoge
```

---

もっといい方法あったら教えてください.
