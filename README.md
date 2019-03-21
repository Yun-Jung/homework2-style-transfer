# HW2 Report
我們一共用了三個方法來做style transfer
- MUNIT
- neural-style
- FastPhotoStyle
## Training
![](https://i.imgur.com/fOoFXnv.jpg)
## Inference
### MUNIT
MUNIT是一種非監督型的multimodal圖片轉換方法。

它的特色是會將圖片分成cotent與style兩個部分，這個步驟稱為encode，將content與style結合成一個圖片稱為decode。

訓練的方法是將兩張圖encode，接著交換彼此的cotent，再將換來的content與自己的style做decode，然後再將decode過的圖片再encode，將最後得到的content與style跟一開始的content與style做比較以得出此次轉換的效果是否良好。

因此，MUNIT不但可以保持住原本的圖片輪廓，並換成其他種風格，而且由於這種content與style分離的訓練方法，MUNIT可以保持住一張圖片的content然後移植其他圖片的style，達到一對多的轉換。

![](https://i.imgur.com/hm0XdjL.jpg)

### inference
- summer to  winter with automatic translation (10 pic)
![](https://i.imgur.com/w9QUCsQ.jpg =300x)![](https://i.imgur.com/PUzhkAQ.gif =300x)

- summer to monet style winter
![](https://i.imgur.com/KSDCHWr.jpg)
- summer to winter
![](https://i.imgur.com/MBpJO2V.jpg)

## Compare with other methods

### neural-style
neural-style 主要是以CNN的方式進行Style Transfer。
neural-style主要分成兩個部分，一個是負責辨識出content，另一個則是負責辨識出style。Content部分的CNN是由許多層layer所組成，而每個layer裡面有許多不同的filter，而被經過filter過的image就被當成是"features".
而style所提取的方式所提取的方式則是計算不同layer之間的filter response的相關性。藉由不同層級的特徵相關性，我們可以得到輸入圖片的multi-scale representation，而裡面包含了texture的訊息。

neural-style架構圖：
![](https://i.imgur.com/SXjdZOw.png)
#### inference
- summer to monet style winter
![](https://i.imgur.com/iIill4W.jpg)
- summer to winter
![](https://i.imgur.com/RN2la2U.jpg)
neural style 將photorealistic的相片轉成artistic的效果非常好。但是在photorealistic轉成photorealistic的轉換上就沒有那麼好。可以看到下面幾張圖都會有明顯的錯誤在背景上。

### FastPhotoStyle
FastPhotoStyle主要base on neural-style的VGG-19network，但nerual style在pooling layer的作用下，會將細微的特徵模糊，導致在風格轉換上，會造成不自然的結果。因此，FastPhotoStyle在network中，加入pooling mask，並在後續的reconstruction中，藉由前面pooling mask的資訊做unpooling，幫助圖片在轉換的過程中，可以保留content較細微的部分。
除此之外，FastPhotoStyle在得到經PhotoWCT後的圖片，會再根據圖片的特性做Matting Affinity，以幫助圖片在視覺上更加自然。

FastPhotoStyle架構圖：

![](https://i.imgur.com/GAkh9Yc.png)

![](https://i.imgur.com/iBWcotz.png)
#### inference
- summer to monet style winter
![](https://i.imgur.com/XMZ0K3g.jpg)
- summer to winter
![](https://i.imgur.com/qdTAZZc.jpg)
### Conlusion
![](https://i.imgur.com/NQDtpyS.png)
- MUNIT
在各種風格的轉換效果均不錯，但是在細節上會有些模糊。
Training速度慢，使用Nvidia Tesla P100要train五到六天。
Inference速度快，幾秒內即可完成。
- nerual-style
在Artistic上轉換的效果最好，但是真實照片上的效果很差。
Inference速度偏慢。
- FastPhotoStyle
效果有點像在原圖上加了一層濾鏡，但是原圖的空間資訊保留的不錯。
Inference速度快，幾秒內即可完成。
