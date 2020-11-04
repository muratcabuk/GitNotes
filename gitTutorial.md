
https://git-scm.com/book/tr/v2/Ba%C5%9Flang%C4%B1%C3%A7-Git%E2%80%99in-Temelleri

https://medium.com/@osmanhomek/git-mi-git-flow-mu-d7573e75269d

https://git-scm.com/book/tr/v1/Git-te-Dallanma-Rebasing-Tekrar-Adresleme
https://aliozgur.gitbooks.io/git101/content/bolum_1_-_baslangic/

https://www.youtube.com/watch?v=LIG-OsGysWM&list=PLPrHLaayVkhnNstGIzQcxxnj6VYvsHBHy&index=15

https://www.youtube.com/watch?v=uncrCoLiq-g&list=PLHN6JcK509bOrevTCFrSMeAfBtuib4Gpg

https://www.youtube.com/watch?v=ciRGHbfKBBo&list=PL_Z0TaFYSF3IqQKPOmbigAOVMMlZ2yU4K


https://fatihhayrioglu.com/git/


https://github.com/vigo/git-puf-noktalari/blob/master/bolum-01/patch-mod-git-add.md

http://marklodato.github.io/visual-git-guide/index-en.html

https://medium.com/data-science-tr/git-ve-github-2ad10716fb83

http://rogerdudler.github.io/git-guide/

https://www.youtube.com/watch?v=TymF3DpidJ8


## Team

https://jameschambers.co/writing/git-team-workflow-cheatsheet/

https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow

https://pewat.blogspot.com/2017/02/git-flow-nedir.html


## github ın git dışındaki özelliklerini kullanabilmek için github tarafındann geliştirilmiş hub tool u. 

https://github.com/github/hub




## Komutlar

en önemli komutlardan birisi 

git config

bununla ilgili sayfalar

https://git-scm.com/docs/git-config

https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config


ileri seviye git için git internals - izlenmeli

- https://www.youtube.com/watch?v=1mJoBwNCPfw&list=PL_Z0TaFYSF3IqQKPOmbigAOVMMlZ2yU4K&index=15


merge ve fast-forward videosu

- https://www.youtube.com/watch?v=1WFcgp3JKlk&list=PL_Z0TaFYSF3IqQKPOmbigAOVMMlZ2yU4K&index=14

rebase komutu

- https://www.youtube.com/watch?v=JW8dH2PplT0&index=16&list=PL_Z0TaFYSF3IqQKPOmbigAOVMMlZ2yU4K



1. Workspace de yapılan değişikliği en son committen dönmek için 
- git checkout -- dosyaadi

aslında bu komut son versyonu bir önceki versiyona alıyor.

iki version arasında  geçiş yapmak için aşağıdaki komut kullanılır. Nokta tüm dosyaları o versiyondaki haline gitmesi gerektiğini söyler.

- git checkout versionhashkodu -- . 

2. Son Commiti bir önceki commite dönmek için alttaki komut çalıştırılır.  bu en son yaptığımız commiti master da geri ancak workspace de geri alınacaksa ozaman üstteki kod çalıştırılır.

- git reset HEAD dosyaadi

yada istenilen ir commite tam (kesin) dönmek için --hard eki kullanılır. bu şu anlama gelir. bu committen sonrasını sil.

- git reset commitid --hard 

eğer kesin dönüş yapılmak istenmiyorsa --soft kulanılır.

- git reset commitid --soft

3. Logları tek satır halinde yazdırmak için

- git log --oneline

ayrıca daha dekoratif görmek için

- git log --oneline --graph --decorate

ayrıca tüm branch leri görmek istiyorsak yani uzakları da

- git log --oneline --graph --decorate --all

4. istenilen herhangibir committe yapılıan değişikliği silmek için yani işlemi tamamen geri almak için revert komutu kullanılır

- git revert commitid 

bu komut çalıştırıldığında direkt karşımıza otomatik oluşturulmuş commit mesaj ekranı çıkar. kaydedildiinde işlem geri alınmış olur.

