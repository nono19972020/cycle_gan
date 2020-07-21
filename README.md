# CycleGANとは
ICCV 2017の[Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks](https://arxiv.org/abs/1703.10593)で提案されたGANを用いた、画像スタイルを変換する手法です。著者の公開しているソースコードは参考にせずに、自分で論文を参考にして再現実装をしました。

# 各ソースコードの説明
- get_dataset.shは、データセットをダウンロードするシェルスクリプトです。
- custom_dataset.pyは、ダウンロードしたデータセットをテンソルに変換します。
- model.pyは、今回用いたネットワークのアーキテクチャを定義しています。
- experiment.pyは、モデルの訓練を行います。1エポックごとにテストを行います。
- test.pyは、モデルの訓練が完了したあとに、全てのテストデータに対して画像変換を行います。
- train_test.shは、experiment.pyとtest.pyを動かすためのシェルスクリプトです。

# 実行の仕方
カレントディレクトリをcycle_ganとします。
現在以下のようなディレクトリ構造になっています。
- cycle_gan
    - custom_dataset.py
    - experiment.py
    - get_dataset.sh
    - model.py
    - test.py
    - train_test.sh
## データセットのダウンロード
まずは以下のコマンドを使って画像変換を行いたいデータセットをダウンロードしましょう。
```Shell
sh get_dataset.sh file_name
```
コマンドを実行すると以下のようなディレクトリ構造になります。
dataというフォルダが作成され、その中にtrainとtestのA,Bフォルダに画像がダウンロードされます。
- cycle_gan
    - data
        - test
            - A
            - B
        - train
            - A
            - B
    - custom_dataset.py
    - experiment.py
    - get_dataset.sh
    - model.py
    - test.py
    - train_test.sh

file_nameにはダウンロードしたいデータセットの名前を代入してください。
### 利用可能なデータセット
- apple2orange
- summer2winter_yosemite
- horse2zebra
- monet2photo
- cezanne2photo
- ukiyoe2photo
- vangogh2photo
- maps
- cityscapes
- facades
- iphone2dslr_flower
- ae_photos.

## CycleGANの学習とテストを行う
データセットのダウンロードが完了したら、学習とテストをしましょう。
エポック数は100に設定しています。
```Shell
sh train_test.sh save_result_folder_name cuda_number
```
save_result_folder_nameには実験を保存するフォルダの名前を、
cuda_numberに使用するGPUの番号を代入してください。

コマンドを実行すると以下のようなディレクトリ構造になります。
cycle_ganの中にresultフォルダが作成され、
resultの中にsave_result_folder_nameが作成されます。
save_result_folder_nameに実験結果が格納されます。
その中のtrainフォルダは学習時における1エポックごとの画像変換の結果、
その中のtestフォルダはテスト時における1エポックごとの画像変換の結果、
その中のtest_100フォルダは、100エポックの学習が完了した後に全てのテストデータに対して画像変換をした結果が格納されます。
学習時とテスト時の1エポックごとのロスの遷移は、loss_train.png, loss_test.pngで保存されます。
generatorとdiscriminatorの学習したパラメータはpthファイルで保存されます。
- cycle_gan
    - data
        - test
            - A
            - B
        - train
            - A
            - B
    - result
        - save_result_folder_name
            - test
                - generated_images_A
                - generated_images_B
                - real_images_A
                - real_images_B
            - train
                - generated_images_A
                - generated_images_B
                - real_images_A
                - real_images_B
            - test_100
                - generated_images_A
                - generated_images_B
                - real_images_A
                - real_images_B
            - loss_test.png
            - loss_train.png
            - netD_A.pth
            - netD_B.pth
            - netG_A2B.pth
            - netG_B2A.pth
    - custom_dataset.py
    - experiment.py
    - get_dataset.sh
    - model.py
    - test.py
    - train_test.sh