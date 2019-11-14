##LayaAir 3 D骨格掛点

###骨格掛点の概要

骨格の挂け技术は3 Dゲームの中で非常に普遍的で、たとえば武器は役の手の动作に従って変化して、それでは私达は武器と手の骨格を挂けることができて结び付けることを行って、武器は手の骨格のサブクラスとして、自然と使いやすい动作と変化することができます。

もちろん、バインディングされた3 Dモデルは、コードによってバインディングを除去したり、別の3 Dモデルを交換したりすることもでき、このような方法で武器や装備の換装機能を実現することができる。



###ユニティに骨格掛点を設置する

骨格掛点はユニティに設置されていてとても便利です。シーンの資源階層で直接操作できます。以下の図（図1）

バインディングを必要とするオブジェクトは3 D容器でもいいです。3 Dモデルだけでもいいです。位置を調整した後、指定された骨格の下にドラッグしてサブレベルとして接着点を連結することに成功しました。動画を再生する時、骨格アニメーションに従って変化していることが分かります。

ある時、私達は最初の時に武器を持たない必要がありますが、またハングアップが必要です。今後武器を換えるために準備します。それなら私達は骨の下に空いているノードの容器GameObjectを入れてもいいです。必要な時には別の3 Dモデルや複数のモデルを追加します。

![图1](img/1.png)<br/>(図1)

**Tips：私たちの骨格の掛点が設置されたら、骨格と掛点の対象は自動的に.lsまたは.lhファイルに導かれます。get ChildByName()方法でそれらを入手できます。ただし、特に注意する必要があります。骨格のハングアップは空の容器オブジェクトだけを結びつけて、後で動的にサブオブジェクトを追加するために使用されます。エクスポートプラグインでは、GameObject SettingのIgnore Null Game Objectは空ノードの設定を無視しています。そうでないと、空容器のハンガーポイントオブジェクトは.lsまたは.lhに導かれません。**  



###コードの中で骨格を実現します。

一般的に、私たちはユニティに骨格掛点を追加します。しかし、LayaAirエンジンはコードのかけ方も提供しています。骨格のハンガーアップを柔軟に追加して除去することができます。

アニメイトアニメーションモジュール類は二つの実例的な方法を提供しています。**linkSprite 3 D ToAvatarNode()**を選択します**unlink Sprite 3 D ToAvatarNode（）**ハンガーポイントの追加と除去が可能です（図2、図3）。

Tips：コードは骨格動画を追加する前に、美術が骨格ノードに関連する名前を提供する必要があります。

![图2](img/2.png)<br/>(図2)

![图3](img/3.png)<br/>(図3)

具体的に使用するコードは以下の通りです。

シーンから骨格アニメーションモデルを取得します。模型のアニメーションコンポーネントを取得します。


```typescript

  //从场景中获取动画模型
  var monkey:Sprite3D=scene.getChildByName("monkey") as Sprite3D;
  //获取动画模型中动画组件
  var monkeyAni:Animator=monkey.getComponentByType(Animator) as Animator;

  //需要挂点的3D对象
  var box:MeshSprite3D=new MeshSprite3D(new BoxMesh(1,1,1));
  //将3D对象加载到scene中（一定要加入到场景）
  scene.addChild(box);
  //将挂点物品添加到某个骨骼上（美术提供骨骼的名称）
  monkeyAni.linkSprite3DToAvatarNode("RHand",box);

  //将挂点物品从骨骼上移除（美术提供骨骼的名称）
  //monkeyAni.unLinkSprite3DToAvatarNode("RHand",box);
```




###骨格掛点運用例

魔法攻撃の簡単な例をあげて、骨格掛点の運用を示します（図4）。

![图4](img/4.gif)<br/>(図4)

まず図1のように、ユニティに魔法絞りを右骨格のサブノード階層に設定し、右骨格の名前を「RHand」に変更し、魔法絞りを「weappon」として出力します。導出後、手骨格と絞りがモデルのサブレベルファイルに現れることがわかった（図5）。必要に応じて名前によって取得できる。

![图5](img/5.png)<br/>(図5)

図4の魔法攻撃効果によって、2つのカテゴリーで実現できます。一つは主にLaya 3 DuBonePoint.asで、アニメ放送と魔法兵器の生成を実現するためのものです。攻撃動画が36フレームぐらいまで再生された時、武器を掛けるのと同じ新しい魔法武具をクローンし、武器シナリオを追加して飛行に使います。また、シミュレーションで魔法が発生し、魔法をかける効果を再表示します。

ウェポンScript.asが魔法飛行と破壊を実現。すべてのコードは以下の通りです。


