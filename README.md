# keras-yolo3

[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](LICENSE)

## Introduction

A Keras implementation of YOLOv3 (Tensorflow backend) inspired by [allanzelener/YAD2K](https://github.com/allanzelener/YAD2K).<br><br>

フォーク元は[コチラ](https://github.com/qqwweee/keras-yolo3).<br>
実験用に色々改修中。。<br>


## Dataset

Google Colab上は以下を利用.<br>
何故かGPU版を入れなくてもGPUが認識されてるぽい.<br>
```
pip install q keras==2.2.4
pip install tensorflow==1.15.0
```

GPU版のインストールが必要な場合はコレ?<br>
```
pip install tensorflow-gpu==1.14.0
```

---

## Dataset

`Dataset/image.zip`を解凍すること.<br>
トレーニングデータ(約1000枚)・バリデーションデータ(約100枚)・テストデータ(約50枚)が展開される.<br>
最終的に以下のようになればOK.<br>
```
Dataset
  - image
      - test_n.png
      - train_n.png
      - val_n.png
  - image.zip
```


## Training

YOLOv3のweightsをダウンロード後、Keras用に変換.<br>
```
wget https://pjreddie.com/media/files/yolov3.weights
python convert.py -w yolov3.cfg yolov3.weights model_data/yolo_weights.h5
```

`python train.py`を実行.<br>
学習は2段階に分かれており、前半50Epoch、後半50Epochの学習が実行される.<br>
メモリ不足などで中断した場合は`train.py内にあるbatch_size`を調整すること.<br>
最終的に`logs/000/trained_weights_final.h5`が生成されたら成功.<br>
```
# 例.
batch_size = 8
```
※EarlyStoppingが設定されているため、学習がこれ以上進まない場合は90Epoch程度で終了する可能性あり.<br>


## Test

`yolo_video.py`にて検証する.<br>
引数にて画像ファイルを指定できるよう改修済み.<br>
異常箇所に矩形で囲われた画像が表示されるはず.<br>
```
#　指定例.
python yolo_video.py --image --file='hogehoge/Dataset/image/test_00000001.png''
```
