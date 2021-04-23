### Intro

git aslında farklı katmanlardan ouşur.

1. en üst katmanda distribution
2. bir alt katmanda revision control system (branch, history..)
3. content tracker (dosyaların ve klasötlerin değişiminin izlenmesi)
4. core da ise disk te tutulan bir key value map store dur

içten dışa doğru inceleme yapacağız

#### Key Value Persistant Map (4. katman)

**Key Value Map**

4.cü katmanda aslında bütün dosyları hash i tutulur (sha1). ve değişiklikler bunun üezridne track edilir. kalsörlerin hatta commitlerin bile hash i vardır.

örneğin altaki kod ile bir string inputu hash i alınır

```
echo "merhaba dünya" | git hash-object --stdin

2ad1d25f2216c4cf25cbfb508d485f2350f317d8
```
bu string ifadede en ufak bir değişiklik yapacak olursak hash değişir. bu da dosyanın değiştirği anlamına gelir.

```
echo "merhaba dünya " | git hash-object --stdin
dd38a8c3d16d052a1b43ad961727d6a0d200d7c5
```

**Persistancy**

kalsörümüzü git init ile gir reposu yaptıktan sonra alttaki kodu çalıştırıp .git klasörüne bakacak olursak object kalsörü altında bazı dosya ve klasölerin olduğunu görebiliriz.

w parametersi diske yaz demektir

```
echo "merhaba dünya" | git hash-object --stdin -w
2ad1d25f2216c4cf25cbfb508d485f2350f317d8
```
info ve pack klasörlerini yok sayıp diğerlerine bakacak olursa  bir adet (bende 2a adında) klasör olduğunu görebiliriz. klasör içinde adı hash imizle aynı olan bir dosya görülmektedir.

bu dosyada bizim merhaba dünya diye yazdığıız metnin sıkıştrılmış binary blob hali bulunmaktadır.

git de bu dosyayı okumak için de komutlar bulunmaktadır.

-t type demekdir ve dostyanın tipini dönmektedir.

```
git cat-file 2ad1d25f2216c4cf25cbfb508d485f2350f317d8 -t
blob
```
-p print demektir

```
git cat-file 2ad1d25f2216c4cf25cbfb508d485f2350f317d8 -p
merhaba dünya
```

#### Content Tracker (3. katman)

bunun için bir klasör içinde birkaç dosya ile birde root da readme dosyası oluşturuyoruz.

```
liste.txt
dosyalar/dosya1.txt
dosyalar/dosya2.txt
```
bu noktada eğer bir önceki git reponuzu kullandıysanız .git klasörü altındaki object klasöründe sadece önceki dosyamızın blob dosyasını görebiliriz. Eğer yeni bir gir reposu oluşturduysanız info ve pack klasörleri dışında dosya görülmeyecektir.

eğer git status ile dosyalarımızın durumuna bakacak olursak dosyaların untrack durumunda olduğunu görebiliriz. yani git bu dosyalar hakkında henüz birşey bilmiyor yani track etmiyor bu dosyları demektir.

```
git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        dosyalar/
        liste.txt

```
dosyaları commit edersek staging area ya almış oluruz dosyaları. 

doyaları öncelikle add yapıyoruz. bu işlemden sonra status a bakacak olursak

```
git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   dosyalar/dosya1.txt
        new file:   dosyalar/dosya2.txt
        new file:   liste.txt

```


doylarımızı commitletiyp _git log_ komutu ile bakacak olursak

```
git log
commit f56470d56df19e45bade938ff2d3a97276c35be6 (HEAD -> master)
Author: Murat Çabuk <murat.cabuk@kizilay.org.tr>
Date:   Thu Apr 22 21:33:02 2021 +0300

    first commit
```
tekrar .git klasörü altındaki objects klasörüne bakacak olursak birçok klasör görebiliriz. log aldığımızda commit karşısında yeralan hash kodunun ilk iki digit i objects klasöründeki klasçrlerin isimlerine denk gelir. 

