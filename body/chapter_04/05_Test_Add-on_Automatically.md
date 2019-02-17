<div id="sect_title_img_4_5"></div>

<div id="sect_title_text"></div>

# アドオンの動作テストを自動化する

<div id="preface"></div>

###### [4-3節](03_Publish_your_Add-on.md)でも書いたように、アドオンを公開した後は、バグ修正や機能追加などのサポートをきちんと行うべきです。しかし、アドオンのソースコードを修正するたびに、アドオンの動作テストを毎回行うのは面倒です。特に、アドオンの規模が大きくなってくると、テストだけで時間の大半を使ってしまいます。<br>本節では、コマンドラインからBlenderを実行できることを活用し、アドオンの動作テストを自動化する方法を説明します。


## アドオンの動作テストを自動化する利点

バグ修正や新機能の追加のためにソースコードを修正し、これまで正しく動作していた既存の機能が正しく動作しなくなってしまったというのは、ソフトウェアの開発ではよくあることです。ソースコードを修正しなければこのようなことは起こりませんが、これではソースコードが修正するなと言っているようなもので、問題解決できていません。そもそもソースコードを修正することでバグが発生してしまうのは、ソースコードの修正の影響が及ぼす範囲をきちんと把握できていないことが原因です。しかし、ソースコードを修正するたびに既存の機能の動作テストを毎回手作業で行っていたのでは、テストに時間をとられてしまい、本来のアドオンの開発ができなくなってしまいます。また、Blenderのアドオンを開発する場合は、Blenderのバージョンごとに仕様が変わるAPIについても意識する必要があります。Blenderのバージョンが変わったことで、今まで動作していた機能が正しく動作しなくなる問題は、コミュニティサイトに投稿されるバグ報告としてよくあることです。このため、新しいバージョンのBlenderがリリースされるたびにアドオンのすべての機能をテストする必要がありますが、これが結構大変な作業になります。

このように、アドオンのテストは非常に手間がかかります。そこで、アドオンのテストのような毎回同じことを繰り返す作業は、自動化してしまいましょう。


## コマンドラインからBlenderを実行

BlenderはGUIベースのソフトウェアですので、アドオンの機能が正しく動作していることを動作確認するためには、マウスやキーボードを使った操作が必要になります。このため、テストを自動化することが難しいと感じるかもしれません。しかし幸いなことに、BlenderはコマンドラインでPythonスクリプトを実行するモード（CUIモード）が標準で備わっているため、CUIモードを活用してアドオンのテストを自動化することができます。

Blenderをコマンドラインから実行するためには、[1-3節](../chapter_01/03_Prepare_Add-on_development_environment.md)で紹介したように、コンソールウィンドウからBlenderを起動します。しかし、[1-3節](../chapter_01/03_Prepare_Add-on_development_environment.md)で紹介した方法でBlenderを起動すると、BlenderがGUIモードで起動してしまいます。BlenderをCUIモードで起動するためには、次のように ```--background``` オプションを指定して実行します。以降の説明では、Blenderの実行ファイルが置かれているパスが ```/path/blender``` であるとします。

```sh
$ /path/blender --background
```

上記のコマンドを実行すると、BlenderがCUIモードで起動します。しかしCUIモードで起動した場合、Blenderが起動した直後に終了してしまい、CUIモードで起動することに意味があるのかわからないかもしれません。CUIモードが強力であるのは、BlenderをGUIで開くことなく、オプション ```--python``` の引数に指定したPythonスクリプトを実行できるところにあります。例えば、レンダリング処理を実行するPythonスクリプトを作成し、高性能なレンダリングを行うサーバでBlenderをCUIで実行することで、ネットワークレンダリングを行うことができます。また、Pythonスクリプトを使って複数の.blendファイルに対して同じ処理を繰り返し行いたい場合にも、コマンドラインから実行することで作業時間を短縮することができます。

ここでは、次に示す簡単なPythonスクリプトを作成し、BlenderのCUIモードで実行します。スクリプトは、```/path/blender``` が配置されているディレクトリと同じところに、ファイル名 ```run_script_on_cui_mode.py``` として保存します。

```python
a = 50
b = 12
print("a+b=%d" % (a+b) )
```

次に、コンソールウィンドウから次のコマンドを実行します。

```sh
$ /path/blender --background --python run_script_on_cui_mode.py
```

コマンドを実行すると、次のメッセージが出力されてBlenderが終了します。

```sh
a+b=62
```

