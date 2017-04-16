---
title: "MageUI.exe (マニフェスト生成および編集ツールのグラフィカル クライアント) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "MageUI.exe"
  - "マニフェストの生成および編集ツール"
ms.assetid: f9e130a6-8117-49c4-839c-c988f641dc14
caps.latest.revision: 38
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 38
---
# MageUI.exe (マニフェスト生成および編集ツールのグラフィカル クライアント)
MageUI.exe でサポートされている機能は、コマンド ライン ツール Mage.exe の機能と同じですが、MageUI.exe には、Windows ベースのユーザー インターフェイス \(UI\) があります。  このツールを使用すると、配置マニフェストおよびアプリケーション マニフェストを作成および編集でき、これらのマニフェストに署名することができます。  MageUI.exe で作成される新しいマニフェストは、[!INCLUDE[net_client_v40_long](../../../includes/net-client-v40-long-md.md)] を対象とします。  以前のバージョンの .NET Framework を対象にするには、以前のバージョンの MageUI.exe を使用する必要があります。  マニフェストに対してアセンブリの追加または削除を実行しても、既存のマニフェストに再署名しても、MageUI.exe は [!INCLUDE[net_client_v40_long](../../../includes/net-client-v40-long-md.md)] を対象にするようにマニフェストを更新しません。  詳しくは、「[Mage.exe \(マニフェストの生成および編集ツール\)](../../../docs/framework/tools/mage-exe-manifest-generation-and-editing-tool.md)」をご覧ください。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。  このツールを実行するには、開発者コマンド プロンプト \(または、Windows 7 の Visual Studio コマンド プロンプト\) を使用します。  詳しくは、「[コマンド プロンプト](../../../docs/framework/tools/developer-command-prompt-for-vs.md)」をご覧ください。  
  
 [!INCLUDE[vs_dev10_long](../../../includes/vs-dev10-long-md.md)] セットアップには、2 つのバージョンの Mage.exe および MageUI.exe がコンポーネントとして含まれています。  バージョン情報を確認するには、MageUI.exe を実行し、**\[ヘルプ\]** をクリックして、**\[バージョン情報\]** をクリックします。  このドキュメントでは、バージョン 4.0.x.x の Mage.exe および MageUI.exe について説明します。  
  
> [!NOTE]
>  MageUI.exe は、MageUI.exe を使用して証明書で署名済みのアプリケーション マニフェストを保存する場合、[compatibleFrameworks](../Topic/%3CcompatibleFrameworks%3E%20Element%20\(ClickOnce%20Deployment\).md) 要素をサポートしません。  代わりに [Mage.exe](../../../docs/framework/tools/mage-exe-manifest-generation-and-editing-tool.md) を使用する必要があります。  
  
## UIElement の一覧  
 次の表に、利用できるメニューおよびツール バー項目の一覧を表示します。  
  
