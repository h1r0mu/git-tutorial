
## Branching model (branch戦略)とは?

branchをどのように切るかを決めるstyleの一つ.

別に守らなくてもErrorは起きないが,守ると便利なルール(Coding styleとかと同じ立ち位置).

## Git Flow

<div class="column-right">
<div align="center">
<img src="https://camo.qiitausercontent.com/bbb6ba70f058f0d10c8f7a769ae8fd089dc7684e/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f30363134303132312d613062362d343237662d633134392d3638353863313439373338652e706e67" width="450px">
</div>
</div>

<div class="column-left">
- **feature**: 
  - 作業用branch
  - Issueをassignされた人が好きにして良い
  - Issueと対応しているとbetter
- **master**:
  - relase専用のbranch
  - ここに直接CommitしたりfeatureをMergeすることはない
- **develop**:
  - feature branchをmergeする専用のbranch
  - ここに直接commitすることはない 
- **(hotfixes)**:
  - masterに緊急のbugが見つかったときに修正のcommitをするbranch
- **(release)**:
  - masterにmergeする直前の作業をするbranch
</div>


## Github Flow

<div align="center">
<img src="https://cdn-images-1.medium.com/max/2600/1*iHPPa72N11sBI_JSDEGxEA.png" width="850px">
</div>

featureとmasterしかないシンプルなFlow.

面倒なことはPR上でやる.

## Gitlab flow

<div class="column-right">
<div align="center">
<img src="https://camo.qiitausercontent.com/0845268343ff051296a5677ff0ac0a22afac168b/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3138353338392f36386235613237622d653332622d643631662d363032302d3936646533626131643333352e706e67" width="450px">
</div>
</div>

<div class="column-left">
- **feature/hotfix**: 機能開発、不具合対応branch
- **master**: mainのbranch
- **pre-production(option)**: release前のtest用(git-flowで言うrelease branch)
- **production**: release済みのcode置き場
</div>