5. hem stage e add yapmak hem de aynı zaman da mesaj eklemek için

- git commit -a -m "message"

6. daha önceki commit e ekleme yapmak için

- git commit -amend

7. branch oluşturmak için

- git branch branchadi

8. branch listelemek için

- git blanch -a

9. başka bir branch e geçiş yapmak için

- gir branch brachadi

10. branch clonelamak / copyalama için kopyalamak istediğimiz brache checkout ile geçiş yaptıktan sonra

- git branch -b yenibranhadi

11. Branch silmek için

- git branch -d branchadi

eğer daha önce merge edilmediği için sistem sislmeyebilir. bu durumda force etmek için

- git branch -D branchadi

12. Merge etmek için. hangi branch üzerine merge yapacaksak öncelikle o branch e checkout ile geçiş yapıyoruz ve alttaki komutu çalıştırıyoruz.

- git merge merge branchadi

13. public repositorylere (github, bitbucket) push yapmak için

- git push repository_url branchadi

14. uzak repository yi sisteme eklemek

- git remote add isim(genelde origin kullanılır) repository_url

15. uzak repository leri listelemek

- git remote -v

16. uzak repository ye isim uzerinden push etmek

- git push repository_ismi(genelde origin dir) branchadi

17. uzak repository i clone lamak için

- git clone repository_url

18. fetch sadece dosyaları getirir ancak merge yapmaz. pull ise fetch koumutu çalıştırdıktan sonra bide merge komutubu çalıştırır.

- git fetch uzakrepository_adi(genelde origin)

19. elimizde fetch yaptığımız uzak branchleri local branch merge yapmak için

- git merge origin/master (uzak repository de bulunan değiiklik üzerimnde bulunduğumuz branche e merge edilir)

20. stash e kaydetmek için

- git stash save mesajtext

21. stash listelemek

- git stash list

22. stash içeriğini görmek için

- git stash show -p stashno(0,1,2, şekline gider)

23. en son eklenen stash silmek için

- git stash drop (buraya stach id yazılırsa onu siler)

24. silinen stash geri getirmek için

-git stash pop (buraya stach id yazılırsa onu getirir)

25.  stage ile son commit in karşılaştırılması 

- git diff --cached (buradaki --cached aslında stage demek yani stage == --cached)

26. stage ile workspace in karşılaştırılması

- git diff

27. iki commiti karşılaştırmak

- git diff commitid1 

28. Head e geçiş yapmak için

- git checkout HEAD~

29. reset ile checkout farkı: reset gidilen commitin üsütndeki commitleri siler. checkout ise sadece HEAD~ i gidilmek istenen commit e götürür. reset te de geri dönüş var ancak bi süre sonra eğer silinen commitler biryere bağlanmazsa gir garbage collector bunları siler

30. local bir git repository yi remote repository olarak yayınlamak (bare komutu)

- git clone --bare . (tüm dosyalar) /hedefklasor

buraya push yapabilmek için kendi local workspace imize remote repository eklemek

- git remote add localdeki_bare_komutuyla_kopyaladığınız_klasor_adresi

daha sonra push etmek için

- git push -u origin master (localde bulunduğumuz branch i uzaktaki (burada bare ile oluşturduğumuz repository)) master branch ine push yap demiş oluyoruz)

31. Git de tracking vardır yani localdeki hangi branch uzak repository de hangi branch ile ilişkili görmek için

- git config --list


burada listede örneğin şu şekilde alanlar görülür

remote.origi.url= buraya url gelir
branch.master.remote=origin 
vs vs vs 

buradan görülebilir

32. uzak branch den lokale copye oluşturmak ve bunun uzakta hangi branch ile track edileceğini belirtmek için

- git checkout -b yeni_branch_adi --track origin/master

33. consol ekranında tree olarak görmek için

git log --oneline --graph --color --all --decorate

34. detaylı log

git reflog

35. 



