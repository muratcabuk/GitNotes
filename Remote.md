### Remote Eklemek

```
git remote add name(genelde origin) <URL>
```
borada origin farklı isimde olabilir ancak origin ismi artık bi standart olmuş. Belki birden fazla remote ekleme durumunda farklı isimlerde verilebilir.

### Remote Listeleme

```
git remote

# veya daha detay istiyorsak

git remote -v
```

### Remote Silmek

```
git remote rm remote_name
```
### Remote Adını Değiştirmek

```
git remote rename old_remote_name new_remote_name
```

### Remote'dan değiiklikleri çekmek

```
git fetch

git fetch remote_name

git pull

git pull remote_name

# Merge yerine rebase kullanarak değişiklikleri almak için
git pull --rebase remote_name

```

### Remote'a değişiklikleri göndermek

```
# Yerel daldaki değişiklikleri uzak depoya gönder 
git push

# Bir daldaki değişiklikleri uzak depo kısa yolu ile ilişkilendirilmiş uzak depoya gönder
git push remote_adi(örneğin origin) branch_name

# Tüm yerel dallardaki değişiklikleri uzak depo kısa yolu ile ilişkilendirilmiş uzak depoya gönder
git push remote_adi(örneğin origin) --all

# Yerel depodaki tag’leri uzak depoya gönder
git push remote_adi(örneğin origin) -- tags
```

### Tracking Branches

```
git checkout -b local_branch_name remote_name/remote_branch_name

# examples

git checkout -b master origin/master
git checkout -b feature1 origin/master
git checkout -b feature1 origin/feature1

```
current rranch üzerinde iken track yapmak için

```
git checkout --track remote_name/remote_branch_name

# examples

git checkout --track origin/master

```
### Remote'a merge yapmak

```
# current branch i merge yapmak için

git merge origin/serverfix

```

ayrıca bir de -u parametresi ile de remote a branh yayaınlyabiliriz.

Önce git checkout komutu ile branch'imizi aktif hale getiriyoruz ve sonra git push komutu ve -u seçeneği ile local branch'imizi remote repository'mizde yayınlıyoruz. Push komutu için verdiğimiz origin ve superyeniozellik değerleri ile HEAD branch'imizi origin remote repository'de superyeniozellik isimli branch olarak yayınlanmasını istediğimizi tanımlıyoruz. -u seçeneği ise local branchimiz ile remote branchimiz arasında, önceki bölümlerde de bahsettiğimiz, Takip İlişkisi (Tracking Relationship) kurulmasını sağlar.


### Remote dan clone almak

```
git clone remote_url
```
belirli bir klasörü klone lamak

```
git clone <repo> <directory>
```

belirli bir tag i clone lamak

```
git clone --branch <tag> <repo>
```

shallow clone (belirli bir history yi kopyalamak)

```
git clone -depth=1 <repo>
```

belirli bir bracnh i clone lamak

```
git clone -branch new_feature git://remoterepository.git
```
sadece .git klasörünü clone lamak. yani working dir yok. bu da doğrudan editleme yapılamaz demek oluyor clone üzerinde 

```
# remote daki .git klasörünü locale example klasör adıyla clone lar.
git clone --bare https://github.com/example
```
bir de --mirror opsiyonu var. --bare den farklı olarak bütün refs detaylarını da kopyalar.

- https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-clone


### diğer komutlar

https://git-scm.com/docs/git-remote

```
git remote [-v | --verbose]
git remote add [-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=(fetch|push)] <name> <url>
git remote rename <old> <new>
git remote remove <name>
git remote set-head <name> (-a | --auto | -d | --delete | <branch>)
git remote set-branches [--add] <name> <branch>…​
git remote get-url [--push] [--all] <name>
git remote set-url [--push] <name> <newurl> [<oldurl>]
git remote set-url --add [--push] <name> <newurl>
git remote set-url --delete [--push] <name> <url>
git remote [-v | --verbose] show [-n] <name>…​
git remote prune [-n | --dry-run] <name>…​
git remote [-v | --verbose] update [-p | --prune] [(<group> | <remote>)…​]

```