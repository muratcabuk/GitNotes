### Tanımlar


- [Interaktif Git Cheatsheet](http://ndpsoftware.com/git-cheatsheet.html) - Kesinlike Bakılmalı
- [animasyonlu anlatım](http://onlywei.github.io/explain-git-with-d3/#commit)

- **Git Alanları**

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



### Merge Strategies


- **Fast Forward ff:** 


```        
        C - D - E           bugfix
      /
A - B                       master

```

git merge --ff

```
A - B - C - D - E           master/bugfix
```

- **No fast-forward no-ff:**


```        
        C - D - E           bugfix
      /
A - B                       master

```

git merge --no-ff

```
        C - D - E           bugfix
      /           \
A - B ------------ F        master

```


- **Recursive strategy**

üstteki ff den farklı olarak master da yerinde sabit durmadan devam etmiş.

```
        C - D - E           bugfix
      /
A - B - F - G               master

```

git merge --no-ff

```
        C - D - E           bugfix
      /           \
A - B - F - G ----- H       master

```

- **Squash merge**

```

        C - D - E           bugfix
      /
A - B - F - G               master

```
git merge --squash

```
        C - D - E           bugfix
      /
A - B - F - G - CDE         master
```
### Diff

![transport3](files/conventions.png)

![transport3](files/diff.png)



### Reset, Checkout ve Revert Tanımları

https://marklodato.github.io/visual-git-guide/index-en.html



- **Checkout**

**dosya adı belirtilerek yapılırsa sadece ilgiliş dosyalrı belitilen noktadan stage ve workspace alanına kopyalar/getirir.dosya adı belirtilmezse branch/commit ler switch edilir. yani hedef branch/commit workspace ve stage e alınır.** 

checkout ile başka bir committe gittiğimiz an HEAD oraya taşınmış olur bunda da **detached HEAD** denir.


![transport3](files/conventions.png)

```
$ git checkout HEAD~ files(yani dosya isimleri)  
```
Henüz commit etmediğimiz değişikliklere Local değişiklik denir. Bazen önceki halinden daha kötü olan kod yazabilirsiniz ve bu değişikliği geri almak isteyebilirsiniz. Bu gibi durumlarda değiştirdiğiniz halinden memnun olmadığınız dosyadaki değişiklikleri geri alıp dosyanın son commit edilmiş haline geri dönmek istediğinizde, önceki bölümlerde de sıkca kullandığımız, git checkout komutunu -- seçeneği ile çalıştırmanız yeterli olacaktır.

![transport3](files/checkout.png)

```
$ git checkout -- dosya1.md veya
$ git checkout -- klasor/dosya2.md 
```
şeklinde kullanabilirsiniz.




![transport3](files/checkout2.png)
farklı bir branch/commit e switch etmek için dosya adı yazılmadan branch veya commit adı yazılır. örneğin üstteki örnekte maint branch ine switch edilmiş.

Altaki örnekte de master branch in de 3 adım geri gidilmiş. master yerine HEAD~3 şeklinde de yazılabilirdi. Bu durumda üzerinde çalıştığımız aktif branch de 3 adım geri giderdi.

![transport3](files/checkout3.png)


eğer check komutuyla birlikte -b flagi kullanılark bir isim verielirse üzerinde bulunduğumuz committen bir branch create edilir.

![transport3](files/checkout4.png)

eğer branch adı ile birlike aşağıdaki örnek komutta olduğu gibi uzak repodaki branch bilgisi de verilirse bağlantı kurulmuş olur.

```
$ git checkout -b <new-branch> --track <remote-branch>

$ git checkout -b new-branch --track origin/develop

```

- **Soft, Hard ve Mixed Reset**

Git reset komutuna soft parametresini verip bir commit belirtirseniz eğer, Git bu belirttiğimiz commiti ve sonrasındaki commitleri silecektir, düzenlenmiş dosyalar da Git’e eklenmiş hale gelecektir. Dosyalardaki değişiklikler bozulmayacaktır.

Git reset komutuna mixed parametresini verdiğimiz zaman ise Git, bu belirttiğimiz commiti ve sonrasındaki commitleri silecektir. Dosyalarımızdaki değişiklikler gitmeyecek ancak dosyalarımız Git’e eklenmemiş olacaktır, yani untracked hale gelecektir.

Git reset komutuna hard parametresini verip bir commit belirttiğinizde ise bu belirttiğiniz commiti ve sonrasındaki commitleri tamamen silip dosyalarda yaptığınız değişiklikleri de geri alıyor. Yani yaptığınız her şey uçup gidiyor. Bunu kullanacağınız zamanlarda çok dikkatli olun.

![transport3](files/reset.png)


![transport3](files/reset2.png)



eğer reset komutuna -- files(dısya isimleri) şekilde flaglar kullanılırsa. aynı checkout un dosya isimleri kullanılırkenki hali gibi çalışır tek fark sadece stage değişik working direktory değişmez. aşağıdaki resim bu duruma örnektir.

![transport3](files/reset3.png)

- **Revert**

kaynak : https://aliozgur.gitbooks.io/git101/content/ileri_seviye_komutlar_ve_islemler/degisikliklerinizi_geri_almak.html

git revert komutu commit ettiğiniz herhangi bir değişikliği geri almak için kullanılır. Bu komut ile commit işleminizin kendisi veya bilgileri silinmez sadece commit işleminizdeki değişiklik geri alınır. Örneğin eklediğiniz bir satırı kaldırmak isterseniz git revert komutu ile bunu yapabilirsiniz. Aslında git revert komutu değişikliğinizi geri almak için otomatik olarak yeni bir commit oluşturur ve geri alma işlemi bu commit sayesinde değişiklik tarihçesinde görünür hale gelir.

Değişiklikleri geri almak için kullanabileceğimiz diğer bir komut ise git reset komutu. Bu komut da herhangi bir bilginizi silmeden işlemi gerçekleştirir, ancak git revert komutundan farklı olarak otomatik yeni bir commit üretmeden değişikliğinizi geri almanızı sağlar.

![transport3](files/revert.jpg)

Yukarıdaki ekran görüntüsünde ilk önce git revert komutunu çalıştırdık. Bu komutun en önemli parametresi geri almak istediğimiz commit'in hash değeri (hash'in ilk yedi karakterini kullanabiliriz). Komutu çalıştırdıktan sonra değişiklik tarihçesini incelediğimizde git'in otomatik olarak bir commit oluşturduğunu ve bu commit'in bilgilerinde hangi değişikliğin geri alındığına dair ayrıntıların yer aldığını görüyoruz.)

### Merge,Cherry Pick and Rebase

[merge ve rebase hakkında daha fazla detay için bu sayfayı kullanbilirsiniz](merge_rebase.md)

- **merge**
![transport3](files/merge2.png)

- **cherry pick**

![transport3](files/cherry_pick.png)

- **rebase**

![transport3](files/rebase.png)

üstteki komut ile topic branchindeki commitler sırasıyla master a uygulanız.

alttaki komutlr yani --onto ile topic branchinde ne kadar geriye gidileceği belirtiliyor.

![transport3](files/rebase2.png)