```typescript

package
{
	import laya.d3.component.Animator;
	import laya.d3.component.Script;
	import laya.d3.core.MeshSprite3D;
	import laya.d3.core.Sprite3D;
	import laya.d3.core.scene.Scene;
	import laya.display.Sprite;
	import laya.display.Stage;
	import laya.events.Event;
	import laya.utils.Handler;
	import laya.utils.Stat;

	public class Laya3D_BonePoint
	{
		public var scene:Scene;		
		/**角色动画组件**/	
		public var monkeyAni:Animator;
		/**骨骼挂点绑定的武器**/		
		public var weapon:Sprite3D;
		/**武器克隆**/	
		public var weaponClone:Sprite3D;
		/**武器是否已克隆**/
		private var weaponIsClone:Boolean=false; 		
		
		public function Laya3D_BonePoint()
		{
			//初始化引擎
			Laya3D.init(1280, 720,true);			
			//适配模式
			Laya.stage.scaleMode = Stage.SCALE_FULL;
			Laya.stage.screenMode = Stage.SCREEN_NONE;			
			//开启统计信息
			Stat.show();
			
			//加载3D资源
			Laya.loader.create("LayaScene_monkey/monkey.ls",Handler.create(this,onComplete));
		}
		
		//资源加载完成回调
		private function onComplete():void
		{
			//创建场景
			scene=Laya.loader.getRes("LayaScene_monkey/monkey.ls");
			Laya.stage.addChild(scene);
			
			//从场景中获取动画模型
			var monkey:Sprite3D=scene.getChildByName("monkey") as Sprite3D;
			//获取动画模型中动画组件
			this.monkeyAni=monkey.getComponentByType(Animator) as Animator;
			
			//获取挂点骨骼(Unity中设置的挂点骨胳会被导出，可获取)
			var handBip:Sprite3D=monkey.getChildByName("RHand") as Sprite3D;
			//获取挂点的武器模型
			this.weapon=handBip.getChildByName("weapon") as Sprite3D;
		 
			//监听动画完成事件
			this.monkeyAni.on(Event.COMPLETE,this,onAniComplete);
			
            //帧循环，用于监控动画播放的当前帧
			Laya.timer.frameLoop(1,this,onFrame);
		}
		
		private function onAniComplete():void
		{
			//动画播放完成后武器激活显示
			this.weapon.active=true;
			//动画播放完成后，设置为未克隆，方便下次克隆新武器
			this.weaponIsClone=false;
		}		
			
		//在攻击动画播放到一定帧时，克隆一个新武器特效
		private function onFrame():void
		{
			//在动画35-37帧之间时克隆一个飞出的武器
			//（不能用==35帧方式，帧率不满时可能跳帧，导致克隆失败。后期版本将支持帧标签事件，可解决此问题）
			if(this.monkeyAni.currentFrameIndex>=35&&this.monkeyAni.currentFrameIndex<=37)
			{
				//确保在35-37帧之间只克隆一次
				if(this.weaponIsClone) return;
				//克隆新武器（模型、位置、矩阵等全被克隆）
				var weaponClone:Sprite3D=Sprite3D.instantiate(this.weapon);
				//为武器特效添加脚本
				weaponClone.addComponent(WeaponScript);
				//将克隆武器放入场景中
				scene.addChild(weaponClone);				
				//设置为已克隆
				this.weaponIsClone=true;				
				//隐藏原始武器
				this.weapon.active=false;
			}
		}		
	}
}
```



```typescript

package
{
	import laya.d3.component.Script;
	import laya.d3.core.ComponentNode;
	import laya.d3.core.Sprite3D;
	import laya.d3.core.render.RenderState;
	import laya.d3.math.Vector3;
	
	/**
	 * 武器脚本(飞行与销毁)
	 */	
	public class WeaponScript extends Script
	{
		/**被脚本绑定的武器**/
		public var weapon:Sprite3D;
		/**武器生命周期**/
		public var lifeTime:int=100;
		
		public function WeaponScript()
		{
			super();
		}
		
		//获取绑定对象
		override public function _load(owner:ComponentNode):void
		{
			this.weapon=owner as Sprite3D;
		}
		
		//覆盖组件更新方法，实现武器帧循环
		override public function _update(state:RenderState):void 
		{
			//武器旋转更新
			weapon.transform.rotate(new Vector3(2,2,0),true,false);
			//武器移动更新
			weapon.transform.translate(new Vector3(0,0,0.2),false);
			//生命周期递减
			lifeTime--;
			if(lifeTime<0)
			{
				lifeTime=100;
				//直接销毁脚本绑定对象会报错（对象销毁后脚本还会更新一次，报找不到绑定对象错误），
                //因此延迟一帧以销毁
				Laya.timer.frameOnce(1,this,function(){weapon.destroy();});
			}
		}		
	}
}
```