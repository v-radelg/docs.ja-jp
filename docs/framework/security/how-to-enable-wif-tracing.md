---
title: "方法: WIF トレースの有効化 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 271b6889-3454-46ff-96ab-9feb15e742ee
caps.latest.revision: 3
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 3
---
# 方法: WIF トレースの有効化
## 対象  
  
-   Microsoft® Windows® Identity Foundation \(WIF\)  
  
-   ASP.NET® Web フォーム  
  
## 要約  
 ここでは、ASP.NET アプリケーションの WIF トレースを有効にするための詳細な操作手順を示します。  また、トレース リスナーとログが正しく動作していることを確認するためにアプリケーションをテストする方法についても説明します。  ここでは、セキュリティ トークン サービス \(STS\) を作成するための詳細な手順については説明しません。代わりに、Identity and Access Tool に付属している開発用 STS を使用します。  開発用 STS はテスト用に用意されたもので、実際の認証は行いません。  このページの内容を完了するには、Identity and Access Tool をインストールする必要があります。  これは、「[Identity and Access Tool](http://go.microsoft.com/fwlink/?LinkID=245849)」からダウンロードできます。  
  
> [!IMPORTANT]
>  パッシブ アプリケーション \(つまり、WS フェデレーション プロトコルを使用するアプリケーション\) の WIF トレースを有効にすると、アプリケーションをサービス拒否 \(DoS\) 攻撃に晒したり、悪意のあるユーザーに情報を開示する可能性があります。  これにはパッシブ RP とパッシブ STS の両方が含まれます。  したがって、稼動環境ではパッシブ RP またはパッシブ STS の WIF トレースを有効にしないことをお勧めします。  
  
## 内容  
  
-   目的  
  
-   概要  
  
-   手順の要約  
  
-   手順 1 – 簡単な ASP.NET Web フォーム アプリケーションの作成とトレースの有効化  
  
-   手順 2 – ソリューションのテスト  
  
## 目的  
  
-   Identity and Access Tool の WIF および開発用 STS を使用する簡単な ASP.NET アプリケーションの作成  
  
-   トレースの有効化と動作確認  
  
## 概要  
 トレースすると、トークン、クッキー、クレーム、プロトコル メッセージなどを含む、さまざまな種類の WIF の問題をデバッグおよびトラブルシューティングすることができます。  WIF トレースは、WCF トレースに似ています。たとえば、トレースの詳細レベルを選択して、警告メッセージから全メッセージまで、すべてを表示させることができます。  WIF トレースは、**.xml** ファイル、またはサービス トレース ビューアー ツールを使用して表示できる **.svclog** ファイルで作成できます。  このツールは、コンピューターの Windows SDK インストール パスの **bin** ディレクトリにあります \(例: **C:\\Program Files\\Microsoft SDKs\\Windows\\v7.1\\Bin\\SvcTraceViewer.exe**\)。  
  
## 手順の要約  
  
-   手順 1 – 簡単な ASP.NET Web フォーム アプリケーションの作成とトレースの有効化  
  
-   手順 2 – ソリューションのテスト  
  
## 手順 1 – 簡単な ASP.NET Web フォーム アプリケーションの作成とトレースの有効化  
 この手順では、新しい ASP.NET Web フォーム アプリケーションを作成し、トレースを有効にするために *Web.config* ファイルを変更します。  
  
#### 簡単な ASP.NET アプリケーションを作成するには  
  
1.  Visual Studio を起動し、**\[ファイル\]**、**\[新規作成\]** の順にポイントして、**\[プロジェクト\]** をクリックします。  
  
2.  **\[新しいプロジェクト\]** ウィンドウで、**\[ASP.NET Web フォーム アプリケーション\]** をクリックします。  
  
3.  **\[名前\]** で、「`TestApp`」と入力し、**\[OK\]** をクリックします。  
  
4.  **ソリューション エクスプローラー**で **\[TestApp\]** プロジェクトを右クリックし、**\[Identity and Access\]** をクリックします。  
  
5.  **\[Identity and Access\]** ウィンドウが表示されます。  **\[Providers\]** で **\[Test your application with the Local Development STS\]** を選択し、**\[Apply\]** をクリックします。  
  
6.  表示されている例 \(**C:\\logs**\) のように、**C:** ドライブのルートに **logs** という名前の新しいフォルダーを作成します。  
  
7.  表示されている例のように、終了要素 **\<\/configSections\>** の直後に、以下の **\<system.diagnostics\>** 要素を *Web.config* 構成ファイルに追加します。  
  
    ```  
    <configuration>  
        <configSections>  
        …  
        </configSections>  
        <system.diagnostics>  
            <sources>  
                <source name="System.IdentityModel" switchValue="Verbose">  
                    <listeners>  
                        <add name="xml" type="System.Diagnostics.XmlWriterTraceListener" initializeData="C:\logs\WIF.xml" />  
                    </listeners>  
                </source>  
            </sources>  
            <trace autoflush="true" />  
        </system.diagnostics>  
    ```  
  
    > [!NOTE]
    >  **initializeData** 属性で指定されたディレクトリの場所は、ログが開始される前に存在している必要があります。  この場所が存在しない場合、ログは作成されません。  
  
     上記の構成設定では、**詳細**な WIF トレースが有効になり、**C:\\logs\\WIF.xml** ファイルに結果のログが保存されます。  
  
## 手順 2 – ソリューションのテスト  
 この手順では、WIF 対応 ASP.NET アプリケーションをテストし、ログが記録されていることを確認します。  
  
#### 正常なトレースのために WIF 対応 ASP.NET アプリケーションをテストするには  
  
1.  **F5** キーを押して、ソリューションを実行します。  既定の ASP.NET ホーム ページが開き、ユーザー名 *Terry* \(開発用 STS によって返される既定のユーザー\) で自動的に認証されます。  
  
2.  ブラウザー ウィンドウを閉じ、**C:\\logs** フォルダーに移動します。  テキスト エディターを使用して **C:\\logs\\WIF.xml** ファイルを開きます。  
  
3.  **WIF.xml** ファイルを確認し、**\<E2ETraceEvent\>** で始まるエントリが含まれていることを確認します。  これらのトレースには、**\<TraceRecord\>** 要素と、トレースされるアクティビティ \(**Validating SecurityToken** など\) の説明が含まれています。