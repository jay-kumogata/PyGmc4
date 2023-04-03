## PyxelによるGMC-4エミュレータ開発記

### 2009-12-11

GMC-4命令セットを調べ始めました．
Haskellの練習をしてみようかと考えています．

### 2009-12-12

命令セットのデータ構造(Data Construction)を定義してみました．
Haskellは，いろんなことを定義していけばいいだけなので，実は簡単なのかもしれません．
どうやるかは，書かなくてもよいです．
どう定義されるかだけ書けば，あとは勝手に動いてくれます．

_(2023-03-18 この期間は，仕事が忙しくて，趣味でプログラムは全くできてないです．
一行も書いてないです．
なんか色々と勉強とかをしてた期間です．まあ役立たなかったのですが．)_

### 2017-06-17

人生でやり残しを減らそうと思います．
また書き始めました．
今更感が満載であまり気が乗りません．

### 2017-10-14

昨日は神降臨日でした．
とてもやる気が出て，進捗しました．

- CPUデータ構造
- CPU初期化(initCPU)
- CPU表示(Show CPU)
- メモリ読み書き(memRead, memWrite)

_(2023-03-18 この後に，Haskellで書くのが面倒になり，Pythonで書き直してしまいました．
Haskellでは大して書いてなかったので，すぐに書きおなせました．)_

### 2023-01-15

同時に作っているPyxelChip8を参考にして，System.pyを一通り作成しました．
メモリ読込も実装されていなかったので，CPU_Read(),Write()を実装しました．
適当なサンプルを作って，TIA命令の動作を確認しました．

	Opコード		ニーモニック	内容			Flag
	8		TIA □ 		□→Ar			1

_(2023/03/18 この時点では，CPU.pyだけしかなく，算術演算が実装されている程度でした．動作チェックはされていませんでした．)_

### 2023-01-22

引き続きで，一通り命令の動作をチェックしました．

	Opコード		ニーモニック 	内容			Flag
	2		CH 		Ar⇔Br , Yr⇔Zr		1
	3		CY 		Ar⇔Yr			1
	4		AM 		Ar→(Yr) 		1
	5		MA 		(Yr)→Ar 		1
	6		M+ 		Ar+(Yr)→Ar 		桁上がり
	7		M- 		Ar-(Yr)→Ar 		マイナス
	9		AIA □ 		Ar+□→Ar			桁上がり
	A		TIY □ 		□→Yr			1
	B		AIY □ 		Yr+□→Yr			桁上がり
	C		CIA □ 		Ar≠□→Flag		不一致
	D		CIY □ 		Yr≠□→Flag		不一致
	F		JUMP □□ 	if(Flag)goto□□		1

### 2023-03-06

PyxelChip8の方がひと段落したので，LED命令関連の命令をまとめて実装しました．

	Opコード		ニーモニック 	内容				Flag
	1		AO 		Ar→Op				1
	E0 		CAL RSTO 	0→数字LED			1
	E1 		CAL SETR 	1→2進LED[Y]			1
	E2 		CAL RSTR 	0→2進LED[Y]			1
	ED 		CAL DSPR 	(E)→2進LED[0:3],(F)→2進LED[4:6]	1

### 2023-03-16

CAL命令の続きを実装しました．
サウンド命令は，ダミーで実装しました．
ウェイト命令も，実装しました．

	Opコード		ニーモニック 	内容			Flag
	E7 		CAL ENDS 	エンド音			1
	E8 		CAL ERRS 	エラー音			1
	E9 		CAL SHTS 	ショート音		1
	EA 		CAL LONS 	ロング音			1
	EB 		CAL SUND 	Arの音階の音		1
	EC 		CAL TIMR 	(Ar+1)×0.1sec待ち	1

ジャンプ命令は，実装誤りを修正しました．

	Opコード		ニーモニック 	内容			Flag
	F 		JUMP □□		if(Flag)goto□□		1

LEDが左右に移動するサンプルが動作しました．

### 2023-03-17

Pyxelで，LEDを表示してみました．
Twitterに投稿しました．
fxpファイルを読み込むように変更しました．
ソースコードだけを公開することが可能になりました．
少し整理して，出しておきます．

### 2023-03-18

2009年に，haskellでGMC4エミュを作りはじめて，2017年に，あきらめて，pythonで作りはじめて，放置してたのが出てきた．LED命令位まではうごいているっぽい．仕方ないから，pyxelでLED表示させてみようかな．と言って，LEDのドット絵を描きはじめました．