このように、BlenderのCUIモードでPythonスクリプトを実行することができます。本節では、CUIモードでPythonスクリプトを実行できることを利用し、アドオンのテスト自動化について説明します。アドオンの機能をテストするPythonスクリプトを実行することで、マウスやキーボードを用いることなくアドオンのテストを行うことができます。

ここで、CUIモードでBlenderを起動した場合は、デフォルトでアドオンが有効化されないことに注意が必要です。CUIモードでBlenderを実行し、作成したアドオンを有効化するためには、```--addons``` オプションの引数に有効化したいアドオンのパッケージまたはモジュール名を指定します。例えば、モジュール名が ```addon_test``` であれば、次のコマンドを実行することにより、アドオン ```addon_test``` が有効化された状態でBlenderが起動します。そして、Blender起動後にスクリプト ```run_script_on_cui_mode.py``` が実行されます。

```sh
$ /path/blender --background --addons addon_test --python run_script_on_cui_mode.py
```


## アドオンの機能をスクリプトから実行する

[2-2節](../chapter_02/02_Register_Multiple_Operation_Classes.md)で紹介したように、有効化したアドオンの機能は、スクリプトから ```bpy.ops.<オペレーションクラスのbl_idname>``` で実行することができます。テストするアドオンを有効化した状態でBlenderを実行し、有効化したアドオンの機能をスクリプトから実行することで、アドオンのテストが行えます。しかし、機能によってはマウスやキーボードが必要なものもあり、スクリプトではテストできないものがあることには注意が必要です。自動化できない機能については、やはり人手で行う必要があります。


## アドオンの動作テストを自動化する方法

CUIモードでスクリプトを実行する方法を説明しましたが、次にアドオンの動作テストを自動化する方法を紹介します。ここではGitHubとTravis CIを連携し、アドオンに対して行った修正をGitHubのリポジトリにpushした時にテストを行い、アドオンの動作テストを自動化する方法を紹介します。


### GitHubに登録する

[4-3節](03_Publish_your_Add-on.md)で紹介したように、GitHubは数あるソースコード管理サイトの中で最も有名なサイトです。このGitHubの機能の1つに、外部サービスとの連携があります。外部のサービスと連携することにより、例えば更新したソースコードをpushした時に、pushした内容が正しいものであるかを外部サービスを利用して確認することができます。GitHubへのアカウントの登録の仕方については、すでに多くの情報がWeb上にあるため、本書では割愛します。以降の説明では、GitHubにアカウントを登録済みのものとして説明します。

GitHubへの登録が完了したら、GitHubにリポジトリを作成します。本節では、```Testing_Blender_Addon_With_Travis_CI``` という名前でリポジトリを作成することを前提に説明します。作成したリポジトリをTravis CIと連携させることで、連携したリポジトリに対してソースコードの修正をpushすると、テストがTravis CI上で自動的に行われるようになります。


### Travis CIに登録する

Travis CIという名前が突然出てきましたが、ここで初めて名前を聞いたという方も多いと思います。Travis CIを説明する前に、CIについて理解しておいた方がよいと思うので、ここで簡単に説明します。

CIとはContinuous Integration（継続的インテグレーション）の略で、ソースコードのビルドやテストをソースコード更新のたびに行うことを指します。ソースコードを修正すると、これまで動作していた機能が動作しなくなる、といった問題が生じることがあると前に書きました。このような不具合を少しでも減らすため、常にソースコードをテストし、問題の発生をできるだけ早く見つけることがCIを行う理由の1つです。CIサービスはCIの実施を支援するサービスで、ソースコードの修正をリポジトリにpushしたときにテストやビルドを自動的に行い、その結果を表示することができます。Travis CIは、そのCIサービスの1つです。CIサービスには他にもJenkinsなどがありますが、本書ではGitHubとの連携が簡単なTravis CIを使って説明します。


<div id="webpage"></div>

