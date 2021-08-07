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


[Learning Rate Schedules and Adaptive Learning Rate Methods for Deep Learning](https://towardsdatascience.com/learning-rate-schedules-and-adaptive-learning-rate-methods-for-deep-learning-2c8f433990d1)
