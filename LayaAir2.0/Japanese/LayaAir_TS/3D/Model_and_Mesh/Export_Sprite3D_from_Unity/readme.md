# 从Unity中导出模型

###### *version :2.0.2beta   Update:2019-4-26 插件版本:2.0.2*

前の方にあります[Unity插件篇](http://localhost/LayaAir2_Auto/%E5%9C%B0%E5%9D%80)簡単にプラグインを使って精霊を導き出すことができます。ここでは精霊の導出について詳しく説明します。

*ここでは猿の模型を例として使っています。*

！[](img/1.png)<br/>(図1)

猿の模型のファイル構造を見てください。

！[](img/2 png)<br/>(図2)

選択`预设`をクリックします。`导出`猿の模型を導き出す。

！[](img/3 png)<br/>(図3)

エクスポート後のファイルディレクトリは下図のようになります。

！[](img/4 png)<br/>(図4)

####*.lhフォーマットデータファイル

`*.lh`エクスポートされた3 D表示対象容器Spirte 3 Dタイプのデータファイルは、JSON形式で符号化され、unity 3 DでlayaAir導出プラグインが選択的にエクスポートされます。**プリセット**カテゴリー生成、コンテンツは*.lsフォーマットよりも少なくなりました。他は全部同じです。

####*.lmフォーマットデータファイル

エクスポートしても**シーンファイル**または**プリセットファイル**タイプは、エクスポートされたリソースフォルダにシリーズ*.lm形式のファイルが含まれています。本プロジェクトでは、`LayaMonkey`フォルダはunityの開発者が作成したFBXモデルを格納するフォルダで、図4のように、エクスポート時に対応するフォルダと.lmリソースファイルが生成されます。

`*.lm`ファイルはモデルのグリッドデータファイルです。Mesh Sprite 3 DやSkinedMesh Sprite 3 Dタイプ表示オブジェクトのMesh（グリッド）を生成することができ、ファイルにはモデルグリッドの頂点位置、法線、頂点色、頂点UVなどの情報が含まれています。

####*.lavフォーマットデータファイル

`*.lav`ファイルはモンゴル骨格アニメーションのデータファイルです。SkinedMesh Sprite 3 D蒙皮格子精霊のAvatar（骨格）を生成することができます。

ファイルには骨格ノード情報が含まれています。

####*.lmatフォーマットデータファイル

`*.lmat`ファイルは材質データファイルです。モデルで使用できるmaterial（材質）を生成できます。材質スタンプ、材質ボールの色情報、定点色情報、テクスチャオフセット情報などの材質に関する情報が含まれています。

>より多くのリソースの種類は、リソースの概要編のリソースの種類の解説を見ることができます。[地址](https://ldc2.layabox.com/doc/?nav=zh-ts-4-3-0))