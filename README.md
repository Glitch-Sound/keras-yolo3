# keras-yolo3

[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](LICENSE)

## Introduction

A Keras implementation of YOLOv3 (Tensorflow backend) inspired by [allanzelener/YAD2K](https://github.com/allanzelener/YAD2K).

フォーク元は[コチラ](https://github.com/qqwweee/keras-yolo3).
実験用に色々改修中。。

---

## Dataset

`Dataset/image.zip`を解凍すること.
トレーニングデータ(約1000枚)・バリデーションデータ(約100枚)・テストデータ(約50枚)が展開される,
最終的に以下のようになればOK.
```
Dataset
  - image
      - test_n.png
      - train_n.png
      - val_n.png
  - image.zip
```


## Training

YOLOv3のweightsをダウンロード後、Keras用に変換.
```
wget https://pjreddie.com/media/files/yolov3.weights
python convert.py -w yolov3.cfg yolov3.weights model_data/yolo_weights.h5
```

`python train.py`を実行.
学習は2段階に分かれており、前半50Epoch、後半50Epochの学習が実行される.
メモリ不足などで中断した場合は`train.py内にあるbatch_size`を調整すること.
最終的に`logs/000/trained_weights_final.h5`が生成されたら成功.
```
# 例.
batch_size = 8
```
※EarlyStoppingが設定されているため、学習がこれ以上進まない場合は90Epoch程度で終了する可能性あり.


## Test

`yolo_video.py`にて検証する.
引数にて画像ファイルを指定できるよう改修済み.
異常箇所に矩形が表示されるはず.
```
#　指定例.
python yolo_video.py --image --file='hogehoge/Dataset/image/test_00000001.png''
```
