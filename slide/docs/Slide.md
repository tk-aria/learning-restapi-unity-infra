---
marp: true
theme: uncover-custom
header: "**VRプロフェッショナルアカデミー @VRエキスパートコース**"
footer: "by **@k-tanaka (twitter@tk_3118 github: tk-aria)**"
---
<!--
_backgroundColor: black
_color: white
headindDivider: 1
-->

# Unityアプリとサーバ連携について

---

## 事前準備に関して

- `Unity2021.3.2f1` のインストール

これより新しい分には基本的に問題ありません
該当バージョンよりも古い場合は一部機能が更新されているため
ハンズオン内容と食い違う部分があります

※ 音声再生などをする為、会場の方はイヤホン持参推奨です。

<!--
- Git
-->

<!--
- Docker
- docker-compose
-->

<!---
## プロジェクトダウンロードについて

gitの方はこちら

```
git clone git@github.com:tk-aria/vracademy-serverintegration.git
```

ghqの方はこちら
```
ghq get tk-aria/vracademy-serverintegration
```

それ以外の方はzipをdownloadします。

※教材調整の為、当日に公開させていただきます。

---
<!--
_backgroundColor: black
-->

---

<!--
zip


そもそも

#

---

# topic

---
-->

### 自己紹介

---

## 自己紹介

<!--![bg left:35% contain](https://www.goodfind.jp/common/img/space/detail/banner_pc_539.jpg)-->

***Kazuki Tanaka (twitter@tk_3118 github: tk-aria)***

- 傭兵 (フリーランスエンジニア)
  - ex) Server, Infra, Multimedia, Game, XR Engineer
- 基本的にエンタメの会社に出没する
- FFIしやすい言語が好き
- モノづくりが好きだが低めのレイヤーで遊んでることが多い

---

## 略歴

