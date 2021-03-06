---
pagetitle: 1-1. Blenderって何？アドオンって何？
subtitle: 1-1. Blenderって何？アドオンって何？
---

本書の読者でBlenderを知らない人はほとんどいないと思いますが、本節ではBlenderとその拡張機能であるアドオンについて簡単に説明します。
基本的な内容かつ、アドオン関する最低限の説明に絞って説明していますので、すでにBlenderを使っている人やすぐにアドオン開発に進みたい人は、本節を読み飛ばしても問題ありません。


# Blenderって何？

## Blenderはフリーで高機能な3DCG制作ソフト

Blenderはフリーソフトとして提供されている、3DCG制作のための総合環境です。

Blenderはもともとオランダで開発されていたレンダリングソフトでした。
現在はオープンソース化され、3Dモデリングからマテリアル設定・アニメーション制作など、3DCG制作に必要なほとんどの作業をBlenderだけで行うことができるようになっています。
さらに最近では、ゲームエンジン機能も強化されるなど、Blenderを用いてインタラクティブなコンテンツを制作できるようになってきています。
Blenderの開発は現在も活発であることから、今後の発展が非常に楽しみなソフトの1つであると言えます。

Blenderはマルチプラットフォームを前提に設計されているため、WindowsだけでなくMacやLinux上でも動作します。
つまりOSをWindowsからMacに変えても、Blenderを使う限りは3DCG製作ソフトの使い方を覚え直す必要がありません。

![](../../images/chapter_01/01_What_Is_Blender_What_Is_Add-on/blender_startup.png "Blender起動画面")

上の画像は、Blender（バージョン2.80）を起動した直後の画面です。


## Blenderの特徴

3DCG制作ソフトは、Blender以外にもあります。
有名な3DCG制作ソフトを次にいくつか挙げてみましたが、世の中にはより多くの3DCG制作ソフトがあります。

* 3ds Max (有料、学生版のみ無料)
* Maya (有料、学生版のみ無料)
* Shade 3D (有料)
* Sculptris (無料)
* ZBrush (有料)
* Metasequoia (一部有料)
* 六角大王super (有料)
* MODO 901 (有料)

3DCG制作ソフトにはそれぞれ長所・短所があり、ソフトごとに特徴があります。
Blenderはオープンソースソフトウェアで高機能なソフトであることが、大きな特徴の1つとなっています。
また、キーボードやマウスのキーマップを他のメジャーな3DCG制作ソフト（3ds MaxやMaya）が採用するキーマップに変更する機能を標準で持っていることも、特徴の1つでしょう。
キーマップを自由に変更したり削除したりすることもできるので、ユーザは自ら使いやすい制作環境を構築できます。


## Blenderの将来性

高機能化が進むBlenderは、商用レベルの機能を持つと言われることが多くなりましたが、商用の3DCG制作ソフトである3ds MaxやMaya等に比べたら機能的に劣っている部分が少なからずあります。
しかしBlenderはオープンソースソフトウェアであり、ライセンスを守りさえすれば、誰でもBlenderの開発に参加できるという強みがあります。
このため要望を出せば、商用の3DCG制作ソフトと比較して、機能を追加してもらえる可能性が高いのが特徴です。

またBlenderはPythonのスクリプトエンジンを内蔵し、Python向けのAPIをユーザに公開しているため、Blender本体のソースコードを読むことなく、提供されているAPIを使ってユーザが新機能を作成できます。


# アドオンって何？

BlenderはPythonのスクリプトエンジンを内蔵し、Python向けのAPIをユーザに公開しています。

アドオン（add-on）とは、これらBlenderが公開しているAPIを利用して作られた、Blenderの拡張機能のことを指します。
他の3DCG制作ソフトとBlenderとのコンバータや作業効率化ツールなど、Blender本体では提供されていない機能を持つアドオンが有志の間で作成されています。

<div class="column">
アドオンはプラグイン（plugin）やスクリプト（script）と呼ばれることもあります。  
プラグインとアドオンを同じような意味で使う人が多いようですが、元から存在するBlender本体に対して新たな機能を追加する（on＝プログラムの上に機能を載せる）ための小さなプログラムという意味ではアドオンのほうが正しいです。
また、Blender内では一貫してアドオンと呼んでいることから、本書でもアドオンと呼ぶことにします。
なおプラグインは、プログラム本体の機能を差し替える（in＝プログラムの中に入れて機能を差し替える）ためのアプリケーションコードのことを指すため、厳密にはアドオンとは異なります。
Blenderは、開発者に向けてBlender本体の動作自体を変えるための手順の規格化や公表を行っていないので、Blenderプラグインは存在しません。  
スクリプトは、ソースコードを記述したらすぐに実行できるプログラムのことを指すため、曖昧になりがちなアドオンとプラグインに比べて明らかに用語の意味が異なります。
</div>

![](../../images/chapter_01/01_What_Is_Blender_What_Is_Add-on/blender_add-on.png "Blenderアドオン")

画像は、Blenderのアドオンを有効/無効化するための画面です。

アドオンは、Blender本体を持っている人なら誰でもすぐに開発を始めることができます。
しかし、アドオン開発のチュートリアルが英語で書かれていることに加え、基本的なことから順を追って解説している資料がほとんどないためややハードルが高いのが現状です。

本書ではBlenderのインストールから初めて、自らアドオンを作成して使用することで、アドオンの開発方法を習得していきます。
本書で紹介するサンプルアドオンは必ず実行して、アドオンがどのように動作するか確認してください。
実際に手を動かすことでより理解が深まるはずです。
さらに一歩進んで、サンプルアドオンを改造できればより理解が進むと思います。


# まとめ

簡単ですが、Blenderとアドオンについて説明しました。
本節を読み終えた段階では、Blenderやアドオンについて理解できていれば十分です。

次節からはBlenderのアドオン開発に焦点を絞って説明していきます。
アドオン開発の解説に入るのは難しいかもしれないので、他者が作成したアドオンをインストールして動作確認することで、アドオンへの理解を深めていきましょう。


## ポイント

* Blenderはオープンソースソフトウェアの3DCG制作ソフトである
* Blenderは複数のプラットフォームで動作することを前提に作られているため、Windows/Mac/Linux上で動作する
* Blenderが提供するPython向けのAPIを利用することで、Blenderの拡張機能（アドオン）を作成できる
