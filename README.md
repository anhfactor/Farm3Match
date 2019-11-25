## SurfingGame

===========
サーフィンゲームを作ります。

###【エディタの使い方】
#### ５つのビュー
Scene View
Game View
Hierarchy View
Project View
Inspector View

#### ４つのScene View 操作

###【Import Asset】
以下のパッケージをインポートします。<br/>
Assets → Import Packages → Water (Basic)　（Pro版の方は「Water (Pro Only)」の方が綺麗 )<br/>
Assets → Import Packages → Skyboxes<br/>
Assets → Import Packages → Particles して、Importします。<br/>

![](README_Resource/Assets_ImportPackages.png)

###【完成図】
CompleteGame をダブルクリックして、完成シーンを開きます。<br/>
![](README_Resource/CompleteGame_Scene.png)
<br/>
こんなゲームを作ります。<br/>

###【背景を作って雰囲気出し】
海を作ります。<br/>
「Standard Assets/Water (Basic)/Daylight Simple Water」をHierarchyにドラッグアンドドロップ（以下D&D）。Position(0,0,1000)、Scaleを(1600,1,1600)に<br/>
![](README_Resource/DaylightWater.png)

###【Skybox】
空を作ります。<br/>
Edit → Render Settings　Skybox Material に [Starndard Assets/Skyboxes/Sunny2 Skybox] をD&D<br/>
![](README_Resource/RenderSettings.png)
<br/>
プレイしてみましょう。<br/>
ためしに何かオブジェクトを移動させてみましょう。そして再度プレイボタンを押して止めてみましょう。<br/>
オブジェクトは最初に戻りました。プレイ中の変更は反映されない、ということです。これを知らないと、プレイ中に色々作業してしまったあげく、プレイを止めた途端元通り、ということになってしまいます。<br/>

###【プレイ中の変更は反映しない！】

Unity → Preferences... (Windowsは 「Edit → Preferences...」)　で　ColorsタブのPlaymode tintで色変更すると、プレイ中のEditor色が変わります。<br/>
![](README_Resource/PlaymodeTint.png)
<br/>
こうすると「プレイ中」ということが分かりやすくなるので、プレイ中に編集してしまうという間違いが減るということです。<br/>

###【シーンを保存】
ここらでFile → Save Scene でシーンファイルをセーブしましょう。名前は適当に「Game」とかにしましょうか。<br/>
![](README_Resource/SaveScene.png)

###【プレイヤー作成】
GameObject → Crete Other → Cube<br/>
![](README_Resource/GameObjectBoard.png)
<br/>
名前をBoardに<br/>
Position (0,0,0.04) Scale (1, 0.2, 4)<br/>

###【暗いのでライト】
GameObject → Crete Other → Directional Light<br/>
Shadow Type を Soft Shadowsに<br/>
![](README_Resource/DirectionalLight.png)

###【移動範囲を設定】
GameObject → Crete Other → Cube<br/>
Position (0,-1.5,1000) Scale (3000, 1, 3000)<br/>
名前をShallows（浅瀬という意味）にしましょう<br/>
Mesh Renderer を オフに（必要ないので）しておきます<br/>
![](README_Resource/GameObjectShallows.png)

###【親子関係】
このままだとボードが水面下に入ってしまうので、ボードは表面において、コリジョンは水面下でやりたいのです。<br/>
そういう場合は、コリジョン用別オブジェクトを作って、それの子供にすればいいのです。<br/>
GameObject → Crete Other → Sphere<br/>
Position (0,0,0)<br/>
Mesh Renderer を オフに（必要ないので）<br/>
名前をBoardBaseに<br/>
![](README_Resource/GameObjectBoardBase.png)
<br/>
Component → Physics → Rigidbody で物理挙動が付きます<br/>
Sherer Collider のRadius を 1 にします。Rigidbodyの方も、Constraintsの（もし開いていなければ▲をクリックして開きます）Freeze RotationのXYZを全てチェック入れて、回転はしないようにします。<br/>
![](README_Resource/Rigidbody.png)
<br/>

Hierarchy で 「Board」を「BoardBase」にD&Dして親子関係にする。こうすることで、親のBoardBaseが動けば、子供のBoardが動くということになる。<br/>
子供の「Board」のBoxColliderはいらないので、削除しましょう。InspectorのBox Colliderの右の小さい歯車アイコンをクリックして「Remove Component」を選択して削除<br/>
![](README_Resource/Parent.png)
<br/>


