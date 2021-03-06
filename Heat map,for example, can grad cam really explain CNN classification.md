# Heat map, for example, can grad cam really explain CNN classification?

## 出處: https://mp.weixin.qq.com/s/9B0QGfgJmPhjuo9OmoSDBg
## 論文網址: https://arxiv.org/abs/2011.06733

## 我們使用 heat map 的原因

現在常用的CNN 分類模型，都想要知道這些演算法，究竟是看圖片中的甚麼關鍵部位當作特徵，以此來做分類，所以經常使用蓋上 heat map 的方式，越紅越熱的地方就越關鍵，但是這些熱圖真的能夠解釋 CNN 是怎麼分類的嗎?

## 文中比較兩個熱圖的算法

用 Grad-CAM 和 I-GOS，把他們蓋在鳥圖上面，Grad-CAM 認為頭和翅膀比較重要，但 I-GOS 認為翅膀和肚子比較重要，這就有趣了，到底對於 CNN 誰比較重要

![image](https://user-images.githubusercontent.com/88547312/140942868-6101afc1-676f-4ffd-b94f-6dc973aead08.png)

## 因此想出一個實驗，稱作 interventional 

實驗的內容就是把圖片以黑色遮擋大部分，只露出一小小塊的區域，丟入 CNN ，看哪些仍能夠答出正確的分類

## 實驗發現

![image](https://user-images.githubusercontent.com/88547312/140943397-c275537e-225c-4a4d-9b7e-75c337ccbd24.png)

在圖中三個不同的區塊，CNN 都能夠正確的分類出來，且有高的信心度，那得到一個結論，**heat map 可以表達出這個區域是重要的，但是重要的區域不是唯一的**，這樣就會造成熱圖的解釋不夠全面

所以事實上，CNN 看到這三個區塊任一，也就是說 CNN 看 region1 or 看 region2 or 看 region3 ，他都可以分出他是鳥，所以這就是熱圖不夠全面的地方，因為熱圖無法表達 "or" 的概念

## 不用熱點圖用類似剛剛實驗的方法(搜索法)，來更全面的解釋 CNN 怎麼看圖片怎麼分類
_我比較認為他是暴力法xdd_

這個研究把圖片分成 7*7 = 49小塊，每次都只開放一小塊，就把這小塊餵進去 CNN，如果用這小塊，就可以讓 CNN 預測正確且和用整張的預測信心度差不多，那我們就可以認為這個小塊就足以讓 CNN 來分類他了!

接著使用 beam search 的方式，搜索出所有能夠分正確且相似信心度的小塊，他的流程是這樣的:

第一次開放一個小塊，第二次就在一個小塊所以在第二輪就有兩個小塊，一直到 k 輪，直到完全解釋 CNN 在圖的分類為止
![image](https://user-images.githubusercontent.com/88547312/140945197-5a5c47a4-2ffb-47c4-866e-f3e6b03619d2.png)

## 定義"完全解釋" CNN 在圖的分類

假設原本CNN對於整張圖片預測分類的機率是P，那作者定義一個極小充分解釋( minimal sufficient explanation, MSE)，這個極小的意思是，遮擋了一個小部分都會導致它預測的機率低於原來好幾倍的 P 。

那這裡定的 threshold 是遮擋的部分要超過全圖的 90% ，才被認定極小充分解釋。

## 因此對於這個 "極小充分解釋" 來做實驗

![image](https://user-images.githubusercontent.com/88547312/142757673-3bff9d04-7064-4e11-a801-a8fb467feb49.png)

從圖片可以看到作者使用的新方法， **beam search** 有 80% 的圖像可以被不超快 10 小塊可以解釋(因為整張圖切成 49 小塊, 所以10 小塊等同於 20%)，也就是用 20% 的小塊可以解釋 80% 的 imageNet 影像。

那其他的方法像是用 Grad-CAM 或是 GOS 這些視覺化方法，它只有取 10 小塊的時候，只能正確分類 imageNet 的50%的影像。

兩個相比同樣用 10 小塊(也就是全圖的20%)， beam search 可以分類正確 80% ，但Grad-CAM 或是 GOS 只能分類正確50%，可以解釋 beam search 找到模型真正關注的關鍵影像。

## 只用熱圖就想知道 CNN 分類看的關鍵區塊這個想法過於簡單

![image](https://user-images.githubusercontent.com/88547312/142762850-d2d238d2-73d6-45c1-a80a-63ee79e172f7.png)

上面這個表說明，如果是每個小區塊不能重疊，而且用 K =15 的方式去做 beam search ，那可以替每個圖片找到 1.87 個解釋，那如果小區塊可以重疊，則每一張圖片可以找到 4.51 個解釋，所以從這裡就知道，只用單張的熱圖來解釋 CNN 在圖像上就竟看哪個關鍵區塊這個想法就有點太簡單了，應該說成它並不是唯一解，應該是一個多解的問題。

## 那既然它是不能只有用熱圖來解釋，他可以怎麼使用

因為他有多個解釋，所以接下來是想要怎麼要顯示這些解釋，最簡單的方法就是把他們都列出來，就是用樹狀的結構，這裡就像是 ML 的決策樹的概念，可以呈現出如果少了哪個小塊對 CNN 的預測機率是有影響的。

這個樹狀結構作者稱為， Structural Attention Graph(SAG) ，可以看下面的圖片。

![image](https://user-images.githubusercontent.com/88547312/142762991-11e6d9f0-cc98-4e68-bcc2-59f7bc437d54.png)

紅框就是被去掉的區塊，亮的部分是輸入的區塊，看最左邊的來說，當左邊的區塊被去除後， CNN 預測的機率從 97% 降為 10% ，就可以知道去掉的這個區塊是重要的，以此類推。

## 那這個樹狀的結構可以怎麼來使用它?

這樣的解釋效果，作者將它運用於  user study ，為了測試不同的解釋是否能讓 user 明白 CNN 的分類機制，把它做成一個測驗如下:

![image](https://user-images.githubusercontent.com/88547312/142763054-b88c0f88-aece-48cb-be33-f051c657fca8.png)

user 需要在左右兩張有遮擋的圖片選擇哪一個預測的機率比較高，換句話說就是如果只看粉紅車的哪一個部分即可知道它就是粉紅車。

如果這樣的識別方式，和 CNN 的方式是相符合的， user 將可以更理解且接受模型的應用，而這樣的方式更勝過於使用單一熱圖的解釋。

## 總結

一般的熱圖不能夠全面解釋 CNN 模型關注的關鍵點們，因此找了一個新的方式更有道理邏輯的演示模型看哪些關鍵區域，使得模型的分類更有解勢力。而這樣的解釋力更適合用於說服 user。

## Handwritten notes

![image](https://user-images.githubusercontent.com/88547312/142764059-4eaf2a40-bd70-46dc-9bbd-5270f4ca4637.png)

![image](https://user-images.githubusercontent.com/88547312/142764068-74b2cc74-1b21-4a58-829f-c3e5814702e4.png)