artık bu hash dosyarlarının içeriğini nasıl olukancağını biliyoruz.

fakat komutu yazdığımızda alttaki gibi bir sonuçla karşılaşıyoruz.

```
git cat-file f56470d56df19e45bade938ff2d3a97276c35be6 -p

tree c7dcd38517a7e45cedf1da70e4edd47a2bcc748b
author Murat Çabuk <murat.cabuk@kizilay.org.tr> 1619116382 +0300
committer Murat Çabuk <murat.cabuk@kizilay.org.tr> 1619116382 +0300

first commit

```
burası aslında ilgili verinin metadasını tutur. asıl context veritabanındadır. bunun için dikkat edilirse tree diye ayrı bir hash bulunmaktadır. aslında bu hash in de ilk iki digit i objects klasörü altında başka bir klasöre map etmektedir.

bu hash i de okuyacak olursak

```
git cat-file c7dcd38517a7e45cedf1da70e4edd47a2bcc748b -p
040000 tree cbca9028b3a2f44955d5b4e5bb31bf799a85a701    dosyalar
100644 blob 40e987f47262db5476ff85dfc872e8294afaa5af    liste.txt
```
yani görüleceği üzere herbir klasör için ayrı bir tree hash i yer almaktadır.

![data-model-1.png](files/data-model-1.png)

burada şuna dikat etmek grekiyr. eğer dosylardan herhangi biri sistemde içerik olarak aynen daha önce varsa onun için yeni bir dosya ve hash oluşturulmaz sistemdeki o dosyanın hash i kullanılır. Yani bizim örneğimizde dosyalar klasörü içindeki dosya1.txt ve dosya2.txt içinde aynı metinler yazıyor osaydı bu iki dosyanın içeriğini _git cat-file_ komutu ile okusaydık aynı hash i görecektik iki dosya için. yani bloblar doğrudan doya değildir dosynın içerğidir ve git dosyların veya klasörlerin bizzat kendisini tutmaz. metadalar vasıtasıyla ilgili blobların hash ini (dolayısıyla kasör yapısını da) tutmuş olur.

her bir commit aslında bir hash dir ve her commit ilgili blobların hash bilgisini ve diğer metadataları tutar. böylece aslında birebir buraki kodları bir başkası uygulaycak olsa content hashlari aynı olacak ama commit hashlarimiz farklı olacaktır çünki metadatlar farklı olacaktır (author, date vs).

#### Versioning (3. katman)

burayı anlamak için öncelikle liste.txt dosyamızın içeriğini değiştiriyoruz. commitleyip log a bakıyoruz.
```
git log
commit 562e2899f2b66c9b8c5a4eae3328de73e975be93 (HEAD -> master)
Author: Murat Çabuk <murat.cabuk@kizilay.org.tr>
Date:   Thu Apr 22 22:01:18 2021 +0300

    second comit

commit f56470d56df19e45bade938ff2d3a97276c35be6
Author: Murat Çabuk <murat.cabuk@kizilay.org.tr>
Date:   Thu Apr 22 21:33:02 2021 +0300

    first commit

```

şimdi second commit in içeriğine bakalım.

```
git cat-file 562e2899f2b66c9b8c5a4eae3328de73e975be93 -p
tree 150fe9fe5a342b53147a54aba2dd38afddd77246
parent f56470d56df19e45bade938ff2d3a97276c35be6
author Murat Çabuk <murat.cabuk@kizilay.org.tr> 1619118078 +0300
committer Murat Çabuk <murat.cabuk@kizilay.org.tr> 1619118078 +0300

second commit

```
burada dikkat edersek parent diye öncekilerden farklı bir metadata bilgisi var. bu parent üsttede görüleceği üzere ilk commit in hash i aslında. 

