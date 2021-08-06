# 2021-08-03 

躺在病床上讓他接近直立的影像拍照
erract_PA 主要是看有沒有空氣跑進腹腔裡面

AP-portable 用攜帶的X光機去拍攝的>成像品質比較不好

OXR_DECUBITUS 側躺 (也有分左側躺和右側躺)

氣胸 : 無法判別的是否也再分一類
氣胸的 data 有 dupicate 需要再做資料清洗
兩包資料有 70 幾張 有重名，同時在有氣胸跟在沒氣胸

使用 SIIMM 的seg 的資料，一般氣胸在上面，用RLE encoder

用seg 的mask 先轉換成 bounding box
做成 object detection
用公開資料訓練
之後拿HP做 inference 給醫生confirm 模型的acc

排程高負載

https://medium.com/bryanyang0528/deep-learning-keras-%E6%89%8B%E5%AF%AB%E8%BE%A8%E8%AD%98-mnist-b41757567684

https://hub.docker.com/r/tensorflow/tensorflow/tags?page=1&ordering=last_updated

Load Balance https://www.tensorflow.org/tfx/guide/serving

celery + rabbitmq
