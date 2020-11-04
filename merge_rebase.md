https://git-scm.com/docs/git-rebase

### Interactive Rebase

Interaktif rebase ile; deponun commit historysinde(gecmisinde) degisiklik yapabilir, commitleri birlestirebilir, commitleri silebilir veya commitlerin sirasini degistirebilirsiniz.


aşağıdaki komutu verdiğimizde git e yapmis oldugumuz son 3 commit uzerinde degisiklik yapmak istedigimizi belirtmiş oluyoruz. 

```
git rebase -i HEAD~3
```
karşımıza şuna benze bir liste çıkmuş olacak

```
pick 490dd3c Catalogue app created
pick d1ff06d Base templates created
pick f7edff3 Logging confs updated

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

- pick: gelen listeyi editleyip başında pick yazısını bırakırsak belirttiğimiz sırada commitler alınmış gibi olacaktır daha doğrusu gibi deği gerçekten olacaktır.
- reword: Sadece commit mesajini degistirmek istiyorsaniz; mesajini degistirmek istediginiz commitin basindaki pick kelimesini reword ile degilstirin.
- edit:Rebase’i durdurur ve commit uzerinde degisiklik yapabilmenizi saglar. Istediginiz degisikligi yapip rebase islemine devam ederseniz (git rebase --continue) o commitin icerigi degisecektir. Ayrica eger rebase yaptiginiz commit birbiriyle veya commit mesaji ile alakasiz degisiklikler iceriyorsa ve siz bu commit ile alakasiz olan degisikligi farkli bir commit olarak kaydetmek istiyorsaniz rebase’den cikmadan commitleri yapip rebase islemini sonlandirdiginizda degisikliklerin farkli commitlere ayrildigini goreceksiniz.
- squash: Commitleri birlestirmek icin kullanilir. git rebase -i HEAD~3 komutunu calistirdigimizda gelen ekranda su sekilde degisiklik yapalim:


```
pick 490dd3c Catalogue app created
s f7edff3 Logging confs updated
pick d1ff06d Base templates created
```

Degisikligi yapip kaydettikten sonra:

f7edff3 hashli commit 490dd3c hashli commit(kendinden onceki commit) ile birlesecektir ve karsimiza yeni commit mesajini belirtebilecegimiz editor ekrani gelecektir. Bunuda kaydedip cikinca commit logumuz suna benzer birsey olacaktir.
```
$ git log --oneline
d1ff06d Base templates created
aabbccd Iki commit bir olunca samanlik seyran olur
077c05c confs updated
81ea1c6 Customer app created
c869e5c Initial commit
```
Burda dikkat edilecek nokra birlestirmek icin isaretledigimiz commit[ler] rebase ekraninda bir ust satirda yazan commit ile birlesti.
- fixup:squash gibi commit’i kendinden once yapilan commit ile birlestirir. Tek farki commit mesaji olarak fixup olarak isaretlenen committen once yapilan commitin mesajini kullanir.
- drop: Belirtilen commitleri siler.





Use git rebase. Specifically:

1. Use git stash to store the changes you want to add.
2. Use git rebase -i HEAD~10 (or however many commits back you want to see).
3. Mark the commit in question (a0865...) for edit by changing the word pick at the start of the line into edit. Don't delete the other lines as that would delete the commits.(^vimnote)
4. Save the rebase file, and git will drop back to the shell and wait for you to fix that commit.
5. Pop the stash by using git stash pop
6. Add your file with git add <file>.
7. Amend the commit with git commit --amend --no-edit.
8. Do a git rebase --continue which will rewrite the rest of your commits against the new one.
9. Repeat from step 2 onwards if you have marked more than one commit for edit.

(^vimnote): If you are using vim then you will have to hit the Insert key to edit, then Esc and type in :wq to save the file, quit the editor, and apply the changes. Alternatively, you can configure a user-friendly git commit editor with git config --global core.editor "nano".

diğer bir teknik

To "fix" an old commit with a small change, without changing the commit message of the old commit, where OLDCOMMIT is something like 091b73a:

1. git add (my fixed files)
2. git commit --fixup=OLDCOMMIT
3. git rebase --interactive --autosquash OLDCOMMIT^

You can also use git commit --squash=OLDCOMMIT to edit the old commit message during rebase.


Cannot save config file 'FileBasedConfig[/home/jenkins/.config/jgit/config]' java.io.IOException: Creating directories for /home/jenkins/.config/jgit failed


- https://mesuutt.com/2016/07/git-rebase-interactive/
- https://aliozgur.gitbooks.io/git101/content/alistirmalar/Gun_10.html
- https://aliozgur.gitbooks.io/git101/content/ileri_seviye_komutlar_ve_islemler/merge_alternatifi_olarak_rebase_kullanimi.html
- https://medium.com/@halilibrahim_4325/git-merge-ve-rebasein-fark%C4%B1-nedir-1c6c81086fa9
- https://git-scm.com/docs/git-rebase
- https://www.bryanbraun.com/2019/02/23/editing-a-commit-in-an-interactive-rebase/

- https://git-rebase.io/

- https://www.freecodecamp.org/news/the-ultimate-guide-to-git-merge-and-git-rebase/

- https://www.bryanbraun.com/2019/02/23/editing-a-commit-in-an-interactive-rebase/

- https://hackernoon.com/beginners-guide-to-interactive-rebasing-346a3f9c3a6d


**Commit Türleri**


- Merge commit (--no-ff) DEFAULT: Always create a new merge commit and update the target branch to it, even if the source branch is already up to date with the target branch.
- Fast-forward (--ff): If the source branch is out of date with the target branch, create a merge commit. Otherwise, update the target branch to the latest commit on the source branch.
- Fast-forward only (--ff-only): If the source branch is out of date with the target branch, reject the merge request. Otherwise, update the target branch to the latest commit on the source branch.
- Rebase, merge  (rebase + merge --no-ff): Commits from the source branch onto the target branch, creating a new non-merge commit for each incoming commit. Creates a merge commit to update the target branch. The PR branch is not modified by this operation.
- Rebase, fast-forward (rebase + merge --ff-only): Commits from the source branch onto the target branch, creating a new non-merge commit for each incoming commit. Fast-forwards the target branch with the resulting commits. The PR branch is not modified by this operation.
- Squash (--squash): Combine all commits into one new non-merge commit on the target branch.

- Squash, fast-forward only (--squash --ff-only): If the source branch is out of date with the target branch, reject the merge request. Otherwise, combine all commits into one new non-merge commit on the target branch.