###【フォルダ作成】
ちょっとここらでファイル整理しましょう。<br/>
（既にGitHubからプロジェクトを持ってきている場合は、フォルダを作る必要ありません）
Projectビューで右クリック→ Create → Folder で名前を MyGame<br/>
もう一つその中に、Prefabsというフォルダを作ります（名前の意味はない）<br/>
同様に、Scripts、Materialsも作っておきます<br/>
![](README_Resource/Folder.png)
<br/>

###【マテリアル】
Materialsフォルダの中に入って、右クリック → Create → Material して新規マテリアルを作ります。<br/>
マテリアルとは素材、材質、ということ。<br/>
名前を「SideBarMat」とかにしておきます。<br/>
Main Color の色の部分をクリックして、色を赤に変更。<br/>
![](README_Resource/SideBarMat.png)
<br/>

###【Prefab】
GameObject → Crete Other → Cube<br/>
Position (-10,0,500) Scale (1, 0.2, 1000)<br/>
名前をSideBarに<br/>
SideBarを Hierarchy の SideBar をProjectの「MyGame/Prefabs」D&Dする。するとPrefabが作られます<br/>
![](README_Resource/Prefab.png)
<br/>
<br/>
今度はそのPrefabを使って、Hierarchy にD&Dして、Position のXを -10 → 10に変更します。（二つのSideBarになる）<br/>
![](README_Resource/Prefab2.png)
<br/>
<br/>
何がうれしいかというと、大元のPrefabを変更すると、全てのPrefabに適応されるということです。<br/>
<br/>
[MyGame/Prefabs/SideBar]を選択して、InspectorのMeshRendererのMaterialsの▲をクリックして開く。するとDefault-Diffuseになっています<br/>
先ほどの[MyGame/Materials/SideBarMat]をSideBarのInspectorビューの先ほどの「Default-Diffuse」の所に上書きするようにD&Dして、マテリアルを適応させます。<br/>
<br/>
すると、大元のPrefabを赤に変更したので、二つのSideBarが両方とも赤になりました。<br/>
![](README_Resource/Prefab3.png)
<br/>
つまり最初に適当に作っておいても、後でデザイン素材を修正することも容易だということです。<br/>

###【Camera】
カメラの位置を変更しましょう。Main Camera を選択。PositionのZ を 0 にします。<br/>
Hierarchyの BoardBase の上にD&Dして、子供します。<br/>
![](README_Resource/Camera.png)
<br/>

###【Legacy Particle】
①[Standard Assets/Particles/Water/Water Fountain] を Hierarchy に D&D。<br/>
②Position( 0, 0, 2.1 )　Rotation( 70, 0, 0 ) とか。<br/>
③Ellipsoid Particle EmitterのLocal Velocity の Y を 4 にしてちょっと弱く。<br/>
Random Velocity の X を 4 にしてちょっと拡散するようにします。<br/>
④BoardBaseの子供にしておきましょう。<br/>
![](README_Resource/LegacyParticle.png)
<br/>

###【スクリプトでプレイヤーの動きを】
①[MyGame/Scripts]の中で 右クリック→ Create → C# Scriptでスクリプトを作りましょう。<br/>
②最初に設定する名前が重要で、Class名になります。ここでは「BoardController」にしましょう。<br/>
![](README_Resource/Script1.png)
<br/>

###【スクリプトとComponentの関係】
①BoardControllerをHierarchyのBoardBaseにD&D<br/>
②Hierarchy の BoardBase を選択すると、Inspectorの下の方に「Board Controller (Script)」という項目があると思います。<br/>
<br/>
つまりこうすることで、BoardControllerスクリプトの機能を BoardBase に追加したことになりました。<br/>
<br/>
③BoardControllerをダブルクリックして、MonoDevelop を開きます。<br/>
![](README_Resource/Script2.png)

<br/>
①ためしに、６行目に、以下のプログラムを追加してみましょう。<br/>

 	public Vector3 moveSpeed = new Vector3(4,4,8);

「ファイル → 保存」でファイル保存してください。<br/>
②そうして、Unityに戻ります。すると、さっきのInspector が変化しているのが確認できます。<br/>
「Move Speed」という項目が追加されているのがわかるでしょうか。<br/>
こうやって Unity と スクリプトで連携しながらゲームを作って行くのが、Unityにおけるプログラミングです。<br/>
![](README_Resource/Script3.png)

###【Input】
次はUpdateの中に以下のプログラムを追加してみましょう。<br/>

		float xForce = 0.0f;
		xForce = Input.GetAxisRaw("Horizontal");

		// velocity setting
		Vector3 vel = this.rigidbody.velocity;
		vel.x = moveSpeed.x * xForce;
		vel.z = moveSpeed.z;
		
		this.rigidbody.velocity = vel;

