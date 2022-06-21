# 合併多個 commit，並且同步到遠端

當我們在使用 Git 時，常常修改完內容後，會推 commit 到 github or gitlab，在一個分支上開發久了， commit 會累積很多，很雜且很亂，所以我們可以試著將 commit 給合併。

<br>

大家可以使用這個檔案來做練習：[點我 GoGo](https://github.com/880831ian/git-merge-multiple-commit) 😉

![圖片](https://raw.githubusercontent.com/880831ian/git-merge-multiple-commit/master/images/1.png)

可以看到上面這張圖，這個與[範例檔案](https://github.com/880831ian/git-merge-multiple-commit)的 commit 相似(不同專案，所以 SHA-1 也會不同，為了模擬所以 commit 相同而已)，我們模擬在同一個分支底下，有很多的 commit，那我們試著把他給合併起來。先說明一下目前的 commit 狀況，我們在 master 分支上有 3 個 commit，且已經推到遠端上。所以我們本地修改後，還要讓遠端的也合併，這個步驟要怎麼做呢？大家可以先想想看，後面會告訴大家答案 🥰

<br>

## 合併本地端 commit

首先我們目的是想要讓 add 2.txt 與 add 3.txt 的 commit 合併成 add txt，可以先使用以下指令來找到他的 commit 的 SHA-1：

```
git log
```

<br>

![圖片](https://raw.githubusercontent.com/880831ian/git-merge-multiple-commit/master/images/2.png)

<br>

要怎麼合併呢？我們先使用 rebase 到不會變動的 commit，也就是 add 1.txt 這個 commit：

```
git rebase -i 3b5bab9d5fb65b965ae55236734103b178f9daf2
```

<br>

![圖片](https://raw.githubusercontent.com/880831ian/git-merge-multiple-commit/master/images/3.png)

<br>

下完後，會跳出上面圖片內容，可以看到上面是 rebase interactive (-i) 要執行的指令，下面是每個指令的簡單說明，我們本次會使用的只有 `pick` 以及 `squash`，分別的意思是：

* pick：會執行該 commit。
* squash：會把這個版本的 commit 合併到前一個 commit。

<br>

所以我們要將它改成以下：

```
pick f8e5882 add 2.txt
squash 3eb0ef4 add 3.txt
```

<br>

也就是將 3eb0ef4 這個版本的 commit 合併到 f8e5882 的 commit，對應我們的例子，將 add 3.txt 合併到 add 2.txt 這個 commit。

儲存離開後，會跳出以下的畫面，他會告訴你原本兩個的 commit message 分別是 add 2.txt 以及 add 3.txt，這時候我們要輸入新的 commit message，也就是 add txt，建議可以把原本的訊息註解掉。

<br>

![圖片](https://raw.githubusercontent.com/880831ian/git-merge-multiple-commit/master/images/4.png)

<br>

儲存後，我們查看 `git log`，就可以看到我們將 add 2.txt 跟 add 3.txt 合併成 add txt 😝

<br>

![圖片](https://raw.githubusercontent.com/880831ian/git-merge-multiple-commit/master/images/5.png)

<br>

## 合併遠端 commit

可以看到下方是我們已經將本機端的 commit 給合併，但遠端還是一樣有 3 個 commit，如果我們就這樣直接推上去，只會多一次的 commit，所以我們該怎辦呢 ?

<br>

![圖片](https://raw.githubusercontent.com/880831ian/git-merge-multiple-commit/master/images/6.png)

<br>

我們就是要使用大家都害怕的：

```
git push -f 
```

強制覆蓋掉分支上的內容，但**切記切記**，這個只適用於自己的分支上歐～不然會直接大爆炸 💣

<br>

![圖片](https://raw.githubusercontent.com/880831ian/git-merge-multiple-commit/master/images/7.png)

<br>

## 參考資料

[如何合併多個commits](https://zerodie.github.io/blog/2012/01/19/git-rebase-i/)

[【狀況題】聽說 git push -f 這個指令很可怕，什麼情況可以使用它呢？](https://gitbook.tw/chapters/github/using-force-push)