Ancak burada dikkaet edersek farklı bir tree oluşturulmuş. çünki aynı dosya yapısında değişen bir dosya var. bu tree hash i okuyacak olursak sadece deiştirdiğimiz liste.txt nin içerik blob hash nin değiştiğini ancak doyalar klasörünün tree hash inin öcekiyle aynı alduğunu görürüz. çünki dosyalar klasörü içeriğinde bir değşiklik olmamıştır bu nedenle önceki bloab ların hash leri (map) aynen kullanılmıştır.

![data-model-3.png](files/data-model-3.png)



```
git cat-file 150fe9fe5a342b53147a54aba2dd38afddd77246 -p
040000 tree cbca9028b3a2f44955d5b4e5bb31bf799a85a701    dosyalar
100644 blob ec30232b025202745d4c1497d93033a003a600ac    liste.txt
```

tabii aklımıza şu soru gelebilir. koca bir dosyada sadece bir satırı değştirdik diye yeni blob oluşturulması doya boyutlarını büyütmez mi? aslında git blob oluştururken ciddi sıkıştırma yapar ayrıca blobları oluştururken sadece farkları yeni blob a yazar böylece yerden tasarruf etmiş olur. 

eğer commit e tag (anootated tag denir aslında) eklersek bu bi tag bilgisi de doğrudan commit hash i içine metadata bigisi olarak yazılır.

tekar edecek olursak objects klasörü altında  4 tip obje bulunur

1. blobs
2. trees
3. commit
4. annotated tags


buraya kadar aslında en alttaki key value map ile bir üstteki katman olan tracker katmanını görmül olduk.


**Resources**

- https://git-scm.com/book/en/v2/Git-Internals-Git-Objects


#### Revision (2. katman)

**branches**


şuan sistemimizde sadece tek bir brach var o da master.

```shell
git brach

*master

```
branch bilgileri .git alttaki refs klasörü altındaki heads altında yer alır. bu dosyanın içeriğine bakacak olursak yine bir hash görürüz.

```
cat .git/refs/heads/master 
562e2899f2b66c9b8c5a4eae3328de73e975be93
```
bu hash aslında second commitimize ait sha-1 hash idir.

master dan yeni bir branch create etiğimizde de aslında master içindeki aynı hash i barındıran bir dosyanın _.git/refs/heads/branchname_ folder ında oluştuğunu göreceğiz.

![head-to-master.png](files/head-to-master.png)

yeni brach de bir değişiklik yapıp git e commitlersek aşağıdaki gibi görüntyü ortaya çıkacaktır.

![advance-master.png](files/advance-master.png)

 
active olarak hangi branch üerinde çalıştığımızı ise HEAD dosyası belirler. HEAD dosyasında branch pathi yazar. branch dosyası içinde ise hangi commir üzerinde olduğumuzu göteren hash verisi bulunur.


merge yaptığımız durumda da aslında yeni bir blob açılır commit için. bu blob read yapıldığında ise bir tree e işaret eden hash görülür. bu commit in metadata verisinde ise aynı anda iki parent  olduğu görülür, çünki iki commit merge yapılmıştır.

![fast-forward-merge.jpg](files/fast-forward-merge.jpg)

merge işlemi master üzerinde iken yapıldıysa HEAD master branch ini master branch i de son commiti göster. diğer branch in HEAD i ise eski yerindedir yani merge commit i üzerinde değildir. bu noktada eğer diğer branch i de yeni oluşturulan commit üzerinde getirmek için diğer branch üzerinde iken masterla merge yapıldığında yeni bir blob ve tree oluşturulmaz. çünki eski veriler zagen conflict ler de çözülerek yeni commite aktarılmıştır. burada commit işlemi çalıştırıldığında ekranda Fast-Forward yapıldığına dair bir mesaj görülür. çünki aslında diğer branchin HEAD'i direkt olarak con commite kaydırılmıştır.  



**orphaned commit**

eğer bir şekilde HEAD in ilerisinde commitler kalırsa bundara orphaned commit denir yada headless. bu commitler bir zaman sonra git in garbage collectoru ile silinir.

![git-reset.png](files/git-reset.png)

