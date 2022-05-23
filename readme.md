# Minecraft Education Edition
# C言語（c1-byod） + Pythonによるプレイヤ自動操作セット

**本プログラムはC演習で利用したc1-byod環境で動作します．**

最終更新日 2021/02/24

---

## ◇MineCraftの初期設定

**※必ずお読みください．プログラムが正常に動作しません．**

本プログラムはPythonのDirectInputを利用しマウスを操作します．そのため視点操作の感度を`100`に設定する必要があります．ゲーム画面から

`【Esc → 設定 → キーボード&マウス → 感度を"100"に変更】`

※（MINECRAFTと表示されている）タイトルメニューにある「設定」ボタンからも設定します．

プログラムを起動する際は，上記の方法で視点操作の感度を予めご確認ください．また視野角という設定がありますが，こちらはデフォルトの値を利用して下さい．

プログラム動作中はキーボードやマウス（特にマウス）を意図的に操作しようとしても正常に動作しません．プログラムが終了するまで待つか，後述するプログラムの停止方法で停止するようにしてください．

また，ディスプレイは1980x1080のディスプレイを要し，テキスト，アプリ，その他の項目のサイズを100%にしておく必要があります．

`【デスクトップ右クリック → ディスプレイ設定 → テキスト，アプリ，その他の項目のサイズを変更する → 100%】`

---

## ◇Botプログラムのダウンロード方法，実行・停止方法

###ダウンロード方法

VPNに接続してc1-byod上で以下のコマンドを実行するとダウンロードできる．フォルダは好きなところで良いが，ここでは「```~/```」に展開するとする．

``` sh
cd ~/
wget https://room404.is.oit.ac.jp/minecraft/micr_cont.zip
unzip micr_cont.zip
chmod -R 755 micr_cont
rm -f micr_cont.zip
```

上記のコマンドを実行すると```~/micr_cont/```というディレクトリが作成され，このディレクトリの中でプログラムをCプログラムを作成する事となる．また，```~/micr_cont/```の中にはtestbot.cというプログラムが置かれており，こちらを参考にプログラムを書いて下さい．ただし，かなり初心者が書いた出来の悪いサンプルプログラムですので，皆さんはこれを賢いBotプログラムに修正するようにしてください．また，以下にmicr_contフォルダのフォルダ構造を記載しておきます．

``` file
  📁micr_cont        // 作成したmicr_contフォルダ
   ┗ 📁python        // Botを動かすために必要なpythonファイル関係
     📃 control.c    // C言語からpythonを呼び出すために必要なライブラリ
     📃 control.h    // control.cファイルのヘッダファイル
     📃 testbot.c    // botプログラムのサンプル．このプログラムを参考にbotプログラムを書く
```

###ライブラリのアップデート

提供するライブラリは現在試行錯誤の段階であり，モジュールがアップデートされる場合があります．その場合は以下のコマンドでモジュールをアップデートすることができます．ダウンロードした個所が「```~/```」であった場合

``` sh
cd ~/
sh ~/micr_cont/python/minecraft/update.sh
```

とするとアップデートする事ができます．ただし，このページの末尾にあるPythonライブラリの改変を行っている学生はアップデートをしないようにしてください（作成したpythonプログラムが削除される場合があります）．

###コンパイル方法

コンパイルは以下のコマンドでコンパイルする．ここではtestbot.cプログラムを自分で作成したプログラムとしてコンパイルし，minebot.exeという実行ファイルを作成する．

```c1
cd ~/
cd micr_cont
gcc -o minebot.exe testbot.c control.c
```

コンパイルするとminebot.exeという実行ファイルが作成される．またC演習ではccというコマンドで実行していたが，本botライブラリではgccコマンドを利用する事に注意が必要です．

###実行方法

プログラムを実行する際には以下の手順で実行します．

1.Minecraftを起動する．Teamsで配布するフラット，天候の変化をOFF，またMOBはわかないようにしたマップを利用する*．
2. ゲーム画面のクロスヘアの位置を地平線に合わせてEscキーを押す（下図を参考）
3. c1-byodをアクティブ化し，作成したプログラムを実行する
4. botプログラムを実行する．

*フラットを「新しく作る」場合には，【新規→「世界のタイプ」で「フラット」を選択】で作成できます．また，【チート→「天候の変化」を「OFF」】に設定しておきましょう．

プログラムを実行すると自動でx20,y20の位置に横幅900，縦幅1080にMinecraftのゲーム画面が自動調整される．本ライブラリは画像処理を用いてゾンビを判定しているため，指定されたデスクトップの場所（x20,y20から横幅900，縦幅1080）でMinecraftを起動させる必要があります．

また，Minecraftを最小化した状態で，Botプログラムを動かすと正常に動作しません．Minecraft EEは最小化するとリソース節約のため動作停止し，起動するまでに時間がかかるため，Botの初期化処理等が正常に動作しません．

※稀にerror:detectZombieという表示が出ますが，その場合再度実行して頂ければ動作します．

###停止方法

プログラムを停止する際には「F12」を押す（連打する）と停止させることができます．停止できたらc1-byod上に終了コード送信と表示されます．

