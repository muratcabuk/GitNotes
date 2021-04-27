tabiri caizse zulalamk gibi bir anlamı vardır. Yaptığımız bazı işleri bu alan koyup ileride kullanak üzere tekrar work area'ya almak için kullanılır. 

Kullanım alanlarına bazı örnekler

- Örneğin tam işin ortasındayken bir hotfix geldi diyelim. bizde yaptıklarımızı kaybetmek istemiyoruz. bu durumda yaptıklarımızı stash area'ya atabiliriz.
- yada bir kodu test etmek ve daha sonra başka bir versiyonu daha test edip aralarında karar vermek istiyoruz diyelim.

work area'daki kodu stash a atmak için alttaki komut kullanılır. bu komut uygulandığında dosyalar work area ve stash areadaki dosyalar silinir ve stash a eklenir.

```
git stash --include-untracked
```
burada --include-untracked opsiyonunu belirtmesek sadece stage(index) area'daki track edilen dosyları stash area'ya alır, work area'daki dosyaları almaz.

stash deki commitlerin listesi almak için alttaki komut kullanılır.

```
git stash list
```
index deki sıfırıncı stash i tekrar work ve stage' a yğklmek için 

```
git stash apply
```

stash deki en üstteki index deki stash i apply edip simek için

```
git stash pop
```
index belirterek işlem yapmak için

```
git stash pop stash@{2}

```
veya 

```
git stash apply stash@{2}
```
stash dan silme yapmak için

```
git stash drop stash@{2}
```




```shell
git stash list [<log-options>]
git stash show [<diff-options>] [<stash>]
git stash drop [-q|--quiet] [<stash>]
git stash ( pop | apply ) [--index] [-q|--quiet] [<stash>]
git stash branch <branchname> [<stash>]
git stash [push [-p|--patch] [-k|--[no-]keep-index] [-q|--quiet]
	     [-u|--include-untracked] [-a|--all] [-m|--message <message>]
	     [--pathspec-from-file=<file> [--pathspec-file-nul]]
	     [--] [<pathspec>…​]]
git stash clear
git stash create [<message>]
git stash store [-m|--message <message>] [-q|--quiet] <commit>
```



**resources**
- https://git-scm.com/docs/git-stash
- https://www.atlassian.com/git/tutorials/saving-changes/git-stash