**Rebasing**

clasik merge işlemminde bir commit girilir iki branch bu committe birleştirilerek iki adet parent eklenir. 

rebase de ise diğer branch ile master branchin ilk ayrıdıkları yernden itibaren diğer branchin her br commiti olduğu kibi kopyalanarak master branchine yeni commit olarka eklenir. ve son olarak ilk birlelşim yerinden itibaren diğer branch silinir. 

![rebase3.png](files/rebase3.png)


**resources**

- https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell
- https://www.atlassian.com/git/tutorials/using-branches



#### Distributed System

burada distribution şunu ifade etmektedir. örneğin biz github dan kocu kopyaladık, bizim gibi başkaları da kod kopyaladı diyelim. aynı anda bir çok developer da kod ve bu koda dayalı git reposu var demektir. 

Bu durumda bir yerin referenace repo olması lazım. bu durumda bu yer github olacaktır.

biz clone lamayı yaptığımızda .git/congig doyasına bazı satırlar ekli gelir. örneğin bu dökümanın localdeki config dosyaın abakaak olursak remote da reference olarak alınba reponun adresini de görebiliriz.

ayrıoca local de tek bir branch olduğunu ve onunda origin adlı remote branch ile map landiğini görebiliriz.

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://github.com/muratcabuk/GitNotes.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master

```

local ve remote bütün branchleri listelemek için alttaki komut kullanılır.
```
git branch --all

* master
  remotes/origin/master
```

remote branch bilgileri de refs klasörü altındaki remotes klasöründe yer alır.


bu refs (refenrences) klasörü altındaki dosyaların içeriğni görmek için alltaki komutu çalıştırırız. görülceği üzere remote ve local master branch lar aynı commit üzerinde.

```
git show-ref master

49cb5e959fea43d37b8c2dbdb9bdedc29b3848a4 refs/heads/master
49cb5e959fea43d37b8c2dbdb9bdedc29b3848a4 refs/remotes/origin/master

```

**push**

biz local brach imizde aklemeler, silmeler ve değiştirmeler yaptıktan sonra push comutunu çalıştırdığımızda git sadece yeni eklenen commitleri ve ilişkili tree, ve diğer oblect lerigönderir. sebebi ise objectlerin immutable olmasıdır ve dolayısıyla biz aslında ne yaparsak yapalım hep yeni objenin eklenmesidir. Yani silme işlminde dahi yeni bir commit ve onunla ilişkili tree ve bloblar oluşur. 

**local remote conflict and pull and fetch**

diyelimki local master branch imizde birşeyler yaptık fakat aynı anda remote branch de de bazı kodlar yazıldı. bu durumda biz kodumuzu remote a push yapmak istediğimizde conflict meydana gelecektir. bu durumu düzeltmenin iki yolu var.

1. kodumucu push edergen -f flag ini kullarak remote da biz aldıktan sonra yapılan değişiklikleri orphand hale getirip local de yaptığımız değişikleri remote dan aldığımız commit e bağlayabiliriz. ancak bu durumda gir garbage collector headles kalan bu bölümü silecektir.
![git-reset.png](files/git-reset.png)]
2. ilgili conflict i local de çözümleyip öyle push edebiliriz.

bu noktada pull ile fetch arasındakiş farkı anlamamız gerekiyor. pull komutu remote brach deki değişiklikleri aldıktan sonra bir de merge yapar. fetch ise sadece remote daki diğişiklikleri alır ancak merge yapmaz.

![fetch_vs_pull.png](files/fetch_vs_pull.png)

local de conflict ler çözümlenip local de merge işlermi tamamlandıktan sonra tekrar push yapıabilir.

bu nedenle local de yapılan rebase leri remote a göndermemek gerekir. çünkü bu tarz bir merge işlemi conflict lere sebep olacaktır.

aşağıda görüleceği üzere rebase 

![rebase3.png](files/rebase3.png)





**resources**

- https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows
- https://git-scm.com/docs/git-push