『昔のフォルダを整理していたら』シリーズの第4弾です．LEDのドット絵を作って，少しPython書きました．gmc4cc (C Compiler for GMC-4)さんの[LEDが左右に動くデモ](http://terus.jp/engineering/gmc4cc/compile.html?autoload=files/samples/pendulum.c)を動かしてみました．

![](https://github.com/jay-kumogata/PyGmc4/blob/main/screenshots/led01.gif)

### 2023-03-19

v0.2開発を開始しました．
『昔のフォルダを整理していたら』シリーズの第4弾です．
2009年にHaskellで書き始めたGMC-4エミュですが，途中Pythonへの移植を経て，Pyxelで基板のドット絵を表示してみました．
gmc4cc(C Compiler for GMC-4)さんの[LEDが左右に動くデモ](http://terus.jp/engineering/gmc4cc/compile.html?autoload=files/samples/pendulum.c)を動かしてみました．

![](https://github.com/jay-kumogata/PyGmc4/blob/main/screenshots/led02.gif)

Pyxelで動くGmc4エミュレータ🕹️（PyGmc4）の[ソースコード🗄️](https://github.com/jay-kumogata/PyGmc4/)を公開しました．

### 2023-03-20

v0.3開発を開始しました．
7セグメントLEDを表示するロジックを実装しました．
0からFまでの数字と小数点が正しく表示されるかチェックしました．
以前書いた箇所が間違っていたので，修正しました．

### 2023-03-21

基板のドット絵に，本物と同じような薄緑色の配線を書き加えました．
散歩してから，キー入力処理を実装しました．

	Opコード		ニーモニック 	内容		Flag
	0		KA 		K→Ar 		キー入力有無

フラグ処理を勘違いして実装していました．
キー入力がある場合には，フラグ=1ではなく，フラグ=0という[仕様](https://ja.wikipedia.org/wiki/GMC-4)でした．
その不具合を修正したところ，1,2,3のキーを押していき，合計が9を越えた人の負けという
[9を越えたら負けゲーム](http://terus.jp/engineering/gmc4cc/compile.html?autoload=files/samples/sum-game.c)
が動作しました．
ソースコードにマジックナンバーが多数存在していたので，定数を定義して置き換えました．

### 2023-03-22

少し基板のドット絵を修正しました．
LEDの表示順が逆になっていました．
左→右から右→左に変更しました．

### 2023-03-23

少し基板のドット絵を修正しました．
GitHubの方に，ソースコードを公開しました．

![](https://github.com/jay-kumogata/PyGmc4/blob/main/screenshots/nine01.gif)

『昔のフォルダを整理していたら』シリーズの第4弾です．
Pyxelで動作するGMC-4エミュの続きです．
基板のドット絵を少し修正して，キー入力処理を追加しました．
gmc4ccさんの[9を越えたら負けゲーム](http://terus.jp/engineering/gmc4cc/compile.html?autoload=files/samples/sum-game.c)が動きました．

### 2023-03-24

v0.4開発を開始しました．
GMC-4の模擬器作成は，楽しいです．週末は，サウンド追加をしようかと考えています．

### 2023-03-25

ウェイト処理で動作が遅くなっていたので，ウェイト値を調整しました．

![](https://github.com/jay-kumogata/PyGmc4/blob/main/screenshots/led03.gif)

サウンド命令以外の命令を実装しました．

	Opコード		ニーモニック 	内容			Flag
	E4		CAL CMPL 	NOT(Ar)→Ar		1
	E5		CAL CHNG 	A,B,Y,Z⇔A',B',Y',Z'	1
	E6		CAL SIFT 	Ar%2→Flag,Ar/2→Ar	Ar[0]
	EE		CAL DEM-	DEC((Y)-Ar)→(Y),Y-- 	1
	EF		CAL DEM+	DEC((Y)+Ar)→(Y),Y-- 	1

### 2023-03-26

本日も雨天でした．
v0.5開発を開始しました．
サウンド命令は，単に音階を再生するだけの簡易実装をしてみました．

	Opコード		ニーモニック 	内容			Flag
	E7	 	CAL ENDS 	エンド音 		1
	E8 		CAL ERRS 	エラー音 		1
	E9 		CAL SHTS 	ショート音 		1
	EA 		CAL LONS 	ロング音 		1
	EB 		CAL SUND 	Arの音階の音 		1


[Nibbled XEVIOUS](http://terus.jp/engineering/gmc4cc/compile.html?autoload=files/samples/nibbled-xevious.c)を動かしてみました．
実機は4MHz動作なので，実行速度をそれに合うように直しました．
サウンドが再生されるように修正しました．
2進LED No.3が正しく表示されない事象は，CAL DSPR命令の実装誤りでした．
2進LED No.3の動作が，オリジナルコメントと異なるのですが，理由は不明です．
動作したということで，次に進むことにします．

![](https://github.com/jay-kumogata/PyGmc4/blob/main/screenshots/xevious01.gif)

一通り動作したので，開発終了にします．

### 2023-03-29

Pyxelで動くGMC-4エミュレータ🕹️（PyGmc4）の[専用リポジトリ](https://github.com/jay-kumogata/PyGmc4)🗄️を作成しました．

### 2023-03-30

v0.6開発を開始しました．
キーボードが押された時に，そのキーが光るアニメーションを追加しました．
[押したキーが数が加算されるコード](http://terus.jp/engineering/gmc4cc/compile.html)を動かしてみました．

![](https://github.com/jay-kumogata/PyGmc4/blob/main/screenshots/sample01.gif)

### 2023-04-02

基板のドット絵を描きました．
GMC-4ロゴを修正したのと，LEDの色味を変更しています．

![](https://github.com/jay-kumogata/PyGmc4/blob/main/screenshots/led04.gif)

以上
