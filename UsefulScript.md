

#### git tree komutu oluşturmak

çağırırken şu komut kullanılır

```
git t1
```


version 1
```
git config --global alias.t1 "log --graph --decorate --pretty=oneline --abbrev-commit --all" 
```

version 2

```
git config --global alias.t2  "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"
```
version 3

```
git config --global alias.t3  "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all"
```

version 4

```
git config --global alias.t4 "forest --pretty=format:\"%C(red)%h %C(magenta)(%ar) %C(blue)%an %C(reset)%s\" --style=15 --reverse"
```
version 5
```
git config --global alias.t5 "log --graph --date-order --date=short --pretty=format:'%C(auto)%h%d %C(reset)%s %C(bold blue)%ce %C(reset)%C(green)%cr (%cd)'"
```