すると、ちゃんと前に進みますか？<br/>
うまく行かなかった場合は BoardController_Ver1.cs の「// ここから」「// ここまで」をコピーして、該当のところにコピペしてください。<br/>
![](README_Resource/Script4.png)
<br/>

###【OnCollisionEnter】
地面に付いている判定をしましょう。<br/>
<br/>
先ほどのmoveSpeedの定義の下に以下の行を追加します。<br/>

	public bool isGround = false;

Update関数の 終了の中括弧閉じ「}」の後に以下のプログラムを追加します。<br/>

	void OnCollisionEnter( Collision col ) 
	{
		// check ground for a water effect
		if ( col.gameObject.CompareTag("Ground") ) isGround = true;
	}

ここの「Ground」というのは”タグ”という概念で条件分岐しています。<br/>
面倒な場合は BoardController_Ver2.cs の「// ここから」「// ここまで」をコピーして、該当のところにコピペしてください。
それを今から設定してみましょう。<br/>

###【Tag】
①メニューからEdit→Project Settings→Tags and Layersを選択すると、Tag & Layersビューが表示されます。<br/>
これのTagsに追加していきましょう。<br/>
②Element 0 の所に「Ground」という文字を代入します。（もう一つ空欄が増えますが気にしないでください）<br/>
これで「Ground」タグが増えました。<br/>
![](README_Resource/TagsLayers.png)
<br/>
<br/>
①HierarchyのShallowsを選択して、②Inspectorの一番上のTagをクリックすると、今度は「Ground」という先ほど追加した項目が増えているので、これを選択します。<br/>
これで、ShallowsオブジェクトがGroundタグが付きました。<br/>
![](README_Resource/TagsLayers2.png)


###【ジャンプ可能に】
「this.rigidbody.velocity = vel;」 の行の手前に<br/>

		// jump
		if ( Input.GetButton("Jump") && isGround ) {
			vel.y += moveSpeed.y;
			isGround = false;
		}

を追加します。こうすると、ジャンプできるようになります。<br/>
うまくいかなかった場合は BoardController_Ver3.cs の「// ここから」「// ここまで」をコピーして、該当のところにコピペしてください。
![](README_Resource/Script5.png)

