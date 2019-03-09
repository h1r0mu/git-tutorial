
## Githubってなんだよ

GitHub は世界最大の Remote Git リポジトリホスティングサービスだよ.

つまり?

**localのrepository = localのPCにおいてあるdirecotry**

とすると,

**Github上のrepository = Google driveにおいてあるdirecotry**

のような関係.

---

有名どころ 

- Github

- Gitlab

- Bitbucket

Githubがやたら有名だが他にもある (Google driveとDropbox的な関係).

できることは大差ないので,お財布事情とUIの好みの問題

(あとは狸と猫のどっちが好きかとか...バケツ...).


<div align="center">
<img src="http://www-creators.com/wp-content/uploads/2017/02/SERVICES-680x220.png" width="750px">
</div>

---

### Github

- とにかく有名 (連携サービスが多いイメージ).

- お金たくさん持ってく.

- [Github Education](https://education.github.com/)とか言う学生プランに加入するとGithub含め色々な有名サービスが無料で使える.

- 本田圭佑がいる

- 他の2つと違い自前のCIとかはない(他のツールと組み合わせる前提OSSの思想的には○?)

---

### Gitlab

- どちらかというと,自組織のサーバにインストールして自分たちしかアクセスできないRemote repositoryとして使う (要するに自前のfile server的なやつ)オンプレミス版のほうが有名.


- Slackを水で薄めたようなchatアプリやCIがついてくるので,これひとつで開発に必要なすべての作業を完結させることができる.

- 大人の事情でPublic臭の強いGithubとかSlackとかを使えない人たちが使ってるっぽい(NASAとか).


---

### Bitbucket 

- 一昔前までPrivate repository(外部公開されない) 作り放題な唯一のサービスだったので,個人の利用者が多くいた(今は,Githubも作り放題).

- Source treeの開発元でもあるAtlassianの製品.

- お金ないらしい(最近値上げした).

---

### 比較

https://stackshare.io/stackups/bitbucket-vs-github-vs-gitlab

2019 2月現在,各サービスの無料プランの状況

                       Github    Bitbucket   Github (学生)   Bitbucket (学生)   GitLab
  -------------------- --------- ----------- --------------- ------------------ -----------
  private repository   無制限    無制限      無制限          無制限             無制限
  public repository    無制限    無制限      無制限          無制限             無制限
  collaborator         3名まで   5名まで     無制限          無制限             無制限
  CI build time        \-        50分/月     \-              500分/月           2000分/月
  LFS                  1GB       1GB         1GB             5GB                無制限?
  LFS Bandwidth        1GB/月    無制限      1GB/月          無制限             無制限?


## Githubの基本操作

<div align="center">
<img src="./fig/github_top.png" height="500px">
</div>

## Issue 

<div align="center">
<img src="./fig/issue_top.png" height="500px">
</div>

## Issueを起てる 

<div align="center">
<img src="./fig/issue_opened.png" height="500px">
</div>

## PRを送る

<div align="center">
<img src="./fig/pr_top.png" height="500px">
</div>

## PRを送る2 

<div align="center">
<img src="./fig/pr_creating.png" height="500px">
</div>

## PRをreviewする

<div align="center">
<img src="./fig/pr_review_top.png" height="500px">
</div>

## PRをreviewする2

<div align="center">
<img src="./fig/pr_reviewing.png" height="500px">
</div>

## PRをreviewする3

<div align="center">
<img src="./fig/pr_review_result.png" height="500px">
</div>

## PRをreviewする4

<div align="center">
<img src="./fig/pr_merged.png" height="500px">
</div>