|Travis CI|
|---|
|https://travis-ci.org/|
|![Travis CI](https://dl.dropboxusercontent.com/s/d5wmzeuhv93nufx/travis_ci.png "Travis CI")|


GitHubと同様、Travis CIへのアカウントの登録の仕方についてもすでに多くの情報がWeb上にあるため、本書では割愛します。Travis CIはGitHubのアカウントを使って登録するため、GitHubにアカウントを登録済みであれば非常に簡単な手順でアカウント登録できるため、困ることはないと思います。


### GitHubとTravis CIを連携する

GitHubとTravis CIとの連携は、次の手順で行います。


<div id="process_title"></div>

##### Work

<div id="process"></div>

|<div id="box">1</div>|右上のユーザのアイコンをクリックして表示されるメニューから、*Accounts* をクリックしてアカウント情報を表示します。GitHubで作成したリポジトリの一覧が表示されますので、テスト対象のリポジトリを有効化します。|![GitHubとTravis CIの連携 手順1](https://dl.dropboxusercontent.com/s/zh7p3nmnt9sx6u7/link_to_travis_ci_1.png "GitHubとTravis CIの連携 手順1")|
|---|---|---|

<div id="process_sep"></div>

---


<div id="process"></div>

|<div id="box">2</div>|リポジトリ有効化ボタンの隣にある歯車マークをクリックすると、テスト（ビルド）を実行する契機を確認することができます。|![GitHubとTravis CIの連携 手順2](https://dl.dropboxusercontent.com/s/h7vf50tbmyzivi6/link_to_travis_ci_2.png "GitHubとTravis CIの連携 手順2")|
|---|---|---|

<div id="process_sep"></div>

---


<div id="process"></div>

|<div id="box">3</div>|デフォルトでは、*Build pushes* （プッシュ時にビルド実行）と *Build pull requests* （プルリクエスト時にビルド実行）の2つが有効化されています。|![GitHubとTravis CIの連携 手順3](https://dl.dropboxusercontent.com/s/f836k1u4ab1j9z3/link_to_travis_ci_3.png "GitHubとTravis CIの連携 手順3")|
|---|---|---|


<div id="process_start_end"></div>

---


ここまでの手順を見てもらえばわかると思いますが、GitHubとTravis CIとの連携はクリックを数回行うことで完了する、非常に簡単なものとなっています。


### テスト対象のアドオンの作成

Travis CIを使ってテストするために、テスト対象とするアドオンを作成します。ファイル名は ```testee.py``` とします。

[import](../../sample/src/chapter_04/sample_4_5/testee.py)

作成するテスト対象のアドオンには、2つのオペレータクラスが定義されています。```TestOps1``` は何もせずに ```{'FINISHED'}``` を返すオペレーションです。```TestOps2``` はオブジェクト名がCubeであるオブジェクトが存在する場合に ```{'FINISHED'}``` 、存在しない場合に ```{'CANCELLED'}``` を返すオペレーションです。これら2つのオペレーションに対してテストを行います。


<div id="space_m"></div>


### アドオンテスト用スクリプトの作成

アドオンをテストするためのスクリプトを作成します。ファイル名は ```test.py``` とします。

[import](../../sample/src/chapter_04/sample_4_5/test.py)

スクリプトの処理自体は単純で、```testee.py``` で登録したオペレータクラスの ```bl_idname``` を使ってアドオンの機能を呼び出し、その結果を確認するだけです。

アドオンの機能を呼び出した後は、戻り値を ```assert``` 文で判定します。```assert``` 文は、第1引数の条件が偽の場合に ```AssertionError``` 例外オブジェクトを発生させます。そして、```except``` 処理の中で ```assert``` 文の第2引数に指定した文字列（第2引数の値は ```e``` に保存されています）を表示した後、```sys.exit(1)``` を呼び出してBlenderが復帰値 ```1``` で終了します。Travis CIは、テストのコマンドの結果が ```0``` 以外の場合には、テストが失敗したと判断します。このためBlenderが復帰値 ```1``` で終了したとき、Travis CIはテストが失敗したと判断します。このように、アドオンの機能を実行したときの戻り値が期待したものではなかった場合に、Blenderが ```0``` 以外で終了するようなスクリプトを作成することで、アドオンが正常に動作しているか否かを確認することができます。

Blender実行開始時には、オブジェクト名が *Cube* であるオブジェクトが最初から作られているため、作成したアドオン ```testee.py``` で定義した全てのオペレーションは ```{'FINISHED'}``` で返ります。また、アドオンが正常に読み込まれていれば、```bpy.ops.object.test_ops_1``` や ```bpy.ops.object.test_ops_2``` は ```None``` 以外の値を持つことになるため、全ての ```assert``` 文の第1引数が ```True``` となり、テストが正常に終了します。


### .travis.ymlの作成

Travis CIでテストを実行するためには、```.travis.yml``` ファイルと呼ばれるYAML形式の設定ファイルを作成する必要があります。

[import](../../sample/src/chapter_04/sample_4_5/.travis.yml)

設定ファイルには、次に示す5つの項目を書く必要があります。```.travis.yml``` にもコメントを書いていますので、こちらも確認してみてください。なお、コメントで★を記載した部分は、テストを実行するBlenderのバージョンに応じて修正が必要な箇所です。

|項目|概要|
|---|---|
|```language```|Travis CIでテストするソースコードのプログラミング言語を指定します。BlenderのアドオンやスクリプトはPythonであるため、```python``` を指定します。|
|```python```|Pythonのバージョンを指定します。ここには、テストを実行するBlenderが採用しているPythonのバージョンを指定します。Blenderが採用するPythonのバージョンを調べる方法については、後述します。|
|```before_install```|```install``` に記述した処理の前に実行する処理を、Linuxのコマンドで記述します。Blenderを動作させるために必要なLinuxのパッケージを取得するため、パッケージマネージャを更新したあとに、パッケージマネージャからBlenderをインストールしています。パッケージマネージャでインストールしたBlenderのバージョンは、パッケージマネージャで管理されているバージョンであり、テストを実行するBlenderのバージョンと必ずしも一致しないことに注意が必要です。このため、パッケージマネージャでインストールしたBlenderはテスト向けに利用しません。|
|```install```|```script``` に記述した処理の前（テスト実行前）に実行する処理です。```before_install``` に記載する処理と同様、Linuxのコマンドで実行する処理を記述します。本節のサンプルでは、バージョンが2.75であるBlenderをダウンロードしたあと、アドオンをインストールします。|
|```script```|テスト実行時に実行する処理を、Linuxのコマンドで記述します。|

項目 ```script``` には、```--python``` と ```--addons``` 、```--background``` オプションを指定し、アドオン ```testee``` を有効化した上でテストスクリプト ```test.py``` をCUIモードで実行します。また、オーディオを無効化する ```-noaudio``` オプションや、初期状態で起動する ```--factory-startup``` オプションも指定します。```-noaudio``` オプションを指定しないで実行すると、オーディオライブラリ関連のエラーがログに出力され、ログが非常に見づらくなります。また、ダウンロード直後の状態では、Blenderは初期状態で起動するため、```--factory-startup``` は本来不要なオプションですが、念のために指定しています。


<div id="column"></div>

Blenderのコマンドラインオプションの一覧は、--helpオプションを指定してBlenderを実行することで表示することができます。


<div id="space_s"></div>


#### Blenderが採用しているPythonのバージョンを調べる方法

Blenderが採用しているPythonは、Blenderのバージョンごとに異なります。このため、テストを実行するBlenderが採用しているPythonのバージョンを調べる必要があります。Blenderが採用しているPythonのバージョンを調べるためには、次のスクリプトを *Pythonコンソール* で実行します。実行結果には、```major``` と ```minor``` 、```micro``` が出力されていますが、これらの値がBlenderが採用しているPythonのバージョンを示しています。```<major>.<minor>.<micro>``` が、Blenderが採用するPythonのバージョンとなります。例えば、次のスクリプト実行結果からは、バージョン2.75のBlenderでは、バージョン ```3.4.2``` のPythonが使われていることがわかります。

```python
>>> import sys
>>> sys.version_info
sys.version_info(major=3, minor=4, micro=2, releaselevel='final', serial=0)
```


### 作成したアドオンのソースコードをpush

ここまでに作成したファイルを、次のようにローカルリポジトリへ配置します。なお、事前にGitHub上のリモートリポジトリをローカルに持ってくる（cloneする）必要があることに注意してください。

```
/    # ローカルリポジトリのディレクトリ
├ .travis.yml   # Travis CIの設定ファイル
├ test.py       # テストスクリプト
└ testee.py     # テスト対象のアドオン
```

ローカルリポジトリへファイル一式を配置したら、GitHub上のリモートリポジトリに反映（push）するため、以下のコマンドを実行します。

```sh
$ cd Testing_Blender_Addon_With_Travis_CI   # ローカルリポジトリのディレクトリへ移動
$ git add .
$ git commit -m "Test add-on with Travis CI"    # ローカルリポジトリへ修正をコミット
$ git push origin master    # リモートリポジトリへ修正を反映（push）
```

リモートリポジトリにpushすると、Travis CIはpushされたことを検知し、```before_install``` 、```install``` 、```script``` に書かれた処理を順番に実行します。


### テスト結果を確認する

これでテストを実行することができましたが、テスト結果を確認してテストが正常に終了したことを確認し、テストしたアドオンに問題ないことを確認する必要があります。テストの実行結果は、Travis CIで次の方法で確認することができます。


<div id="process_title"></div>

##### Work

<div id="process"></div>

|<div id="box">1</div>|Travis CIのサイトの右上のユーザのアイコンをクリックして表示されるメニューから、*Accounts* をクリックしてアカウント情報を表示し、結果を確認したいリポジトリ名をクリックします。|![テスト結果の確認 手順1](https://dl.dropboxusercontent.com/s/eowughm4ekq90vv/check_test_result_1.png "テスト結果の確認 手順1")|
|---|---|---|

<div id="process_sep"></div>

---


<div id="process"></div>

|<div id="box">2</div>|テストが正常に終了した場合、#で始まるビルド番号の隣にpassedと表示されます。|![テスト結果の確認 手順2](https://dl.dropboxusercontent.com/s/8zpzlg5jz7gtesn/check_test_result_2.png "テスト結果の確認 手順2")|
|---|---|---|

<div id="process_sep"></div>

---


<div id="process"></div>

|<div id="box">3</div>|なお、*Job log* からテストの実行ログを確認することができ、```.travis.yml``` に記載した処理が行われていることを確認することができます。|![テスト結果の確認 手順3](https://dl.dropboxusercontent.com/s/162cqvaq0fqq9e4/check_test_result_3.png "テスト結果の確認 手順3")|
|---|---|---|



<div id="process_start_end"></div>

---


#### テスト失敗時の表示を確認する

テスト成功時について確認しましたが、テスト失敗時についてTravis CIでどのように表示されるかも確認したいと思います。ここでは、故意にテストを失敗させるために、```test.py``` を次のように書き換えます。変更した箇所について、コメントの先頭に ```$``` を追加しています。オブジェクト名が「Cube」であるオブジェクトを削除する処理を追加しました。オブジェクト名が「Cube」であるオブジェクトを削除し、```bpy.ops.object.test_ops_2()``` が ```{'CANCELLED'}``` を返すことで、テストが失敗するようにします。

[import](../../sample/src/chapter_04/sample_4_5/test_alt.py)

ソースコードを修正したあとにリモートのリポジトリに修正内容をpushし、テストを実行します。


<div id="sidebyside"></div>

|期待した通りテストが失敗し、#で始まるビルド番号の隣にfailedと表示されます。|![テスト失敗時の表示確認1](https://dl.dropboxusercontent.com/s/3cfuwl2hkmhnyd0/check_test_failed_result_1.png "テスト失敗時の表示確認1")|
|---|---|


<div id="sidebyside"></div>

|テストが失敗した原因を確認するためにログを見ると、test_ops_2が失敗していることがわかります。*Cube* を削除したことによって、オペレータクラス ```TestOps2``` の ```execute()``` が ```{'CANCELLED'}``` を返し、asset文で期待した ```{'FINISHED'}``` と一致しなかったために、テストが失敗しています。|![テスト失敗時の表示確認2](https://dl.dropboxusercontent.com/s/ljo7ty7uin68v58/check_test_failed_result_2.png "テスト失敗時の表示確認2")|
|---|---|


## まとめ

GitHubとTravis CIを利用し、アドオンのテストを自動化する方法を紹介しました。すでにGitHubでアドオンを公開している方や、これからGitHubでアドオンを公開することを考えている方は、アドオンの品質や開発効率の向上のために、本節で紹介したテスト自動化を検討してみる価値があります。GitHubにアドオンを公開しない方でも、BlenderをCUIモードで起動してテストスクリプトを実行することで、アドオンをテストすることができることを覚えておくとよいかもしれません。

本節で紹介したテスト自動化は、修正時に他の機能への影響が見えづらく、かつテスト量が多くなりがちな規模の大きいアドオンに対して効果を発揮します。一方、数百行程度の規模が小さいアドオンの場合は、テスト自動化のためにテストスクリプトや設定ファイル作成が必要となることから、かえって手間がかかってしまう可能性があります。また、今後のアドオンのサポートを考えていない場合など、あくまで個人用途で作成したアドオンであれば、テストを行う必要がないかもしれません。作成したアドオンの規模やサポート範囲をテスト自動化によるメリットと比べながら、アドオンの自動テスト化を行うかを判断するとよいと思います。


<div id="point"></div>

### ポイント

<div id="point_item"></div>

* アドオンのテストを自動化することで、ソースコードを修正したことによるバグの発生を早期に発見できる可能性がある
* コンソールウィンドウからBlenderを実行する時にオプション ```--background``` を指定することで、CUIモードでBlenderを実行することができる
* CUIモードでPythonスクリプトを実行するためには、```--python``` オプションの引数に実行するスクリプト名を指定する
* CUIモードでアドオンを有効化するためには、```--addons``` オプションの引数に有効化したいアドオンのパッケージ名またはモジュール名を指定する
* GitHubをTravis CIと連携することで、GitHubのリポジトリにpushした時に ```.travis.yml``` に記載した内容に従ってビルドやテストを行い、その結果をTravis CI上で確認することができる