###【ジャンプ時にエフェクトオフに】（時間次第で削除）
ジャンプ時にちゃんとエフェクトがでるのは変ですよね。<br/>
では、水エフェクトをオンオフするためのスクリプトを作りましょうか。<br/>
今度は違う方法でスクリプトを作成してみましょうか。<br/>
①Project ビューからではなく、Hierarchy の「Water Fountain」（BoardBaseの子供になっています）を選択します。<br/>
②Inspector の一番下の「Add Component」で、「New Script」を選びます。<br/>
③そして「WaterFx」、Laungageは「CSharp」にします。こうすることでスクリプトを作ると同時に、GameObjectにそのスクリプトを付加することもできるのです。<br/>
<br/>
![](README_Resource/Script6.png)
<br/>
では、スクリプトを編集して行きます。付けたWaterFxの右側の歯車アイコンをクリックして、一番下の「Edit Script」をクリックして、スクリプトファイルを開いてください。<br/>
<br/>
①「public class WaterFx : MonoBehaviour {」の行の下に<br/>

	public BoardController board;

を追加します。<br/>
<br/>
②Update関数の中括弧の間に<br/>

		if ( board.isGround && this.particleEmitter.emit == false ) {
			this.particleEmitter.emit = true;
		}
		if ( board.isGround == false && this.particleEmitter.emit ) {
			this.particleEmitter.emit = false;
		}

を追加します。「ファイル → 保存」をしてファイルを保存を忘れないでください。そしてUnityに戻ります。<br/>
![](README_Resource/Script7.png)
<br/>
面倒な場合はWaterFxSampleをコピペしてください。（その際は BoardController_Complete を BoardController に変更することをお忘れなく！）<br/>
<br/>
先ほどの「Water Fountain」を選択して、Inspectorの下の方をみると、「Water Fx (Script)」がありますが、「Board」の項目が「None (Board Controller)」となっています。<br/>
ここにBoard ControllerをD&Dします。<br/>
このままHierarchy の「BoardBase」を、先ほどの「None (Board Controller)」のところにD&Dします。うまくD&Dでくると、「None (Board Controller)」が「BoardBase (Board Controller)」となります。<br/>
![](README_Resource/Script8.png)
<br/>
<br/>
これで、ジャンプの際に水エフェクトが消えるようになったでしょうか。<br/>

###【障害物の配置】
では障害物を作って行きましょう。<br/>
Game Object → Create Other → Sphere<br/>
で球オブジェクトを作ります。名前はBallとかにしておきましょう。<br/>
<br/>
色も変えましょう。どうするんでしたっけ？<br/>
<br/>
そうです、マテリアルです。<br/>
<br/>
[MyGame/Materials] の中で、右クリック → Create → Material して新規マテリアルを作る。<br/>
名前を「BallMat」とかにしましょうか。Main Color の色の部分をクリックして、色を黄色とかに変更。（何でも良いです）<br/>
<br/>
そうして、このBallMatを Hierarchy のBallにD&Dで黄色マテリアルを適応させます。<br/>
<br/>
このBall、Prefab化しておきましょうか。どうするんでしたっけ？<br/>
Ball を ProjectのPrefabsにD&Dするんですよね。<br/>
<br/>
で、prefab化したら、今度はPrefabの方からD&Dして、どんどん新しい障害物を作って行きましょう。<br/>
スケール変えてもいいですね。<br/>
<br/>
これでプレイすると、どうでしょうか？ちゃんとぶつかりますか？<br/>
ちょっとボードがめり込みますね。<br/>
BoardBaseの「Spherer Collider」のCenter の 1.2 にしましょう。<br/>
<br/>
長いCubeでジャンプしないといけない障害物を作ってもいいですね。<br/>
Game Object → Create Other → Cube<br/>
ポジションは(0,0,40)ぐらい、スケールは(19,1,1)にして幅広にしましょう。<br/>
マテリアルはさっきのBallMatを適応させてしまいましょう。<br/>
<br/>
このままぶつかると、そこで止まってしまいます。摩擦が大きいんですね。<br/>
そこで新たにPhysic Material で物理挙動に関する設定を作りましょう。<br/>
Assets → Create → Physic Material <br/>
とやって、新しいPhysic Materialを作って、名前は「Obstacle」とでもしておきましょう。<br/>
そして、Dynamic Friction、Static Friction、を 0 にしましょう。そして、Friction Combine をMinimumにしておきましょう。平均だとぶつかった相手との相互作用になってしまうので<br/>
これでどうでしょう？うまく滑りましたか？<br/>

###【Update と Fixed Update】（時間次第で削除）
このゲームはこのままでもいいのですが、他オブジェクトも動いている場合はこのままだと不整合が起きてしまいます。<br/>
移動に関わる部分はFixed Updateに書くべきなんですね。なので、<br/>

	private float xForce;
	private bool isJump;

	void Update () {
		xForce = Input.GetAxisRaw("Horizontal");

		isJump = false;
		if ( Input.GetButton("Jump") && isGround ) isJump = true;
	}

	void FixedUpdate()
	{
		Vector3 vel = this.rigidbody.velocity;
		vel.x = moveSpeed.x * xForce;
		vel.z = moveSpeed.z;

		if ( isJump ) {
			vel.y += moveSpeed.y;
			isGround = false;
            isJump = false;
		}

		this.rigidbody.velocity = vel;
	}

というように修正してください。<br/>
<br/>
これで、入力と移動を分離できました。<br/>
うまくいかなかった場合は BoardController_Ver4.cs の「// ここから」「// ここまで」をコピーして、該当のところにコピペしてください。

###【傾くように】（時間次第で削除）
サーフィンボードが傾かないので味気ないですね。傾くようにしてみましょうか<br/>
最初の方の定義のところに<br/>

	public Transform boardObject;
	public Vector3 targetAngle;

を追加します。<br/>
Updateのところに以下の文を<br/>

		Vector3 nowAngle = boardObject.localRotation.eulerAngles;
		nowAngle.y = Mathf.LerpAngle( nowAngle.y, targetAngle.y * xForce, 0.1f );
		nowAngle.z = Mathf.LerpAngle( nowAngle.z, targetAngle.z * xForce, 0.1f );
		boardObject.localRotation = Quaternion.Euler(nowAngle);

追加しましょう。保存します。<br/>
Unityに戻って、「BoardBase」 の Board Controller の Board Object に子供の「Board」をD&Dします。<br/>
Target Angle には Yには20 を Zには-30を入れましょう。<br/>
<br/>
これでどうでしょうか？<br/>
ボードが傾くようになったでしょうか。<br/>
うまくいかなかった場合は BoardController_Ver5.cs の「// ここから」「// ここまで」をコピーして、該当のところにコピペしてください。

###【Oculus対応その１】
ではお待ちかねのOculus Riftに対応してみましょう。<br/>
まずはOculusVRのサイトで開発者登録をする必要があります。<br/>
１、①developer.oculusvr.com のサイトにアクセスします。 ②Registerをクリックします。
![](README_Resource/Oculus1.png)
<br/>
<br/>
２、必要な項目を登録します。<br/>
![](README_Resource/Oculus2.png)
<br/>
<br/>
３、メールを送ったよー的なメッセージが表示されます。<br/>
![](README_Resource/Oculus3.png)
<br/>
<br/>
４、OculusVRからメールが来ているので、リンクをクリックしてサイトに飛びます<br/>
![](README_Resource/Oculus4.png)
<br/>
<br/>
５、認証サイトに飛びます。Nextをクリックします<br/>
![](README_Resource/Oculus5.png)
<br/>
<br/>
６、プロジェクト名とJapanを設定して、Create Projectをクリックします。<br/>
![](README_Resource/Oculus6.png)
<br/>
<br/>
これで開発者登録が完了しました。<br/>
<br/>
<br/>
７、①Loginをクリックし、②ユーザー名、パスワードを入力し、③Loginボタンをクリックします<br/>
![](README_Resource/Oculus7.png)
<br/>
<br/>
８、右側の「Latest Builds」のリンクをクリックします。（今回は0.2.5cを使います）<br/>
![](README_Resource/Oculus8.png)
<br/>
<br/>
９、「Unity 4 Pro Integration」のダウンロードボタンをクリックします。<br/>
![](README_Resource/Oculus9.png)
<br/>
<br/>
Oculus Rift SDKをダウンロードしてきて、そのパッケージの中にある「OculusUnityIntegration.unitypackage」をImportします。<br/>
Assets → Import Package → Custom Package... で、[ovr_unity/OculusUnityIntegration/OculusUnityIntegration.unitypackage] を選択してOpenします。<br/>
続けて、Importボタンを押します。<br/>
<br/>
[OVR/Prefabs/OVRCameraController]をHierarchyにD&Dします。そして、位置を調整して、（Ｙをちょっと1mぐらい上げて）BoardBaseの子供にします。（Main Cameraと同じ）<br/>
で、Main Cameraはこの場合いらないので、オフにしておきましょう。Inspectorの一番左上の項目のチェックをはずすだけで使っていないことになります。<br/>
<br/>
はい、Oculusで見ると、立体に見えるでしょうか？簡単ですよね？<br/>

###【Oculus対応その２】（時間次第で削除）
今回は傾きでコントロールできるようにしましょう。ジャンプは加速度で判定します。<br/>
そして、OculusJump.cs というスクリプトをBoardBaseに追加します。<br/>
そして34,35行目のコメントしている部分<br/>

	//		if ( OVRDevice.IsHMDPresent() == false ) return;
	//		OVRDevice.GetAcceleration(0, ref x, ref y, ref z);

を<br/>

 		if ( OVRDevice.IsHMDPresent() == false ) return;
 		OVRDevice.GetAcceleration(0, ref x, ref y, ref z);

というようにコメント解除。<br/>
<br/>
<br/>
BoardControllerの定義のところを<br/>

	public bool isOculus = true;
	private OculusJump jumpDetector;
	public float angleLimit = 45.0f;

を追加します。<br/>
Start関数に<br/>

		if ( isOculus ) {
			jumpDetector = GetComponent<OculusJump>();
		}

を追加します。<br/>
<br/>
Update関数の<br/>

		xForce = Input.GetAxisRaw("Horizontal");

という行を以下のように変更して、<br/>

		if ( isOculus )  {
			Vector3 angles = jumpDetector.riftCam.transform.rotation.eulerAngles;
			float rad = Mathf.Clamp( Mathf.DeltaAngle( angles.z, 0.0f ), -angleLimit, angleLimit ) / angleLimit;
			xForce = rad;
		} else {	
			xForce = Input.GetAxisRaw("Horizontal");
		}

に、<br/>
<br/>
同様に<br/>

		isJump = false;
		if ( Input.GetButton("Jump") && isGround ) isJump = true;

という行を以下のように変更します。<br/>

		isJump = false;
		if ( isOculus )  {
			if ( jumpDetector.isJump && isGround ) isJump = true;
		} else {	
			if ( Input.GetButton("Jump") && isGround ) isJump = true;
		}

これでOculus対応が出来たと思います。<br/>
完成版のBoardController は BoardController_Complete.cs を参照してください。
