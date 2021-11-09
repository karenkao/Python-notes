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

## 定義"完全解釋 CNN 在圖的分類"

