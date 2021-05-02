### Index

- [How Git Works](HowGitWorks.md)
- [Git Tutorial](gitTutorial.md)
- [Branching and Solving Conflicts](BranchingAndSolvingConflicts.md)
- [Remote](Remote.md)
- [Commit Part Of File](CommitPartOfFile.md)
- [Checkout Rm Revert Reset Clean.md](CheckoutRmRevertResetClean.md)
- [FastFoward vs CherryPick vs Rebase vs Squash.md](FastFowardVsCherryPickVsRebaseVsSquash.md)
- [Stashing](Stashing.md)
- [Fixing History and Interactive Rebase](FixingHistoryAndInteractiveRebase.md)
- [Git Flow and Best Practices](GitFlowAndBestPractices.md)
- [Filter and FilterSpec](FilterAndFilterSpec.md)
- [sparse-checkout](sparse-checkout.md)
- [Submodules](Submodules.md)
- [Useful Script](UsefulScript.md)
- [Yapılması Gereneknler](YapilmasiGerekenler.md)
  ### Tanımlar

- upstream : hedef, target
- diverged : ayrılan, ayrılmış

**Faydalı Linker**
- [Interaktif Git Cheatsheet](http://ndpsoftware.com/git-cheatsheet.html) - Kesinlike Bakılmalı
- [animasyonlu anlatım](http://onlywei.github.io/explain-git-with-d3/#commit)


### Git Alanlar

![transport](files/git-transport.png)

![transport2](files/git-transport2.png)


temelde 4 alan var.
1. Workspace: aktif olarak çalıştığımız alan yani text editörde açıp editleme yapabildiğimiz alan.
2. Index: local repoya gidecek olan dosyları indexlendiği alan (git add .).
3. Local Repo: local repoya index den alınmış (git committ) dosyaların bulunduğu alan.
4. Remote Repo: uzak repo (github, gitlab ve ya bitbucket)


- Fetch: uzak sunucudan son pull dan beri birdeğişiklik olup oladığı meta bilgisini alır. değişiklik olan dosyları alır ancak locak dosyalar entegre etmez.
- Pull: uzak sunucudan gerçekten değişen dosyaları alır ve local e entegre eder. slında pull fetch yaptıktan sonra merge yapar.
![git](files/git.png)



![transport3](files/git-transport3.png)