※上記でも停止しない場合はc1-byod上で`【Ctrl + C】`で強制終了できますが，正常に終了できない可能性があります．その場合はPCを再起動する等で対応して下さい．

---

## ◇Botプログラムの作成方法

###プログラム作成手順

ダウンロードしたmicr_contフォルダの中にプログラムを作成する必要があります．micr_contフォルダの中にtestbot.cを改変して頂いても結構ですし，新たにファイルを作成して頂いても結構です．その時にコンパイル方法でも注意したようにgccコマンドを使う事と，control.cをコマンドの中に入れてコンパイルするようにしてください．

Botプログラムで必ず実行しなければいけないプログラムが以下となります．実行しなければいけないプログラム（関数）のはコメントを参考にしてもらえれば良いかと思います．また，Botプログラムを書く個所は以下のプログラムのwhile文のコメントの範囲となります．

```C
#include <stdio.h>
#include <unistd.h>
#include "control.h"

int main(int argc, char *argv[]){
    init();          //Minecraftのゲームコントロール関数．ウィンドウサイズを設定する等を行う．
    setTime();       //Minecraft上で時間を夜にしてくれる．
    exePython();     //画像処理プログラムを実行する関数．実行結果はdetectZombie関数で取得できる．
    setSurvival();   //サバイバルモードにする．
    while(rk){       //無限loopする．rkはF12キーを押すと0となり，プログラムが停止します．
        /*ここからBotプログラムを書く*/

        /*ここまでBotプログラムを書く*/
        sleep(0.1);
    }
    setCreative();  //クリエイティブモードにする．
    setMorning();   //朝にする．
}
```

###ライブラリの関数一覧

関数一覧．int detectZombieに関しては次項で説明する．

|戻値 関数名(引数)             |説明
|:----------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| void init(void)             | Botプログラム初期設定関数．Minecraftのウィンドウ位置，サイズを強制的にx20，y20の横幅920，縦幅1080のする．また，マウスの初期位置を記憶する．                                                         |
| void exePython(void)        | Botプログラム初期設定関数．画像処理プログラムが実行される．                                                                                                                                   |
| void setTime(void)          | Minecraftの時間を夜に設定してくれる．                                                                                                                                                       |
|void setMorning(void)        | Minecraftの時間を朝に設定してくれる．                                                                                                                                                       |
|void setSurvival(void)       | Minecraftの設定をサバイバルモードにしてくれる．                                                                                                                                              |
|void setCreative(void)       | Minecraftの設定をクリエイティブモードにしてくれる．                                                                                                                                          |
| int detectZombie(void)      | 画像処理の結果を取得する．戻り値はint型で，1と0の7桁の値が返って来る．2桁目から7桁目の値から画像処理結果である．詳細は後述．1桁目はゾンビに攻撃が当たったかの判定．                                    |
| int detectZombie2(void)     | 画像処理の結果を取得する．戻り値はint型で，15bitの2進数結果を10進数に変換した0から4924までの値が返って来る．詳細は後述．                                                                                                          |
| void attackLeft(void)       | 左クリック．0.01秒間入力されるが，実際にはもう少し遅い．                                                                                                                                      |
| void attackRight(void)      | 右クリック．1.50秒間入力される．                                                                                                                                                            |
| void moveForward(void)      | 前進する．間隔は0.135間隔で動く．                                                                                                                                                           |
| void moveLeft(void)         | 左に動く．間隔は0.135間隔で動く．                                                                                                                                                           |
| void moveRight(void)        | 右に動く．間隔は0.135間隔で動く．                                                                                                                                                           |
| void moveBack(void)         | 後進する．間隔は0.135間隔で動く．                                                                                                                                                           |
| void cameraDown(void)       | カメラを下10度に動かす．                                                                                                                                                                   |
| void cameraLeft(void)       | カメラを左10度に動かす．                                                                                                                                                                   |
| void cameraRight(void)      | カメラを右10度に動かす．                                                                                                                                                                   |
| void cameraUp(void)         | カメラを上10度に動かす．                                                                                                                                                                   |
| void cameraPos(void)        | カメラをinit関数を実行時の地点に戻す．                                                                                                                                                      |

例えば以下のようなプログラムを記載するとc1-byod環境でF12キーを押さない限り前進し続けるプログラムとなる．

```C
#include <stdio.h>
#include <unistd.h>
#include "control.h"

int main(int argc, char *argv[]){
    init();          //Minecraftのゲームコントロール関数．ウィンドウサイズを設定する等を行う．
    setTime();       //Minecraft上で時間を夜にしてくれる．
    exePython();     //画像処理プログラムを実行する関数．実行結果はdetectZombie関数で取得できる．
    setSurvival();   //サバイバルモードにする．
    while(rk){       //無限loopする．rkはF12キーを押すと0となり，プログラムが停止します．
        /*ここからBotプログラムを書く*/

           moveForward(); 

        /*ここまでBotプログラムを書く*/
        sleep(0.1);
    }
    setCreative();  //クリエイティブモードにする．
    setMorning();   //朝にする．
}
```