|コマンド|メニュー|ショートカット|説明|  
|----------|----------|-------------|--------|  
|**\[Application Manifest\]**|**\[File\] \> \[New\]**||アプリケーション マニフェストを新規作成します。|  
|**\[Deployment Manifest\]**|**\[File\] \> \[New\]**||配置マニフェストを新規作成します。|  
|**開く**|**ファイル**|Ctrl \+ O|既存の配置マニフェスト、アプリケーション マニフェストまたは信用ライセンスを編集用に開きます。|  
|**閉じる**|**ファイル**|Ctrl \+ F4|開いているファイルを閉じます。<br /><br /> 閉じるファイルが変更されていた場合は、公開キー、キー ペア、または格納済みの証明書を使ってファイルに再度署名することを求めるメッセージが表示されます。|  
|**上書き保存**|**ファイル**|Ctrl \+ S|ユーザー入力のフォーカスがある文書をディスクに保存します。|  
|**名前を付けて保存**|**ファイル**||ファイルをディスクに保存します。この場合、ユーザーは新しいファイル名や場所を指定できます。|  
|**\[Save All\]**|**ファイル**||MageUI.exe 内で現在開かれている各ファイルに対して行った変更を保存します。|  
|**\[Preferences\]**|**ファイル**||**\[Preferences\]** ダイアログ ボックスを開きます。  詳細については、以下のセクションを参照してください。|  
|**終了**|**ファイル**|Alt \+ F4|MageUI.exe を終了します。|  
|**切り取り**|**Edit**|Ctrl \+ X|現在選択されているテキストをアプリケーションから削除し、システムのクリップボードに移動します。|  
|**コピー**|**Edit**|Ctrl \+ C|現在選択されているテキストを、システムのクリップボードにコピーします。|  
|**貼り付け**|**Edit**|Ctrl \+ V|システム クリップボードから現在アクティブなテキスト要素にテキストを貼り付けます。|  
|**削除**|**Edit**||現在選択されている一覧、たとえば **\[Deployment Manifest\]** タブの信用ライセンスなどで現在選択されている要素を削除します。|  
|**\[Close All\]**|**ウィンドウ**||現在 MageUI.exe で開かれているすべてのファイルを閉じます。  保存する必要のあるファイルが存在する場合は、それらのファイルを保存するように要求するメッセージが表示されます。  また、未署名のファイルや変更されたファイルについては、署名キーを選択するように示すメッセージもファイルごとに表示されます。|  
|**バージョン情報**|**ヘルプ**||MageUI.exe のバージョンおよび著作権についての情報が表示されます。|  
  
## \[Preferences\] ダイアログ ボックス  
 **\[Preferences\]** ダイアログ ボックスには、以下の項目があります。  
  
|UI 要素|説明|  
|-----------|--------|  
|**\[Sign on save\]**|変更内容を保存するたびに、ファイルへの署名を促すメッセージが表示されます。|  
|**\[Use default signing certificate\]**|どのファイルに署名する場合でも **\[Certificate file\]** ボックスに入力されたキーを使用します。  こうすると、**\[Sign on Save\]** チェック ボックスをオンにした状態でファイルを保存した場合などに表示される、署名を要求するメッセージが表示されなくなります。  キー ファイルを選択するには、**\[Certificate file\]** ボックスの横の省略記号 \(**...**\) ボタンをクリックします。|  
|ダイジェスト アルゴリズム|アルゴリズムの依存関係のダイジェストを生成するように指定します。  値は、"sha256RSA" または "sha1RSA" であることが必要です。  既定として SHA1 を使用します。  アプリケーション マニフェストと配置マニフェストの両方に署名します。  ユーザーがマニフェストの保存時に証明書を提供した場合、依存関係ダイジェストを生成するために証明書内のアルゴリズムを使用します。|  
  
## \[Signing Options\] ダイアログ ボックス  
 **\[Signing Options\]** ダイアログ ボックスは、マニフェストや信用ライセンスを初めて保存する場合、またはマニフェストや信用ライセンスを変更する場合に表示されます。  **\[Preferences\]** ダイアログ ボックスの **\[Sign on Save\]** チェック ボックスがオンの場合にだけ表示されます。  **\[TimeStamping URI\]** ボックスの値を指定するマニフェストに署名するときには、インターネットに接続している必要があります。  
  
 このダイアログ ボックスには、以下の項目があります。  
  