<!--![bg left:35% contain](https://www.goodfind.jp/common/img/space/detail/banner_pc_539.jpg)-->

- **XRスタートアップ(技術検証・新規B向けアプリ開発)**
- **大手スマホゲーム会社(長期タイトルの運用)**
- **ライブ配信アプリ開発/SDK開発**
- **通信事業社でSMS/RCSの配信システム開発**
- 現在 => REALITY株式会社でサーバ/インフラ/unity/(native少々)

他には技術的なアドバイスや支援を色んな方にしてたりします.

---
## 本講義の目的


**サーバに関して知る**
**サーバと通信が行えるようになる**

---
## 本講義の目的

実務レベルにスケールすると
↓

**サーバ担当者と連携してサーバ機能の組み込みが行えるようになる**

---

## アジェンダ

- サーバの実用例
- サーバについて知る
- Unityでhttp通信を行う方法について
- ハンズオン

<!-- ※ ハンズオン内でも逐次ご説明させていただきます。-->

---
<!--
_backgroundColor: orange
paginate: true
-->

# サーバの実用例

---
# サーバがあるとこんなものが作れます

---

<!--
_backgroundColor: black
-->

![height:600px](https://www.holynight.net/wp-content/uploads/2020/10/VRChat_1920x1080_2020-10-04_08-01-41.305.jpg.webp)


---
<!--
_backgroundColor: black
-->

![height:600px](https://img.redbull.com/images/c_crop,w_1613,h_1075,x_239,y_0,f_auto,q_auto/c_scale,w_1500/redbullcom/2020/5/13/i1pyqdapxumjllhfjoco/duos-apex-legends)

---
<!--
_backgroundColor: black
-->

![height:600px](https://imag.malavida.com/mvimgbig/download-fs/twitter-12580-2.jpg)

---
<!--
_backgroundColor: black
-->

![height:600px](https://i.ytimg.com/vi/668cIbMGoHI/maxresdefault.jpg)
<!--![height:600px](https://pbs.twimg.com/media/EwAZsqJUYAIsj0O.jpg)-->

---

### どういう部分でサーバと連携しているか
### イメージついたでしょうか？

---
# 実際の用途

---

## 実際の用途

### SNS

- ユーザー管理
- コンテンツの投稿
- コンテンツの配信
etc...

---

## 実際の用途

### ゲーム

- ユーザー管理
- 通信対戦
- ダウンロードコンテンツ
- 課金
- ガチャ
etc...(様々なデータ)

---

このほかにもサーバと連携すると...
- ユーザーログの取得
- クラッシュ通知
- 他サービスとの連携が可能

=> 運用面でも大きなアドバンテージになる

---

### 先程の例を出すと...

- VRChat(ユーザー管理、位置同期、コンテンツ)
- ApexLegends(ユーザー管理、マッチング, PvP、コンテンツ)
- Twitter(ユーザー管理、ツイートの投稿・表示)
- ウマ娘(ユーザー管理、ガチャ、課金、コンテンツ)

※ 実際の処理と異なる可能性はありますが、
こういった用途でサーバと連携しています。

---
## 結論

このようにサーバがないとアプリケーション開発が難しいケースが多い
基本的にサービスを展開するとなるとこの辺りの知識が必要になる。

---
<!--
_backgroundColor: orange
paginate: true
-->

# サーバについて知る

---


# サーバって何？

---

# サーバって何？

サーバとは、サービスを利用者のリクエストに応じて提供するソフトウェア、あるいはその機能が稼働しているコンピュータのこと
クライアントからの要求に対して情報や処理結果を提供する機能を果たす側(wikipedia引用)

---

## サーバって何？

### サーバーとは

ここでは、アプリケーション（サービス）を動作させるに当たって必要になるアプリケーションとします。

様々なプロトコルを通してネットワークを介しデータのやり取りを行う。

---
## サーバって何？
### クライアントとは

ユーザが直接使用するアプリケーションのこと

<!---
### 何してるの？なんで必要？

各ユーザー間でデータを共有したり、データを配信したりするのに必要。
-->
---
## アプリケーションの範囲


ネイティブアプリケーション, Webアプリケーション, Game,
VR/AR(XR), Webサーバ, ファイルサーバ, メールサーバ,
DNSサーバ, プロキシサーバ, データベース,
配信サーバ(エンコーダ)VPNサーバ, 認証サーバ,
PVPサーバ, アセットサーバ、Gitサーバ


---
## アプリケーションの範囲

- クライアント

```
ネイティブアプリケーション, Webアプリケーション, Game,
VR/AR(XR)
```

- サーバ

```
Webサーバ, ファイルサーバ, メールサーバ,
DNSサーバ, プロキシサーバ, データベース, 配信サーバ(エンコーダ)
VPNサーバ, 認証サーバ, PVPサーバ, アセットサーバ、Gitサーバ
```

---
## サーバの種類

沢山ある

- Webサーバ
- ファイルサーバ
- メールサーバ
- DNSサーバ
- プロキシサーバ
- SQLサーバ
- 配信サーバ(エンコーダ)
- VPNサーバ
etc...

---
## サーバの種類


用途によって多岐にわたる
実際の開発でもサーバが１台や１種類ということはなく
複数種類、台数展開されていたりします。

<!--
---

# サーバの仕組みを知る

---
## サーバの仕組みを知る

仕組みを少しでも知っておくことでサーバサイドエンジニアと連携する際にコミュニケーションがとりやすくなる

---


# サーバ見たことありますか？

---
## サーバ見たことありますか？

サーバもアプリケーションなので動かしているマシンが必ずあります。

---

#### じゃあどこにあるのか？

---

#### *A. 社内のサーバ室やデータセンターなどに置いてある*

---
## サーバ室とは？

![height:450px](https://www.mogiten-rentaru.com/spotcooler/blog/wp-content/uploads/2020/10/IMG_8675-scaled.jpg)

---
## サーバ室とは？

会社によって規模が違うが、
写真のようにPCが置いてあり基本的に常時冷却装置が稼働している。

---
## データセンターとは？

![height:450px](https://www.goodfind.jp/common/img/space/detail/banner_pc_539.jpg)

---
## データセンターとは？

サーバーを設置する物理的な施設のこと
サーバーを安定的に稼働させるための電力やネットワークをはじめとして
データを守る為、様々な部分で安全な環境が構築されている。
緊急時において日本の中でも最も安全な場所とのこと。

---
## データセンターとは？

また流行りのクラウドもAmazonやGoogleなど各社が
世界中のデータセンターに設置しているサーバにアクセスしています。

---
## 通信速度のはなし

ここまでの話でサーバはデータセンターなどにあり
そのサーバ同士は直接優先で繋がっているものもあります。
基本的にはファイバーなどの物理線で全て繋がっていて
最終的にLANを介してネットワークにつながります。

---
## 通信速度のはなし

通信速度を早くするには、
データの伝送距離を短縮することが望ましいです。
結果通信されるであろう顧客に対して
近い場所を選んでサーバを置くと良いです。
(厳密には少し違います)

---
## 通信速度のはなし

クライアント側で出来る通信速度の対応的には、
並列で通信を行うことなどになりますが、
前述した物理的要因とサーバの実装、クライアントの実装と
要因が広いので知識として知っておくと引き出しになるかと思います

---

## サーバの構成

- オンプレ(on premises)
- クラウド

---

## オンプレ

会社でサーバをもっている
物理PCをデータセンターまたは社内のサーバ室などにおいている

---

## クラウド
最近主流なのはこっち
有名なのはgoogle, amazonのGCPとAWS
他にはmicrosoftのazure、oracle cloudなど沢山ある
モバイルがターゲットだとmBaaSとしてFirebaseもよく使われる
オンプレからクラウドの移行業務などがあったりする

---

## クラウドについて

- VM(仮想マシン)
- サーバレス

---

## VM(仮想マシン)

レンタルサーバなど昔から普及しているよくあるパターン
例として...

- GCE
- AmazonECS
- OracleVMInstance
- さくらレンタルサーバ

---

## サーバレス

近ごろになってよく耳にするようになった
構築が用意で管理面などの面倒も見てくれるので
よく使われるようになった。

- Google AppEngine
- Google CloudRun
- AWS Lambda
- CloudFunctions
- Heroku
etc...
-->

<!---


これよりも上の話は、
インフラ構成やソフトウェアの話になり共通事項にならない為、
説明は省略させていただきます。
-->

---

## まとめ

用途によってさまざまなサーバが存在する
構築や設計もさまざまなので、これらを念頭にサーバサイドエンジニアと連携すると幸せになれる

---

さまざまなサーバがあり、
それによってアプリケーション連携の方法も多用ですが
最も一般的なアプリケーションサーバと連携してみる


---
<!--
_backgroundColor: orange
paginate: true
-->

# Unityでhttp通信を行う方法について


---
## プロジェクトの準備

配布済みのunitypackageファイルをimportします

<!--
manifest.jsonに以下のモノを追加
(今回使用する追加パッケージです)

```
{
    "dependencies": {
        "com.tk-aria.unitask": "https://github.com/tk-aria/UniTask.git
        ?path=src/UniTask/Assets/Plugins/UniTask#master",
        ...
```
-->


---

### 準備中に説明していきます。

---
## アプリケーションサーバとは

実際に運用されているアプリケーションのほとんどにこのサーバが関わっている。
ユーザーデータのやり取りやコンテンツの配信など
アプリケーションを開発する上で必要な不可欠な要素です。

---

## アプリケーションサーバとは？

Webサーバの一種
というか基本的にはwebサーバ
用途が違うためこのように呼んだりすることがある。

---

## アプリケーションサーバとは？

基本的にはhttpを用いて通信する
使用するのはhttp1.1でhttp2やhttp3ではない
(制約でhttp2のプロトコルが対応してなかったりすることもあるのでhttp1.1がなんだかんだ無難)

---

## アプリケーションサーバとの通信手法

- REST API(最もポピュラーな手法)
- GraphQL(最近出てきた)

---
## REST APIについて

'**REpresentational State Transfer**' の略

***リクエストの種類***

- Get
- Post
- Put
- Delete

上記の種類がある(あとで出てきます)

---
## REST APIについて

基本的にGETとPOSTしか使わないケースがほとんど
プロジェクトによってはPOSTのみもあったりする。

---

Unityでhttp通信する最も一般的な手法として
UnityWebRequestがある
今回はこれとUniTaskを用いてハンズオンを行います。

---


<!--さて-->
http通信を行うにあたって必要になる知識として
非同期処理があります。

---

### 非同期とは

一般的に、ある処理を実行中でも処理待ちをすることなく、他の処理を実行することができる仕組みのこと

=> 要は大きいデータのやり取り（ファイルのダウンロードなど）でも処理待ちを気にすることなく処理できる仕組みのこと

---

Unityで非同期を取り扱う方法として以下の選択肢がある

- coroutine
- async/await

---

### Coroutine

元々は手続き的な処理をリストにして順番に処理して行く用途として存在している
これを用いて非同期用途で使ってきたのが.net3.5時代のUnity
.net4.xを選択できるようになり
C#5で追加されたasync/awaitが使えるようになり最近はこちらに移行する流れ

---
### async/await

C#で非同期を扱う本来の機能
try/catchできたり
Coroutine時代に存在した細かいとり回しのしづらさなどが
解消されていたりする。

---
### async/await

このあたり詳しく知りたい方は、こちらのスライドをみると◎
- https://www.slideshare.net/UnityTechnologiesJapan/unite-tokyo-2018asyncawait

---

### UniTaskとは

C#の標準TaskおよびTaskSchedulerを
Unity向けに最適化して実装したC#のライブラリ
拡張メソッドによって使いやすくなっていたり、
同様にしてIEnumeratorをawaitできたりする
使いやすいように様々なメソッドまで用意されていて
Unityでasync/awaitするなら基本的に使用が推奨される
※ 各社で導入実績も多い(UniRxよりも使われているイメージ)

---

### UniTaskを導入する

CysharpのGitHubリリースページから
最新版の `unitypackage` をダウンロードする

- https://github.com/Cysharp/UniTask/releases


<!--
様々な導入方法があるが今回はUPM経由で行う。
manifest.jsonの依存パッケージに以下を追加。

```
"dependencies": {
    "com.tk-aria.unitask": "https://github.com/tk-aria/UniTask.git?path=src/UniTask/Assets/Plugins/UniTask#master",
    ...
```
--->

<!---

以下のような’RunOnMainThread’というメソッドを追加している。

```
public static async UniTask RunOnMainThread(Action action, bool configureAwait = true, CancellationToken cancellationToken = default)
{
    cancellationToken.ThrowIfCancellationRequested();
    await UniTask.SwitchToMainThread();
    cancellationToken.ThrowIfCancellationRequested();
    if (configureAwait){
        try{
            action();
        }
        finally{
            await UniTask.Yield();
        }
    }else{
        action();
    }
    cancellationToken.ThrowIfCancellationRequested();
}
```


<!-- slide-memo



-->

---

## サンプル

UnityWebRequestで通信してみる(GETリクエスト)

```
async UniTask<string> Request(string url)
{
    using (var req = UnityWebRequest.Get(url))
    {
        await req.SendWebRequest();
        Debug.Log($"{req.url} : {req.downloadHandler.text}");
        return req.downloadHandler.text;
    }
}
```

---
### サンプル


呼び出し側

```
async void Start()
{
    var res = await Request("http://localhost:8080");
    Debug.Log($"response: {res}");
}
```

---

## サンプル


Coroutineの場合

```
IEnumerator Request(string url, Action<string> onComplete)
{
    using (var req = UnityWebRequest.Get(url))
    {
        yield return req.SendWebRequest ();
        Debug.Log($"{req.url} : {req.downloadHandler.text}");
        onComplete?.Invoke(req.downloadHandler.text);
    }
}
```

---

## サンプル


呼び出し側

```
void Start()
{
    StartCoroutine(Request("http://localhost:8080", (x) =>
        {
            Debug.Log($"response: {x}");
        }));
}
```

---

基本的な処理の流れは変わらず、
コルーチンを使ってデータを受け取るにはコールバックを用いる必要がある。
ここでは分からないが機能面はasync/awaitの方が豊富。

---

UnityWebRequestで気になることがあれば、
公式マニュアルをあたってみよう！

```
https://docs.Unity3d.com/ja/ScriptReference/Networking.UnityWebRequest.html
```

---
<!--
_backgroundColor: orange
paginate: true
-->

# ハンズオン


---

## ハンズオンの内容

- hello world
- UIと連携する
- ユーザーデータを送信する
- textureのダウンロード
- audioのダウンロード
- タイムアウト処理
- エラーハンドリング
- キャンセル処理

---
### Unityを開いてハンズオンを始めよう!

※Unityとエディタ画面を共有して進めていきます。

---
## 接続先（サーバ）の設定

`Utility.cs` の `HostName` を指定のurlへ書き換える

```
public static partial class Utility
{
    public const string HostName = /*ここを書き換える*/;
}
```

---
## hello world

ここではまずはじめに、httpリクエストのレスポンスを
コンソールログから確認してみます。

---
## hello world

- `Example01.Scene` を開きます

---
## hello world

Example01.csへ以下を実装します

```
async UniTask<string> Request(string url)
{
    using (var req = UnityWebRequest.Get(url))
    {
        await req.SendWebRequest();
        Debug.Log($"{req.url} : {req.downloadHandler.text}");
        return req.downloadHandler.text;
    }
}
```

---
## hello world

`ConsoleLog` で `hello world` の出力がされていれば成功です

---
## UIと連携する

ここでは、UIイベントからhttpリクエストを発行し
UIに出力するということをやってみます

---
## UIと連携する

`Example02.Scene` を開きます

---
## UIと連携する

scene用でobjectの設定を行います

---
## UIと連携する

Example02.csへ以下を実装します
※ 先程のExample01をベースに以下を追加するイメージになります

```
[SerializeField] private Text messageText;
```

```
public async UniTaskVoid SendRequestAsync()
    => messageText.text = await Request(Utility.HostName);
```

---
## UIと連携する

ボタンから発行するイベント関数を以下のように実装します

```
public void OnClickButton()
    => SendRequestAsync().Forget();
```

---
## UIと連携する

ボタンを押してText文字が `helloworld` になっていたら成功です

---
## ユーザーデータを送信する

POSTを使用した通信を行います。
今まではデータを取得することを行ってきましたが、
POSTを使用することでサーバにデータを送信することが出来るようになります

ここでは、ユーザーデータをサーバに送ってみます

---
## ユーザーデータを送信する

`Example03.Scene` を開きます

---
## ユーザーデータを送信する

- ユーザーデータの送信処理

```
private async UniTask<User> Request(string url, User user)
{
    var json = JsonUtility.ToJson(user);
    var payload = Encoding.UTF8.GetBytes(json);

    using (var req = new UnityWebRequest(url, UnityWebRequest.kHttpVerbPOST, new DownloadHandlerBuffer(), new UploadHandlerRaw(payload)))
    {
        req.SetRequestHeader("Content-Type", "application/json");
        await req.SendWebRequest();
        Debug.Log($"{req.url} : {req.downloadHandler.text}");
        return JsonUtility.FromJson<User>(req.downloadHandler.text);
    }
}
```

---
## ユーザーデータを送信する

- ユーザーデータの送信処理

```
public async UniTaskVoid SendRequestAsync()
{
    var user = await Request($"{Utility.HostName}/user/create",
        new User {
            name = nameField.text,
            email = mailField.text
        });
    Debug.Log($"response => name:{user.name}, email:{user.email}");
}
```

---
## ユーザーデータを送信する

- UI連携部分とイベント関数

```
[SerializeField] InputField nameField;
[SerializeField] InputField mailField;

public void OnEnterInputForm()
    => SendRequestAsync().Forget();
```

---
## ユーザーデータを送信する

`ConsoleLog` で `hello world` の出力がされていれば成功です

---
## textureのダウンロード

ここでは、http通信を用いてテクスチャを取り扱う方法を実践します。
ダウンロードしてきたTextureをCubeに貼り付けていきます。

---
## textureのダウンロード

`Example04.Scene` を開きます

---
## textureのダウンロード

```
[SerializeField] private Material material;

private async UniTaskVoid SendRequestAsync()
    => material.mainTexture = await Request(Utility.HostName + "/asset/icon.png");

async UniTask<Texture> Request(string url)
{
    using (var req = UnityWebRequestTexture.GetTexture(url))
    {
        await req.SendWebRequest();
        Debug.Log($"completed => {req.url}");
        return DownloadHandlerTexture.GetContent(req);
    }
}
```

---
## textureのダウンロード

リクエストの発行に関しては、これまでの方法を使って呼び出しましょう。
リクエストを送信して `Cube` の `Texture` が変更されていれば成功です

---
## audioのダウンロード

ここでは、http通信を用いて音声データを取り扱う方法を実践します。
ダウンロードしてきたWavファイルを再生する処理を行なっていきます。

---
## audioのダウンロード

`Example05.Scene` を開きます

---
## audioのダウンロード

- `SerializeField`

```
[SerializeField] private AudioSource audioSource;
[SerializeField] private string assetName = "sample.wav";
```

---
## audioのダウンロード

- Wavのダウンロード処理

```
private async UniTask<AudioClip> Request(string url)
{
    using (var req = UnityWebRequestMultimedia.GetAudioClip(url, AudioType.WAV))
    {
        await req.SendWebRequest().ToUniTask();
        return DownloadHandlerAudioClip.GetContent(req);
    }
}
```

---
## audioのダウンロード

- Wavのダウンロード処理

```
async UniTaskVoid SendRequestAsync()
{
    var res = await Request($"{Utility.HostName}/asset/{assetName}");
    if (audioSource.isPlaying)
    {
        audioSource.Stop();
    }
    audioSource.clip = res;
    audioSource.Play();
}
```

---
## audioのダウンロード

リクエストの発行に関しては、これまでの方法を使って呼び出しましょう。
リクエストを送信して `AudioSource` を通じて 音声の再生が行われていれば成功です

---
## さらに進んだ先

- タイムアウト処理
- エラーハンドリング
- キャンセル処理

---
<!--
_backgroundColor: black
_color: white
-->
---


## まとめ

unityでhttp通信する方法に手法に関してお話ししました

- async/awaitを押さえる
- Unityでhttp通信をする場合はUnityWebRequestを使う
- UniTaskはunity上でasync/awaitを使いやすくしたライブラリ
- サーバ側のことも知っておくといいことがある

- (おまけ)<!--ソフトウェアレイヤーになると-->使用するプロトコルによって実装手法が変わるので都度調べる必要がある。

---
<!--
_backgroundColor: gray
_color: white
-->

## 著作権表記など

- 効果音ラボ

https://soundeffect-lab.info/sound/anime/mp3/fate1.mp3


---
<!--
_backgroundColor: gray
_color: white
-->
## 引用画像

- https://www.holynight.net/wp-content/uploads/2020/10/VRChat_1920x1080_2020-10-04_08-01-41.305.jpg.webp
- https://www.goodfind.jp/common/img/space/detail/banner_pc_539.jpg
- https://www.mogiten-rentaru.com/spotcooler/blog/wp-content/uploads/2020/10/IMG_8675-scaled.jpg
- https://i.ytimg.com/vi/668cIbMGoHI/maxresdefault.jpg
- https://imag.malavida.com/mvimgbig/download-fs/twitter-12580-2.jpg
- https://img.redbull.com/images/c_crop,w_1613,h_1075,x_239,y_0,f_auto,q_auto/c_scale,w_1500/redbullcom/2020/5/13/i1pyqdapxumjllhfjoco/duos-apex-legends
- https://www.holynight.net/wp-content/uploads/2020/10/VRChat_1920x1080_2020-10-04_08-01-41.305.jpg.webp
---
<!--
_backgroundColor: black
_color: white
-->

# 本日はおつかれさまでした

<!---


クライアントとは

ユーザが直接使用するアプリケーションのこと

サーバーとは

ここでは、アプリケーション（サービス）を動作させるに当たって必要になるアプリケーションとします。



これらが通信を行うことで、アプリケーションが連携をすることができる。

application

NativeApplication
WebApplication
Game
VR/AR(XR)
business application
アプリケーションサーバ
認証サーバ
PVPサーバ
配信サーバ
アセットサーバ
DB
ペイウォール
プロキシサーバ

serverside, clientside

サーバ運用の種類
オンプレ、クラウド...

Amazon, Google, Microsoft(Azure), Oracle...

仮想レンタルサーバ

さくらレンタルサーバ
GCE
AmazonECS
OracleVMInstance

serverless

Lambda
CloudFunctions
Heroku


開発の仕方



local -> dev -> stg -> prod





GCS Price

STANDARD STORAGE	NEARLINE STORAGE	COLDLINE STORAGE	ARCHIVE STORAGE
$0.02～（GB 単位/月）	$0.01～（GB 単位/月）	$0.004～（GB 単位/月）	$0.0012～（GB 単位/月）



1 日あたり 28 インスタンス時間を無料でご利用いただけます。


タイムアウト処理
キャンセレーション
アセットバンドル
Adressable

Coroutine Async/Await

返り値を返せるようになるのでシンプルにまとまる。
可読性の向上
try catchができる。

coroutineはもともと非同期の仕組みではないので、細かいとこで制約が発生する。
今回の本題とずれるので気になるかたは調べてみると良いかと思います。


---
-->
