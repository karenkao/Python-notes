# about keras lerning rate

## learning rate 是甚麼?

學習率是一個控制每次更新模型權重時響應估計誤差而調整模型程度的超參數。學習率選取是一項具有挑戰性的工作，學習率設置的非常小可能導致訓練過程過長甚至訓練進程被卡住，而設置的非常大可能會導致過快學習到次優的權重集合或者訓練過程不穩定。[參考:探索学习率设置技巧以提高Keras中模型性能 | 炼丹技巧](https://zhuanlan.zhihu.com/p/74115804)

## 如何尋找最優的 learning rate

使用 LR Range Test 算法，[參考:Keras 實現最優學習率尋找（LR Range Test)](https://zhuanlan.zhihu.com/p/80487971)

## 各種 learning rate decay
[參考:Tensorflow中learning rate decay的奇技淫巧](https://zhuanlan.zhihu.com/p/32923584)

learning rate 有很多種策略: 指數衰減、分段常數、polynomial_decay、natural_exp_decay、逆時間衰減、餘弦衰減、cosine_decay_restarts、線性餘弦衰減、noise_linear_cosine_decay、auto_learning_rate_decay

**繼續看這個 :　必备 | 调参技能之学习率衰减方案（一）—超多图直观对比** https://zhuanlan.zhihu.com/p/78096138
