<a href="/atiksoftware/pubg_mobile_memory_hacking_examples/blob/master1">
<img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://github.com/eagle705/brucesplit" alt="Hits" data-canonical-src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://github.com/eagle705/brucesplit" style="max-width:100%;"></a> 

# YOLO_v3-分割法結果
## 優化或下載請使用tensorflow_yolo_split文件
本研究是使用物體檢測的技術嘗試結合航測標記點的辨識，有利於未來自動化刺點的應用，在這方面的領域，不需要快速的辨識速度，但是需要準確的辨識率及精準範圍選取。

-->分割法檔案位置為`tensorflow_yolo_split/mode/yolo_test.py`



### 主要特性

YOLO的缺點在於對於範圍廣闊的目標辨識成效不彰，對於小範圍的檢測更加優秀， 由於這機制的關係，我使用了分割法將照片的範圍縮小，這將有利於檢測的成效，這裡做了兩個版本，而碩士論文為使用keras文件作為測試數據。

|   | keras文件  | tensorflow文件  |
| :------------: | :------------: | :------------: |
|  架構 | 上採樣時沒有加上另外兩個網格特徵  | 論文架構  |
|  優點 |   近距離極佳|   中遠距離佳|
|  缺點 |  中遠距離不好|   近距離不好|

tensorflow文件內的程式已經過優化，因此檢測速度從原本3秒，降低至每張速度為0.3秒，但是還達不到論文中0.022秒一張的速度，原因在於我的電腦本身配件不算太好。
### 分割法改良(此圖數據為keeas文件架構得出的結果)
![image](https://github.com/bruce601080102/YOLO_v3-/blob/master/img/%E6%A8%A1%E5%9E%8B%E6%AF%94%E8%BC%83%E6%95%88%E8%83%BD%E6%8C%87%E6%A8%99.png)
#### 在距離上改善了yolo辨識的成果，並拉遠的辨識的距離
![image](https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/%E8%B7%9D%E9%9B%A2%E6%9B%B2%E7%B7%9A%E5%9C%96.png)
### -->四分割法在越遠的距離越能體現成效，但是還是要取決於模型本身的能力
#### 左圖是一般使用yolov3的辨識成果，右邊則是分割過後的成果
![image](https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/%E6%9C%AA%E5%91%BD%E5%90%8D-3.jpg) ![image](https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/%E5%88%86%E5%89%B2%E4%BA%94%E5%85%AC%E5%B0%BA.jpg)
------------
------------
------------
## 接下來是使用tensorflow文件中的模型:
此文件內的模型為下採樣時有加上兩種不同的網格特徵，即為yolov3論文的架構。

圖1:為使用尺寸為416大小的圖片放入模型中檢測

圖2:則是使用四分割法1024大小的尺寸

圖3:則是無使用分割法直接將圖片放大至1024後給模型檢測
## -------------------(1)    --------------------------------------(2)  
![image](https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/3%E5%85%AC%E5%B0%BA416%E7%84%A1%E4%BD%BF%E7%94%A8.jpg)  ![image](https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/3%E5%85%AC%E5%B0%BA%E4%BD%BF%E7%94%A8.jpg)
## -------------------(3)
![image](https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/3%E5%85%AC%E5%B0%BA1024_%E7%84%A1%E4%BD%BF%E7%94%A8.jpg)

可以從結結果發現好像416的尺寸辨識度較高，但是這尺寸只適合在近距離的物體上，才有較好的成效，如果拉遠至10公尺，將會檢測不到任何東西，反之越大的尺寸則辨識的距離將會遠。
尺寸拉至一定的大小將會使電腦記憶體不足或是檢測太久，以我的電腦為例，將尺寸放大至2024將會內顯不足，而將圖片放大至1024以上將會明顯的感受檢測速度變慢。

再來一點是一個重要的因素，可以發現雖然yolo可以檢測出全部的目標，但是目標的選取上還是太過隨意，這樣是無法應用至高精度建模的領域上，而使用四分割後，隨然損失一個辨識目標，但是換取更嚴僅的檢測。

## 以下是我對於yolo 模型的評判
速度上還可以再改良，如果是以論文中辨識的速度，計算分割法的速度約一秒8-10幀，傳統自動化刺點檢測三千張航拍圖大約需要一小時的時間，並且限制條件多以及不準，而一秒8幀的速度對於檢測照片刺點位置來說，已經相當足夠快速了。

# `四分割法於tensorflow_yolo_split/mode/yolo_test.py`

|   | 小尺寸  (416)  |  大尺寸(1024) | 分割法( 小尺寸)  | 分割法( 大尺寸)  |
| :------------: | :------------: | :------------: | :------------: | :------------: |
| 速度  |   0.1 s/img |   0.3 s/img  |0.6 s/img  |  1.7 s/img |
|  超近距離 |   中等|   非常低|  低 |  非常低 |
| 近距離  | 極佳  |   非常低|  中上 |中上   |
| 遠距離  |  非常低 |  中 | 中  |   極佳|
| 物體框選嚴謹度  |  中 |   中| 佳  |  佳 |

### 7/13更新:
使用opencv將框線範圍進行輪廓面積篩選，達到高精度辨識。

圖a是使用分割法在15公尺進行辨識，辨識數量20個

圖b是使用分割法+opecv在15公尺進行辨識，辨識數量20個

沒有使用分割技術，辨識數量是1個

<p align="center">
    <img src="https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/512_15.jpg" alt="Sample"  width="416" height="416">
   <img src="https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/15m_512_opncv.jpg" alt="Sample"  width="416" height="416">
    <p align="center">
        <em>(a)-----------------------------------------</em>
        <em>(b)</em></p>
### 7/27更新:

#### 航拍實測
`最遠可以在航拍15~17公尺辨識長寬15公分的目標物`
<p align="center">
    <img src="https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/15m_%E7%84%A1%E4%BD%BF%E7%94%A8%E5%88%86%E5%89%B2.jpg" alt="Sample"  width="416" height="416">
   <img src="https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/15m_%E4%BD%BF%E7%94%A8%E5%88%86%E5%89%B2.jpg" alt="Sample"  width="416" height="416">
    <p align="center">
        <em>(a)-----------------------------------------</em>
        <em>(b)</em></p>

<p align="center">
    <img src="https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/%E7%84%A1.png" alt="Sample"  width="193" height="198">
   <img src="https://github.com/bruce601080102/YOLO_v3-splite/blob/master/img/%E6%9C%89.png" alt="Sample"  width="193" height="198">
    <p align="center">
        <em>(a)-----------------------------------------</em>
        <em>(b)</em></p>

