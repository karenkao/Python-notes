# about keras lerning rate

## learning rate 是甚麼?

學習率是一個控制每次更新模型權重時響應估計誤差而調整模型程度的超參數。學習率選取是一項具有挑戰性的工作，學習率設置的非常小可能導致訓練過程過長甚至訓練進程被卡住，而設置的非常大可能會導致過快學習到次優的權重集合或者訓練過程不穩定。[參考:探索学习率设置技巧以提高Keras中模型性能 | 炼丹技巧](https://zhuanlan.zhihu.com/p/74115804)

## 如何尋找最優的 learning rate

使用 LR Range Test 算法，[參考:Keras 實現最優學習率尋找（LR Range Test)](https://zhuanlan.zhihu.com/p/80487971)

## 各種 learning rate decay
[參考:Tensorflow中learning rate decay的奇技淫巧](https://zhuanlan.zhihu.com/p/32923584)

learning rate 有很多種策略: 指數衰減、分段常數、polynomial_decay、natural_exp_decay、逆時間衰減、餘弦衰減、cosine_decay_restarts、線性餘弦衰減、noise_linear_cosine_decay、auto_learning_rate_decay


## 摘要- [必备 | 调参技能之学习率衰减方案（一）—超多图直观对比](https://zhuanlan.zhihu.com/p/78096138)

在訓練模型的過程中，有個重要的工作不可少-就是調參，調參的目的在於有更好的準確率，讓模型獲得更佳的學習，這篇會學習到調參會用到的學習率衰減方案(keras learning rate schedules and decay)，這篇可以理解到keras 的標準學習率衰減，還有各種衰減方案的變形(例如:線性和多項式學習率衰減方案)。

將使用不同的學習率方案在 CIFAR-10 公開資料上進行實驗，並以準確率來評估哪一個方案比較好。

其中文章裡的: 為什麼要調整我們的學習率並使用學習率方案 ，這段寫的很清楚!!!

![image](https://user-images.githubusercontent.com/88547312/128589425-ef6be2f7-41f0-4d6c-b656-fce0f1a8ad0d.png)

接下來就是實驗各種方案，包含沒有學習率衰減及標準、線性、階梯型(step-based)和多項式學習率衰減:

1. keras中標準的衰減方案（The standard “decay” schedule in Keras）

  ```
  # 初始化优化器和模型，并编译
  opt = SGD(lr=1e-2, momentum=0.9, decay=1e-2/epochs)
  model = ResNet.build(32, 32, 3, 10, (9, 9, 9),  (64, 64, 128, 256), reg=0.0005)
  model.compile(loss="categorical_crossentropy", optimizer=opt,  metrics=["accuracy"])
  ```
  這個學習率會跟著每一次的 epoch 更新，這一個範例是keras 附帶的基於時間的一個學習率方案，接下來的學習率方案皆為作者自定義的。

2. keras中階梯型的學習率方案（Step-based learning rate schedules with Keras）

3. keras中的線性和多項式學習率方案 (作者最喜歡的兩個學習率方案)

  ```
  class PolynomialDecay(LearningRateDecay):
    def __init__(self, maxEpochs=100, initAlpha=0.01, power=1.0):
      # 存储最大的epoch数，基本学习率和多项式的幂
      self.maxEpochs = maxEpochs
      self.initAlpha = initAlpha
      self.power = power

    def __call__(self, epoch):
      # 基于多项式衰减计算得到新的学习率
      decay = (1 - (epoch / float(self.maxEpochs))) ** self.power
      alpha = self.initAlpha * decay

      # 返回新的学习率
      return float(alpha)
  ```

使用定義的學習率方案，把它放入 callback 之中
![image](https://user-images.githubusercontent.com/88547312/128590493-94ec297a-7495-40da-a200-dc789f0f15e1.png)
![image](https://user-images.githubusercontent.com/88547312/128590539-5bf503c4-8e5f-4d5d-8153-e824fcfa0a60.png)

接下來就是這次作者透過不同的學習率計畫的結果了!

1. 實驗1 : 沒有學習率衰減/時間方案

![image](https://user-images.githubusercontent.com/88547312/128590604-7546af1d-0359-44ed-8a0f-96970323a6cc.png)

我們獲得了大約85％的準確度，但正如我們所看到的，驗證loss和準確率停滯在epoch〜15之後並且在100個epoch的剩餘週期內沒有改善。

2. 實驗2 : Keras標準優化器學習率衰減

![image](https://user-images.githubusercontent.com/88547312/128590626-9cc603d4-0712-4fe3-9def-d34c32b94300.png)

這次我們只獲得82％的準確率，這方案明，學習率衰減/方案（調整方案）並不總能改善你的結果！你需要小心使用哪種學習率計劃。

3. 實驗3：階梯型學習率方案結果

![image](https://user-images.githubusercontent.com/88547312/128590658-ee3644e1-46f6-4197-ac4f-cceaf567b91e.png)

訓練/驗證loss減少 + 訓練/驗證準確率提高

4. 實驗4：線性學習率方案結果

![image](https://user-images.githubusercontent.com/88547312/128590687-12ad7558-f3c7-4799-9907-ced7a90cc371.png)

我們現在看到訓練和驗證loss的急劇下降，特別是在大約75個epoch左右; 但請注意，我們的訓練loss明顯快於我們的驗證loss - 我們可能面臨過度擬合的風險。

無論如何，我們現在可以獲得88％的數據準確率，這是迄今為止我們的最佳結果。

5. 實驗5：多項式學習率計劃結果

![image](https://user-images.githubusercontent.com/88547312/128590717-0a7656b2-126c-4806-aec2-e2d5d1774d82.png)

獲得約~86％的準確率

總結: 沒有所謂最好的學習率方案，適用於每一種資料每一個模型，因此你需要運行自己的一組實驗，包括改變初始學習率，以確定適當的學習率計劃。我建議你保留一個實驗日誌，詳細說明任何超參數選擇和相關結果，這樣你就可以參考它，並對看起來很有希望的實驗進行雙重調整。

參考: [Keras learning rate schedules and decay](https://www.pyimagesearch.com/2019/07/22/keras-learning-rate-schedules-and-decay/)

## 摘要- [Learning Rate Schedules and Adaptive Learning Rate Methods for Deep Learning](https://towardsdatascience.com/learning-rate-schedules-and-adaptive-learning-rate-methods-for-deep-learning-2c8f433990d1)


