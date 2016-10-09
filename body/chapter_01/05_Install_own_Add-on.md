<div id="sect_title_img_1_5"></div>

<div id="sect_title_text"></div>

# 自作のアドオンをインストールする

<div id="preface"></div>

###### これまでアドオン開発の準備ばかりで飽きてしまった方も多いと思いますが、本節ではいよいよアドオンを作成します。ここでは具体的なアドオンのソースコードの解説はせずにアドオンを作成し、作成したアドオンをインストールして使うまでの手順を紹介します。具体的なアドオンのソースコードの解説は、次節以降で行います。

## アドオンを作成する

本節では、インストールやアンインストールだけを行うことのできるアドオンを作成します。以下の手順に沿ってアドオンを作成してください。

<div id="process_title"></div>

##### Work

<div id="process_noimg"></div>

|<div id="box">1</div>|[1-3節](03_Prepare_Add-on_development_environment.md) を参考にしてコンソールからBlenderを起動します。|
|---|---|

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">2</div>|*テキストエディター* エリアのメニューバーにある *新規* をクリックして空のテキストを作成します。|![アドオン作成 手順1](https://dl.dropboxusercontent.com/s/6x7jkbaadtehb2e/blender_make_add-on_1.png "アドオン作成 手順1")|
|---|---|---|

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">3</div>| 以下に示すソースコード全文を入力します。空白は全て半角スペースで入力し、タブや全角スペースが含まれないように注意してください。|![アドオン作成 手順2](https://dl.dropboxusercontent.com/s/t6agj2bu859vk1c/blender_make_add-on_2.png "アドオン作成 手順2")|
|---|---|---|

[import](../../sample/src/chapter_01/sample_1-5.py)

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">4</div>|入力が完了したら、 *テキストエディタ* エリアのメニューバーから *テキスト* > *名前つけて保存* を実行します。|![アドオン作成 手順3](https://dl.dropboxusercontent.com/s/cbwyg0yebb8loww/blender_make_add-on_3.png "アドオン作成 手順3")|
|---|---|---|

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">5</div>|ファイル名 *sample_1-5.py* という名前で保存します。<br>[1.2節](02_Use_Blender_Add-on.md) でも解説しましたが、保存先はOSごとに異なりますので注意してください。|![アドオン作成 手順4](https://dl.dropboxusercontent.com/s/z9ibf7qz2t1jlj7/blender_make_add-on_4.png "アドオン作成 手順4")|
|---|---|---|

|OS|保存先|
|---|---|
|Windows|```C:\Users\<ユーザ名>\AppData\Roaming\Blender Foundation\Blender\<Blenderのバージョン>\scripts\addons```|
|Mac|```/Users/<ユーザ名>/Library/Application Support/Blender/<Blenderのバージョン>/scripts/addons```|
|Linux|```/home/<ユーザ名>/.config/blender/<Blenderのバージョン>/scripts/addons```|

<div id="process_start_end"></div>

---


## アドオンを有効化する

以下の手順で、作成したアドオンを有効化します。

<div id="space_xxl"></div>


<div id="process_title"></div>

##### Work

<div id="process"></div>

|<div id="box">1</div>|*情報* エリアの *ファイル* > *ユーザ設定* を選択します。|![アドオン有効化 手順1](https://dl.dropboxusercontent.com/s/7p3apgnyvjj8dl0/blender_enable_add-on_1.png "アドオン有効化 手順1")|
|---|---|---|

<div id="process_sep"></div>

---

<div id="process_noimg"></div>

|<div id="box">2</div>|*アドオン* タブを選択し、サポートレベルを *テスト中* に変更すると、今回作成したアドオンが表示されます。|
|---|---|

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">3</div>|チェックボックスをクリックし、アドオンを有効化します。|![アドオン有効化 手順2](https://dl.dropboxusercontent.com/s/ghc3rhh2wf3v9zc/blender_enable_add-on_2.png "アドオン有効化 手順2")|
|---|---|---|

<div id="process_sep"></div>

---

<div id="process_noimg"></div>

|<div id="box">4</div>|アドオンを有効化すると、コンソールに以下の文字列が出力されます。|
|---|---|

```sh
アドオンが有効化されました。
```

これにより今回作成したアドオンが有効化され、使用する準備が整いました。

<div id="process_start_end"></div>

---

<div id="space_l"></div>


## アドオンを無効化する

今回作成したアドオンは機能を持たない単純なアドオンですので、有効化・無効化以外にできることはありません。

<div id="sidebyside"></div>

|右図のように、アドオン有効化時にクリックしたチェックボックスを再度クリックしてチェックを外すことでアドオンを無効化できます。|![アドオン無効化](https://dl.dropboxusercontent.com/s/73xlppzkxu21u5w/blender_disable_add-on.png "アドオン無効化")|
|---|---|


アドオンを無効化すると、コンソールに以下の文字列が出力されます。

```sh
アドオンが無効化されました。
```

## まとめ

本節ではアドオンを作成し、作成したアドオンをインストールしてアドオンの有効化/無効化を試しました。実際にソースコードを入力してアドオンを作成してもらいましたが、ソースコードの解説が無いので何をしているかよくわからなかったと思います。

次節からは新たなサンプルを紹介し、もう少し踏み込んだソースコードの解説をしながらアドオンの作り方を紹介していきます。

<div id="point"></div>

### ポイント

<div id="point_item"></div>

* アドオンのソースコードは、Blender本体に備わっている *テキストエディタ* を用いて作成・編集できる
* アドオンの有効化/無効化は、 *情報* エリアのメニューバーから *ファイル* > *ユーザ設定* で表示される *Blenderユーザ設定* ウィンドウの *アドオン* タブから行う

<div id="space_page"></div>