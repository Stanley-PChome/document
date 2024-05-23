## Git
```

建立新的本地端 Repository
git init

複製遠端的 Repository 檔案到本地端
git clone [Repository URL]

檢查本地端檔案異動狀態
git status

查看先前的 commit 記錄
git log

顯示所有標籤
git tag

將所有檔案或資料夾加入版本控制
git add

將指定的檔案或資料夾加入版本控制
git add [檔案或資料夾]

提交目前的異動
git commit

提交目前的異動並設定摘要說明
git commit -m "提交說明內容"

獲取遠端 Repository 所有 branch 更新
git fetch

獲取指定遠端 Repository 所有 branch 更新
git fetch <remote-name>

獲取指定遠端 Repository 特定 branch 更新
git fetch <remote-name> <branch-name>

顯示分支
git branch

建立分支
git branch <branch-name>

刪除本地分支
git branch -d <branch-name>

强制删除本地分支
git branch -D <branch-name>

修改分支名稱
git branch -m <old-branch-name> <new-branch-name>

顯示所有未合併的分支
git branch --no-merged

切換至該分支
git checkout <branch-name>

建立並切換至該分支
git checkout -b <branch-name>

切換至該 commit
git checkout <hash>

遠端Repository拉取並合併至當前分支(git fetch和git merge的合併)
git pull

指定遠端Repository拉取並合併至當前分支
git pull <remote-name> <branch>

發佈至遠端分支
git push

發佈至指定的遠端分支
git push <remote-name> <branch-name>

強推至指定的遠端分支
git push --force <remote-name> <branch-name>

強推至指定的遠端分支前，檢查遠端分支是否已被修改
git push --force-with-lease <remote-name> <branch-name>

發佈所有至指定的遠端
git push --all <remote-name>

刪除遠端分支
git push <remote-name> --delete <branch-name>

發佈本地指定分支到遠端分支
git push <remote-name> <local-branch-name>:<remote-branch-name>

合併指定分支至當前分支
git merge <branch-name>

當前分支轉移指定分支上
git rebase <branch-name>

rebase出現衝突，需要手動解决衝突後繼續rebase
git rebase --continue

中止 rebase
git rebase --abort

交互式 rebase (重新排序提交、合并提交、修改提交消息)
git rebase -i <commit>

強制恢復到指定的 commit（透過 Hash 值）
git reset --hard [HASH]

```
