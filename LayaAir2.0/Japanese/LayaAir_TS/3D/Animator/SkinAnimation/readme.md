# 骨骼动画的使用

###### *version :2.1.0beta   Update:2019-6-13*

骨格のアニメーションはまた蒙皮のアニメーションと呼ばれて、このようなアニメーションは主に模型の頂点を変える方式でアニメーションを生みます。骨格アニメーションも一番多く使われています。例えば猿の模型は骨格アニメーションです。

猿の模型を使って例を挙げます。

まず事前に用意した猿の模型を見てみます。

！[](img/1.png)<br/>(図1)

その後、アニメーションコントローラを作成し、Take 001のアニメーションを追加します。

！[](img/2 png)<br/>(図2)

猿の模型にアニメイトのセットを追加し、あらかじめ用意していたアニメコントローラとメッシュをアニメイトに追加します。同図3に示す

！[](img/3 png)<br/>(図3)

上記の準備ができたら、プレビュー動画を選択して、問題がないことを確認したら動画をエクスポートできます。ここではシーン全体を一緒にエクスポートする方法を選択します。エクスポートオプションのシーンオプションを選択し、エクスポートボタンをクリックしてシーンをエクスポートします。

！[](img/4 png)<br/>(図4)

エクスポートパネルの詳細については、表示できます。**Unityプラグインの使用**編です。

>**エクスポート前に注意が必要です。**

！[](img/5 png)<br/>(図4)

Animation TypeはGeneranicタイプのみ対応しています。

Optimze Game Objectはチェックできません。

------

シーンをエクスポートした後、動画効果をロードします。


```typescript

//加载我们导出的场景
Laya.Scene3D.load("res/LayaScene_LayaMonkey/Conventional/LayaMonkey.ls",Laya.Handler.create(this,function(s){
	Laya.stage.addChild(s);
}));
```


！[](img/6.gif)<br/>(図6)