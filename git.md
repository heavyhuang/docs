## 合并多个commit
- Step1：$ git rebase -i <不变动的commit的SHA-1>，可用git log查看commit的版本号
- Step2：squash\
将编辑窗口中pick
```
pick 033beb4 b1
pick d426a8a b2
```
修改为squash,wq存储离开。
```
pick 033beb4 b1
squash d426a8a b2
```
- Step3：输入新的commit message，建议把原本的commit删掉。