###ゾンビ検出関数，detectZombie関数の仕様

地平線にクロスヘアを合わせた状態で，地面部分にゾンビがいるかを検出を行う．detectZombie関数は1と0の値である7桁の数値を戻り値する．各桁の値は以下の通り．

| 7桁目                                      | 6桁目                                          | 5桁目                                       | 4桁目                                      | 3桁目                                          | 2桁目                                       | 1桁目                                       |
|:-----|------|------|-------|------|------|-----|-------|
|地面の左上にゾンビがいれば1，いなければ0となる．|地面の真ん中上にゾンビがいれば1，いなければ0となる．|地面の右上にゾンビがいれば1，いなければ0となる． |地面の左下にゾンビがいれば1，いなければ0となる．|地面の真ん中下にゾンビがいれば1，いなければ0となる．|地面の右下にゾンビがいれば1，いなければ0となる． |1であれば攻撃がヒット，0であれば攻撃が当たってない（精度はよくない）|

例えば，detectZombie関数の戻り値が1010000の場合，地面の左上，右上にゾンビがいるとなる．また，0100100となると画面中央にゾンビがいるため，ゾンビが接近している可能性がある．そのため攻撃できる可能性があるという事になる．ただし，画面中央にいるからと言って必ず攻撃ができるという訳では無い．

本ライブラリは単純に現状のゾンビ配置からゾンビを倒そうとするとかなり難易度が高いです．ゾンビの配置を何らかの手法で記憶させておく必要があります．

また，本ライブラリはC言語から無理やり動作させるため精度が悪いです．もし，精度に不満がある場合は下の「更なる改良を行いたい方へ」をお読みください．もちろん精度の悪いなり工夫を入れると事が本コンテストの目的でもあります．

###ゾンビ動態検出関数，detectZombie2関数の仕様

利用する場合は**明るさの設定を50**にして利用してください．

detectZombie2関数を呼び出すと0～4924までの値が戻り値として返却される．戻り値は2進数を10進数に返却した15bitの値である．画像検出は以下のURLにある画像の①～⑤ように画面右下，左下，右上，左上，中央の5カ所で動態検知をしています．更にそのエリアの中で遠距離，中距離，近距離の３段階を検出し，これらを15bitの値で管理されています．また，近距離にゾンビがいる場合は攻撃が当たる範囲にいる可能性が高いです．

[detectZombie2による動態検出画像](https://oskit-my.sharepoint.com/:i:/g/personal/masaki_obana_oit_ac_jp/EV6amJG9qf9IikoaKJ8ksM8BMUpeWLJ5TaZ6FPEEtqk7Fg?e=Pga4NH)　（右クリックから「開く」を選択しないと見れないかも・・・）

戻り値を2進数に変換し，15bitを3bit区切りととなり先頭から右下 ， 左下 ， 右上 ， 左上 ， 中央の順番に遠距離を表記されています．具体的な例は以下の表を参考にして下さい．

|状態                                        |10進数 |2進数           |
|:------------------------------------------|-------|----------------|
|全検出エリアの遠くにゾンビがいる                  |18724  |100100100100100  | 
|全検出エリアの中距離にゾンビがいる                |9362  |010010010010010  |
|全検出エリアの近距離にゾンビがいる                |4681  |001001001001001  |
|検出エリア中央の近距離にゾンビがいる                |1     |000000000000001  |
|検出エリア中央に近距離と画面左上の中距離にゾンビがいる|17    |000000000010001 |

また，detectZombie2関数には以下のような欠点が存在します．
 *動くものがあるとゾンビがいると検出される可能性があります．具体的にはスポーンブロックの炎のエフェクトや剣を振る動作等．
 *同一の検出エリアにゾンビが複数体いると動態検出の面積が増えてしまい近距離にゾンビがいると検出してしまう
 *画面中央の検出精度は高いが、周辺の検出精度は低い
 *境界線をまたぐ場合、画面中央を優先した値を返却することが多い

##更なる改良を行いたい方へ

本ライブラリはpythonで書いたMinecraft操作プログラムをC言語から実行することでBotライブラリとしています．このような構造になっている目的はC演習Ⅰで学習した内容を用いてBotプログラムを作成しよーという事なのですが，もし力量があるかたはPythonプログラムの改変をして頂いても問題ありません．Pythonプログラムは以下のフォルダに存在します．また，新しいpythonプログラムを作成して頂いても問題ありません．ただし，コンテスト時には必ずC言語で書かれたコードを実行するようにしてください．

ただし，pythonプログラムを改変する場合，今後運営から提供されるアップデートは実行しないでください．実行すると改変したcontrol.cやpythonファイル等の全てモジュールが削除されてしまいます．

```file
  📁micr_cont      // 一番最初のフォルダ
   ┗📁python          // この上にはmicr_contフォルダがある．
     ┗📁minecraft       // このminecraftフォルダにpythonコードが入っている．
       ┗📃Control.py
         📃clickLeft.py
         📃clickRight.py
         :
         :
         :
         📃pushKey.py
         📃test.py
```