|UI 要素|説明|  
|-----------|--------|  
|**\[Sign with certificate file\]**|ファイル システムに格納されているデジタル証明書でマニフェストに署名します。|  
|**ファイル**|証明書を表す .pfx ファイルのパスの入力欄です。|  
|**...**|既存の .pfx ファイルを選択するための **\[Choose File\]** ダイアログ ボックスを開きます。|  
|**新規作成**|証明機関 \(CA: Certificate Authority\) によって証明できない、.pfx ファイルを新規作成します。  [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] の配置に署名するために使用する証明書の種類の詳細については、「[信頼されたアプリケーションの配置の概要](../Topic/Trusted%20Application%20Deployment%20Overview.md)」を参照してください。|  
|**パスワード**|この証明書で署名するために使用するパスワードの入力欄です。  該当しない場合は、空白にできます。|  
|**\[Sign with stored certificate\]**|使用しているコンピューターの証明書ストアに格納されている、選択可能なデジタル署名が一覧表示されます。|  
|**\[TimeStamping URI\]**|デジタル タイムスタンプ サービスの URI \(Uniform Resource Locator\) が表示されます。  次のバージョンのアプリケーションを配置する前にデジタル証明書の有効期限が切れる場合、マニフェストにタイムスタンプを設定すると、マニフェストに再署名する必要がなくなります。  詳しくは、「[Windows ルート証明書プログラム \- メンバー](http://go.microsoft.com/fwlink/?LinkId=159000)」および「[ClickOnce と Authenticode](../Topic/ClickOnce%20and%20Authenticode.md)」をご覧ください。|  
|**\[Don't Sign\]**|デジタル証明書から署名を追加せずにマニフェストを保存できます。|  
  
## タブおよびパネルの説明  
 MageUI.exe で文書を開くと、その文書用のタブ ページに文書が表示されます。  各タブには、一連のプロパティ パネルがあります。  パネルには、文書のデータをグループ化したサブセットが表示されています。  
  
### \[アプリケーション マニフェスト\] タブ  
 \[**アプリケーション マニフェスト**\] タブには、アプリケーション マニフェストの内容が表示されます。  アプリケーション マニフェストには、配置に含まれるすべてのファイル、およびクライアントでアプリケーションを実行するために必要なアクセス許可が記述されています。  
  
 \[**アプリケーション マニフェスト**\] タブには、次のタブが含まれます。  
  
|UI 要素|説明|  
|-----------|--------|  
|**名前**|この配置に関する識別情報を指定します。|  
|**説明**|発行者、製品、およびサポート情報を指定します。|  
|**アプリケーション オプション**|これがブラウザー アプリケーションであるかどうか、このマニフェストが信頼情報のソースであるかどうかを指定します。|  
|**ファイル**|この配置を構成するすべてのファイルを指定します。|  
|**\[必要なアクセス許可\]**|アプリケーションをクライアント上で実行するために必要な、最小のアクセス許可セットを指定します。|  
  
### \[名前\] タブ  
 \[**名前**\] タブは、アプリケーション マニフェストを初めて作成したり開いたりしたときに表示されます。  このタブでは、配置を一意に識別するほか、オプションで有効な対象プラットフォームを指定できます。  
  
|UI 要素|説明|  
|-----------|--------|  
|**名前**|必ず指定します。  アプリケーション マニフェストの名前です。  通常、ファイル名と同じです。|  
|**Version**|必ず指定します。  *N.N.N.N* という形式の配置バージョン番号です。  先頭のメジャー ビルド番号だけが必須です。  たとえば、アプリケーションのバージョンが 1.0 の場合、有効な値は、`1`、`1.0`、`1.0.0`、`1.0.0.0` です。|  
|**プロセッサ**|省略可能です。  この配置を実行できるコンピューターのアーキテクチャです。  既定値は `msil` \(Microsoft Intermediate Language\) です。これは、すべてのマネージ アセンブリでの既定の形式です。  アプリケーションのアセンブリを特定のアーキテクチャ用にプリコンパイルした場合は、このフィールドを変更します。  プリコンパイルの詳細については、「[Ngen.exe \(ネイティブ イメージ ジェネレーター\)](../../../docs/framework/tools/ngen-exe-native-image-generator.md)」を参照してください。|  
|**カルチャ**|省略可能です。  このアプリケーションが実行される国\/地域を、2 つの部分で構成される ISO のコードで指定します。  既定値は `neutral` です。|  
|**公開キー トークン**|省略可能です。  このアプリケーション マニフェストが署名されたときの公開キーです。  新しいマニフェストまたは未署名のマニフェストでは、このフィールドに \[`署名されていません`\] と表示されます。|  
  
### \[説明\] タブ  
 通常、この情報は配置マニフェスト内で指定します。  これらのフィールドは、\[**アプリケーション オプション**\] タブの \[**信頼情報のアプリケーション マニフェストを使用する**\] チェック ボックスをオンにした場合にのみ変更できます。  
  
|UI 要素|説明|  
|-----------|--------|  
|**発行者**|このアプリケーションの責任を負う個人または組織の名前です。  この値は、\[スタート\] メニューのフォルダー名として使用されます。|  
|**製品**|製品の完全な名前です。  配置マニフェストの \[**配置オプション**\] タブで \[**アプリケーションの種類**\] の \[**ローカルにインストール**\] を選択した場合は、\[**スタート**\] メニューおよび \[**プログラムの追加と削除**\] に、このアプリケーションを示す項目としてこの名前が表示されます。|  
|**サポート URL**|このアプリケーションのヘルプおよびサポート情報をユーザーが入手できるサイトの URL です。|  
  
### \[アプリケーション オプション\] タブ  
  
|UI 要素|説明|  
|-----------|--------|  
|**Windows Presentation Foundation ブラウザー アプリケーション**|これが XAML ブラウザー アプリケーション \(XBAP\) としてブラウザーで実行される WPF アプリケーションであるかどうかを指定します。|  
|**信頼情報のアプリケーション マニフェストを使用する**|このマニフェストに信頼情報を含めるかどうかを指定します。|  
  
### \[ファイル\] タブ  
  
|UI 要素|説明|  
|-----------|--------|  
|**アプリケーション ディレクトリ**|アプリケーションのファイルが格納されているディレクトリです。  ディレクトリを選択するには、省略記号 \(**…**\) ボタンをクリックします。|  
|**作成**|アプリケーション ディレクトリとサブディレクトリにあるすべてのファイルをアプリケーション マニフェストに追加します。  ディレクトリ内に実行可能ファイルが 1 つしか見つからなかった場合は、このファイルがエントリ ポイントとして自動的にマークされます。エントリ ポイントは、ClickOnce アプリケーションをクライアントで起動したときに、最初に実行されます。|  
|**アプリケーション ファイル**|アプリケーション内のすべてのファイルの一覧を示します。  各ファイルには、以下で説明する 3 つの編集可能な属性があります。|  
|**ファイルの種類**|ファイルの種類は以下の 4 つのうちのいずれかです。<br /><br /> -   なし。<br />-   エントリ ポイント。  アプリケーションのプライマリ実行可能ファイルです。  エントリ ポイントとしてマークできる実行可能ファイルは 1 つだけです。<br />-   データ ファイル。  XML ファイルなど、アプリケーションにデータを提供するためのファイルです。<br />-   アイコン ファイル。  アプリケーション ウィンドウの隅やデスクトップに表示されるアプリケーション アイコンです。|  
|**省略可能**|省略可能のマークを付けたファイルは初期インストール時や更新時にはダウンロードされませんが、System.Deployment オンデマンド API を使用して実行時にダウンロードできます。  詳細については、「[チュートリアル : デザイナーを使用し、ClickOnce 配置 API で必要に応じてアセンブリをダウンロードする](../Topic/Walkthrough:%20Downloading%20Assemblies%20on%20Demand%20with%20the%20ClickOnce%20Deployment%20API%20Using%20the%20Designer.md)」を参照してください。|  
|**グループ化**|省略可能なファイル セットのラベルです。  ファイル セットにはグループ ラベルを付けることができ、オンデマンド API を使用して、1 回の API 呼び出しで一連のファイルをダウンロードできます。|  
  
### \[必要なアクセス許可\] タブ  
 アプリケーションに対して、既定で付与される以上のローカル コンピューターへのアクセス許可を与える必要がある場合は、\[**必要なアクセス許可**\] タブを使用します。  詳しくは、「[ClickOnce アプリケーションのセキュリティ](../Topic/Securing%20ClickOnce%20Applications.md)」をご覧ください。  
  
|UI 要素|説明|  
|-----------|--------|  
|**アクセス許可セットの種類**|‏このアプリケーションをクライアントで実行するために必要な、最小のアクセス許可セットです。  これらのアクセス許可セットおよび必要なアクセス許可については、「 [NIB: Named Permission Sets](http://msdn.microsoft.com/ja-jp/08250d67-c99d-4ab0-8d2b-b0e12019f6e3)」を参照してください。|  
|**詳細**|アクセス許可セットを表すために作成された、アプリケーション マニフェスト用の XML です。  アプリケーション マニフェストの XML 形式に精通している場合を除き、この XML を手動で編集しないでください。  詳細については、「[ClickOnce アプリケーション マニフェスト](../Topic/ClickOnce%20Application%20Manifest.md)」を参照してください。|  
  
### \[配置マニフェスト\] タブ  
 \[**配置マニフェスト**\] タブには、次のタブが含まれます。  
  
|UI 要素|説明|  
|-----------|--------|  
|**名前**|この配置に関する識別情報を指定します。|  
|**説明**|発行者、製品、およびサポート情報を指定します。|  
|**配置オプション**|アプリケーションの種類や開始場所など、配置に関する追加情報を指定します。|  
|**更新オプション**|[!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] がアプリケーションの更新プログラムを確認する頻度を指定します。|  
|**アプリケーション参照**|この配置のアプリケーション マニフェストを指定します。|  
  
### \[名前\] タブ  
 \[**名前**\] タブは、配置マニフェストを初めて作成したり開いたりしたときに表示されます。  このタブでは、配置を一意に識別するほか、オプションで有効な対象プラットフォームを指定できます。  
  
|UI 要素|説明|  
|-----------|--------|  
|**名前**|必ず指定します。  配置マニフェストの名前です。  通常、ファイル名と同じです。|  
|**Version**|必ず指定します。  *N.N.N.N* という形式の配置バージョン番号です。  先頭のメジャー ビルド番号だけが必須です。  たとえば、アプリケーションのバージョンが 1.0 の場合、有効な値は、`1`、`1.0`、`1.0.0`、`1.0.0.0` です。|  
|**プロセッサ**|省略可能です。  この配置を実行できるコンピューターのアーキテクチャです。  既定値は `msil` \(Microsoft Intermediate Language\) です。これは、すべてのマネージ アセンブリの既定の形式です。  アプリケーションのアセンブリを特定のアーキテクチャ用にコンパイルした場合は、このフィールドを変更します。|  
|**カルチャ**|省略可能です。  このアプリケーションの実行環境となる、ISO の国\/地域コードです。2 つの部分で構成されています。  既定値は `neutral` です。|  
|**公開キー トークン**|省略可能です。  この配置マニフェストが署名されたときの公開キーです。  新しいマニフェストまたは未署名のマニフェストでは、このフィールドに \[`署名されていません`\] と表示されます。|  
  
### \[説明\] タブ  
  
|UI 要素|説明|  
|-----------|--------|  
|**発行者**|必ず指定します。  このアプリケーションの責任を負う個人または組織の名前です。  この値は、\[スタート\] メニューのフォルダー名として使用されます。|  
|**製品**|必ず指定します。  製品の完全な名前です。  \[**配置オプション**\] タブで \[**アプリケーションの種類**\] の \[**ローカルにインストール**\] を選択した場合は、\[**スタート**\] メニューおよび \[**プログラムの追加と削除**\] に、このアプリケーションを示す項目としてこの名前が表示されます。|  
|**サポート URL**|省略可能です。  このアプリケーションのヘルプおよびサポート情報をユーザーが入手できるサイトの URL です。|  
  
### \[配置オプション\] タブ  
  
|UI 要素|説明|  
|-----------|--------|  
|**\[アプリケーションの種類\]**|省略可能です。  このアプリケーションが、クライアント コンピューターにインストールされるか \(\[**ローカルにインストール**\]\)、オンラインで実行されるか \(\[**オンラインのみ**\]\)、ブラウザーで実行される WPF アプリケーションか \(\[**WPF ブラウザー アプリケーション**\]\) を指定します。  既定値は \[**ローカルにインストール**\] です。|  
|**開始位置**|省略可能です。  アプリケーションを実際に起動するサイトの URL です。  CD からアプリケーションを配置し、Web サイト経由で更新する必要がある場合に便利です。|  
|**開始場所 \(ProviderURL\) をマニフェストに含める**|省略可能です。  [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] でアプリケーションの更新プログラムを確認する場合に参照する URL を指定します。|  
|**インストール後にアプリケーションを自動的に実行する**|必ず指定します。  URL からの最初のインストールが完了したら、すぐにアプリケーションを実行するように [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] に指定します。  既定では、チェック ボックスがオンになっています。|  
|**URL パラメーターをアプリケーションに渡すことを許可する**|必ず指定します。  配置マニフェストの URL にクエリ文字列を追加することで、[!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] アプリケーションにパラメーター データを渡すことができるようにします。  既定では、チェック ボックスがオフになっています。|  
|**.deploy ファイル拡張子を使用する**|必ず指定します。  オンにした場合、アプリケーション マニフェストのすべてのファイルの拡張子が .deploy になります。  既定では、チェック ボックスがオフになっています。|  
  
### \[更新オプション\] タブ  
 \[**更新オプション**\] タブには、\[**名前**\] タブの \[**アプリケーションの種類**\] 選択ボックスで \[**ローカルにインストール**\] が選択されている場合にのみ、ここで説明するオプションが表示されます。  
  
|UI 要素|説明|  
|-----------|--------|  
|**このアプリケーションの更新プログラムを確認する**|[!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] でアプリケーションの更新プログラムを確認する必要があるかどうかを指定します。  このチェック ボックスをオフにした場合、アプリケーションの更新情報は確認されません。ただし、<xref:System.Deployment.Application> 名前空間の API を使用すると、プログラムによってアプリケーションを更新できます。|  
|**アプリケーションの更新を確認する方法を選択してください**|更新プログラムの確認について次の 2 つのオプションがあります。<br /><br /> -   \[**アプリケーションの開始前に行う**\]。  アプリケーションを実行する前に更新プログラムを確認します。<br />-   \[**アプリケーションの開始後に行う**\]。  アプリケーションのメイン フォームが初期化されると更新プログラムの確認が開始され、アプリケーションが次に起動されたときに更新プログラムの確認が実行されます。|  
|**更新プログラムの確認の頻度**|[!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] で更新プログラムを確認する頻度を指定します。<br /><br /> -   \[**アプリケーションが実行するたびに確認する**\]。  ユーザーがアプリケーションを起動するたびに、[!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] によって更新プログラムが確認されます。<br />-   \[**確認する間隔**\]: 更新プログラムを確認するまでの経過時間の間隔と単位 \(時間、日、または週\) を選択します。|  
|**このアプリケーションに最低限必要なバージョンを指定する**|省略可能です。  アプリケーションの特定のバージョンを必須インストールとして指定し、ユーザーが旧バージョンを使用しないようにします。|  
|**Version**|\[**このアプリケーションに最低限必要なバージョンを指定する**\] チェック ボックスをオンにした場合は必須です。  指定されたバージョン番号 が*N.N.N.N* という形式である必要があります。  先頭のメジャー ビルド番号だけが必須です。  たとえば、アプリケーションのバージョンが 1.0 の場合、有効な値は、`1`、`1.0`、`1.0.0`、`1.0.0.0` です。|  
  
### \[アプリケーション参照\] タブ  
 \[**アプリケーション参照**\] タブには、このトピックで既に説明した \[**名前**\] タブと同じフィールドがあります。  ただし、次のフィールドだけが異なります。  
  
|UI 要素|説明|  
|-----------|--------|  
|**マニフェストの選択**|アプリケーション マニフェストを選択できます。  アプリケーション マニフェストを選択すると、このページの他のすべてのフィールドの値が設定されます。|  
  
## 参照  
 [ClickOnce のセキュリティと配置](../Topic/ClickOnce%20Security%20and%20Deployment.md)   
 [チュートリアル : ClickOnce アプリケーションを手動で配置する](../Topic/Walkthrough:%20Manually%20Deploying%20a%20ClickOnce%20Application.md)   
 [Mage.exe \(マニフェストの生成および編集ツール\)](../../../docs/framework/tools/mage-exe-manifest-generation-and-editing-tool.md)