---
title: ".NET Framework 配置ガイド (管理者向け) | Microsoft Docs"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- administrator's guide, deploying .NET Framework
- deployment [.NET Framework], administrator's guide
ms.assetid: bee14036-0436-44e8-89f5-4bc61317977a
caps.latest.revision: 40
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: Machine Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 95f0a049db17d8aa2d55200c184be2b2f2ec45fc
ms.contentlocale: ja-jp
ms.lasthandoff: 06/02/2017

---
# <a name="net-framework-deployment-guide-for-administrators"></a>.NET Framework 配置ガイド (管理者向け)
この記事では、システム管理者が Microsoft System Center Configuration Manager を使用して [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] とそのシステムの依存関係をネットワーク経由で配置する方法を手順に沿って説明します。 ここでは、すべての対象のクライアント コンピューターが .NET Framework の最小要件を満たしていることを前提としています。 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] のインストールに必要なソフトウェア要件とハードウェア要件の一覧については、「[システム要件](../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
> [!NOTE]
>  [!INCLUDE[net_v45](../../../includes/net-v45-md.md)]、System Center Configuration Manager、Active Directory など、このドキュメントで言及されるソフトウェアには、ライセンス条項が適用されます。 このドキュメントの内容は、ライセンス条項がソフトウェアの適切なライセンス取得者によって確認され、同意されていることを前提にしています 記載の内容についても、ライセンス条項は効力があるものとします。  
>   
>  .NET Framework のサポートの詳細については、Microsoft サポート オンラインの [Microsoft .NET Framework のサポート ライフサイクル ポリシー](http://go.microsoft.com/fwlink/?LinkId=196607)に関するページを参照してください。  
  
 このトピックは、次のセクションで構成されています。  
  
 [配置プロセス](#the_deployment_process)   
 [.NET Framework の配置](#deploying_in_a_test_environment)   
 [コレクションの作成](#creating_a_collection)  
 [パッケージとプログラムの作成](#creating_a_package)  
 [配布ポイントの選択](#select_dist_point)  
 [パッケージの配置](#deploying_package)  
[リソース](#resources)  
[トラブルシューティング](#troubleshooting)  
  
<a name="the_deployment_process"></a>   
## <a name="the-deployment-process"></a>配置プロセス  
 サポートするインフラストラクチャが整っている場合は、System Center 2012 Configuration Manager を使用して、.NET Framework 再頒布可能パッケージをネットワーク上のコンピューターに配置します。 インフラストラクチャを構築するには、コレクション、ソフトウェアのパッケージとプログラム、配布ポイント、配置という 5 つの主要な項目を作成し定義する必要があります。  
  
-   **コレクション**は、ユーザー、ユーザー グループ、コンピューターなど、.NET Framework の配置先となる Configuration Manager リソースのグループです。 詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のコレクション](http://technet.microsoft.com/library/gg682169.aspx)」を参照してください。  
  
-   **パッケージとプログラム**は、通常、クライアント コンピューターにインストールされるソフトウェア アプリケーションを表しますが、個々のファイル、更新プログラム、さらには個々のコマンドが含まれることがあります。 詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のパッケージとプログラム](http://technet.microsoft.com/library/gg699369.aspx)」を参照してください。  
  
-   **配布ポイント**は、ソフトウェアをクライアント コンピューター上で実行するために必要なファイルを格納する Configuration Manager のサイト システムの役割です。 Configuration Manager クライアントは、ソフトウェア配置を受け取って処理するときに、配布ポイントに接続してソフトウェアに関連するコンテンツをダウンロードし、インストール処理を開始します。 詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のコンテンツ管理の概要](http://technet.microsoft.com/library/gg682083.aspx)」を参照してください。  
  
-   **配置**は、指定されたターゲット コレクションの該当するメンバーにソフトウェア パッケージをインストールするよう指示します。 詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager でのアプリケーションの展開方法](http://technet.microsoft.com/library/gg682082.aspx)」を参照してください。  
  
> [!IMPORTANT]
>  このトピックの手順では、パッケージとプログラムを作成するための一般的な設定を使用していて、すべての可能な設定について説明していない場合があります。 その他の Configuration Manager の配置オプションについては、[Configuration Manager のドキュメント ライブラリ](http://technet.microsoft.com/library/gg682041.aspx)を参照してください。  
  
<a name="deploying_in_a_test_environment"></a>   
## <a name="deploying-the-net-framework"></a>.NET Framework の配置  
 System Center 2012 を使用して、インストール処理時にユーザーが操作しない [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] のサイレント インストールを配置できます。 この場合は、以下の手順に従ってください。  
  
1.  [コレクションを作成します](#creating_a_collection)。  
  
2.  [.NET Framework 再頒布可能パッケージとプログラムを作成します](#creating_a_package)。  
  
3.  [配布ポイントを選択します](#select_dist_point)。  
  
4.  [パッケージを配置します](#deploying_package)。  
  
<a name="creating_a_collection"></a>   
### <a name="create-a-collection"></a>コレクションの作成  
 この手順では、パッケージとプログラムを配置するコンピューターを選択し、それをデバイス コレクションにグループ化します。 Configuration Manager でコレクションを作成するときは、ダイレクト メンバーシップ規則 (コレクション メンバーを手動で指定) またはクエリ規則 (指定した条件に基づいて Configuration Manager がコレクション メンバーを決定) を使用できます。 メンバーシップ規則の詳細については、クエリ規則とダイレクト規則も含めて、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のコレクションの概要](http://technet.microsoft.com/library/gg682177.aspx)」を参照してください。  
  
 コレクションを作成するには  
  
1.  Configuration Manager コンソールで、**[資産とコンプライアンス]** をクリックします。  
  
2.  **[資産とコンプライアンス]** ワークスペースで、**[デバイス コレクション]** をクリックします。  
  
3.  **[ホーム]** タブの **[作成]** グループで、**[デバイス コレクションの作成]** をクリックします。  
  
4.  **デバイス コレクションの作成ウィザード**の **[全般]** ページで、コレクションの名前を入力します。  
  
5.  **[参照]** をクリックして、限定するコレクションを指定します。  
  
6.  **[メンバーシップの規則]** ページで、**[規則の追加]** をクリックし、**[ダイレクト規則]** をクリックして**ダイレクト メンバーシップの規則の作成ウィザード**を開きます。 **[次へ]** をクリックします。  
  
7.  **[リソースの検索]** ページで、**[リソース クラス]** ボックスの一覧の **[システム リソース]** をクリックします。 **[属性名]** ボックスの一覧の **[名前]** をクリックします。 **[値]** フィールドに「`%`」と入力し、**[次へ]** をクリックします。  
  
8.  **[リソースの選択]** ページで、.NET Framework を配置する各コンピューターのチェック ボックスをオンにします。 **[次へ]** をクリックして、ウィザードの操作を完了します。  
  
9. **デバイス コレクションの作成ウィザード**の **[メンバーシップの規則]** ページで、**[次へ]** をクリックし、ウィザードの処理を完了します。  
  
 コレクションの詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のコレクション](http://technet.microsoft.com/library/bb693730.aspx)」を参照してください。  
  
<a name="creating_a_package"></a>   
### <a name="create-a-package-and-program-for-the-net-framework-redistributable-package"></a>.NET Framework 再頒布可能パッケージとプログラムの作成  
 次の手順では、.NET Framework 再頒布可能パッケージを手動で作成します。 パッケージには、.NET Framework をインストールするためのパラメーター、およびターゲット コンピューターへのパッケージの配布元となる場所が含まれます。  
  
 パッケージを作成するには  
  
1.  Configuration Manager コンソールで、**[ソフトウェア ライブラリ]** をクリックします。  
  
2.  **[ソフトウェア ライブラリ]** ワークスペースで、**[アプリケーション管理]** を展開し、**[パッケージ]** をクリックします。  
  
3.  **[ホーム]** タブの **[作成]** グループで、**[パッケージの作成]** をクリックします。  
  
4.  **パッケージとプログラムの作成ウィザード**の **[パッケージ]** ページで、次の情報を入力します。  
  
    -   名前: `.NET Framework 4.5`  
  
    -   製造元: `Microsoft`  
  
    -   言語:  `English (US)`  
  
5.  **[このパッケージにソース ファイルを含める]** チェック ボックスをオンにし、**[参照]** をクリックして .NET Framework のインストール ファイルを含むローカルまたはネットワーク フォルダーを選択します。 フォルダーを選択したら、**[OK]** をクリックし、**[次へ]** をクリックします。  
  
6.  ウィザードの **[プログラミングの種類]** ページで、**[標準プログラム]** をクリックし、**[次へ]** をクリックします。  
  
7.  **パッケージとプログラムの作成ウィザード**の **[プログラム]** ページで、次の情報を入力します。  
  
    1.  **名前:** `.NET Framework 4.5`  
  
    2.  **コマンド ライン:** `dotNetFx45_Full_x86_x64.exe /q /norestart /ChainingPackage ADMINDEPLOYMENT` (コマンド ライン オプションの説明はこの手順の後の表にあります)  
  
    3.  **実行:** **[非表示]** をクリックします。  
  
    4.  **プログラムの実行条件:** ユーザーがログオンしているかどうかに関係なく、プログラムを実行できることを指定するオプションを選択します。  
  
8.  **[要件]** ページで、**[次へ]** をクリックして既定値を受け入れ、ウィザードの処理を完了します。  
  
 次の表は、手順 7. で指定したコマンド ライン オプションについて説明しています。  
  
|オプション|説明|  
|------------|-----------------|  
|**/q**|クワイエット モードを設定します。 ユーザー入力は必要なく、出力は表示されません。|  
|**/norestart**|セットアップ プログラムが自動的に再起動しないようにします。 このオプションを使用する場合、Configuration Manager でコンピューターの再起動を処理する必要があります。|  
|**/chainingpackage** *PackageName*|チェーンを行っているパッケージの名前を指定します。 この情報は、[Microsoft カスタマー エクスペリエンス向上プログラム (CEIP)](http://go.microsoft.com/fwlink/p/?LinkId=248244) に申し込んだ場合のその他のインストール セッション情報と共に報告されます。 パッケージ名にスペースが含まれている場合は、区切り記号として二重引用符を使用します (例: **/chainingpackage "Chaining Product"**)。|  
  
 これらの手順によって、.NET Framework 4.5 という名前のパッケージが作成されます。 プログラムは、.NET Framework 4.5 のサイレント インストールを配置します。 サイレント インストールでは、ユーザーはインストール プロセスと対話しないので、チェーン アプリケーションがリターン コードをキャプチャし、再起動を処理する必要があります。MSDN ライブラリの「[Getting Progress Information from an Installation Package](http://go.microsoft.com/fwlink/?LinkId=179606)」 (インストール パッケージからの進行状況に関する情報の取得) を参照してください。  
  
<a name="select_dist_point"></a>   
### <a name="select-a-distribution-point"></a>配布ポイントの選択  
 サーバーからクライアント コンピューターにパッケージとプログラムを配布するには、まずサイト システムを配布ポイントとして指定し、次にパッケージを配布ポイントに配布する必要があります。  
  
 以下の手順を使用して、前のセクションで作成した .NET Framework 4.5 パッケージの配布ポイントを選択します。  
  
1.  Configuration Manager コンソールで、**[ソフトウェア ライブラリ]** をクリックします。  
  
2.  **[ソフトウェア ライブラリ]** ワークスペースで、**[アプリケーション管理]** を展開し、**[パッケージ]** をクリックします。  
  
3.  パッケージの一覧で、前のセクションで作成したパッケージ **[.NET Framework 4.5]** を選択します。  
  
4.  **[ホーム]** タブの **[展開]** グループで、**[コンテンツの配布]** をクリックします。  
  
5.  **コンテンツの配布ウィザード**の **[全般]** タブで、**[次へ]** をクリックします。  
  
6.  ウィザードの **[コンテンツの配布先]** ページで、**[追加]** をクリックし、**[配布ポイント]** をクリックします。  
  
7.  **[配布ポイントの追加]** ダイアログ ボックスで、パッケージとプログラムをホストする配布ポイントを選択し、**[OK]** をクリックします。  
  
8.  ウィザードを完了します。  
  
 これでパッケージには、.NET Framework 4.5 をサイレントで配置するために必要なすべての情報が含まれています。 パッケージとプログラムを配置する前に、そのパッケージが配布ポイントにインストールされていることを確認します。Configuration Manager ドキュメント ライブラリの「[Configuration Manager のコンテンツ管理の操作とメンテナンス](http://technet.microsoft.com/library/gg712694.aspx#BKMK_MonitorContent)」の「コンテンツの監視」セクションを参照してください。  
  
<a name="deploying_package"></a>   
### <a name="deploy-the-package"></a>パッケージの配置  
 .NET Framework 4.5 パッケージとプログラムを配置するには  
  
1.  Configuration Manager コンソールで、**[ソフトウェア ライブラリ]** をクリックします。  
  
2.  **[ソフトウェア ライブラリ]** ワークスペースで、**[アプリケーション管理]** を展開し、**[パッケージ]** をクリックします。  
  
3.  パッケージの一覧で、作成したパッケージ **[.NET Framework 4.5]** を選択します。  
  
4.  **[ホーム]** タブの **[展開]** グループで、**[展開]** をクリックします。  
  
5.  **ソフトウェアの展開ウィザード**の **[全般]** ページで、**[参照]** をクリックし、前に作成したコレクションを選択します。 **[次へ]** をクリックします。  
  
6.  ウィザードの **[コンテンツ]** ページで、ソフトウェアを配布するポイントが表示されていることを確認し、**[次へ]** をクリックします。  
  
7.  ウィザードの **[展開設定]** ページで、**[操作]** が **[インストール]** に設定され、**[目的]** が **[必須]** に設定されていることを確認します。 これにより、ソフトウェア パッケージがターゲット コンピューターに対する必須インストールになることが保証されます。 **[次へ]** をクリックします。  
  
8.  ウィザードの **[スケジュール]** ページで、.NET Framework をインストールする時期を指定します。 **[新規]** をクリックしてインストール時期を指定することも、ユーザーがログオンまたはログオフしたとき、またはできるだけ早い時期にインストールすることもできます。 **[次へ]** をクリックします。  
  
9. ウィザードの **[ユーザー側の表示と操作]** ページで、既定値を使用し、**[次へ]** をクリックします。  
  
    > [!WARNING]
    >  稼動環境では、配置スケジュールで異なる選択が必要になるポリシーが適用されている場合があります。 これらのオプションについては、TechNet ライブラリの「[[提供情報の名前のプロパティ] の [スケジュール] タブ](http://technet.microsoft.com/library/bb694016.aspx)」を参照してください。  
  
10. ウィザードの **[配布ポイント]** ページで、既定値を使用し、**[次へ]** をクリックします。  
  
11. ウィザードを完了します。 **[監視]** ワークスペースの **[展開]** ノードで配置の進行状況を監視できます。  
  
 これで、対象のコレクションにパッケージが配置され、.NET Framework 4.5 のサイレント インストールが開始されます。 .NET Framework 4.5 のインストールのエラー コードについては、このトピックの「[リターン コード](#return_codes)」セクションを参照してください。  
  
<a name="resources"></a>   
## <a name="resources"></a>リソース  
 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 再頒布可能パッケージの配置をテストするためのインフラストラクチャの詳細については、次のリソースを参照してください。  
  
 **Active Directory、DNS、DHCP:**  
  
-   [Windows Server 2008 向け Active Directory Domain Services](http://technet.microsoft.com/library/dd378891.aspx)  
  
-   [DNS サーバー](http://technet.microsoft.com/library/cc732997.aspx)  
  
-   [DHCP サーバー](http://technet.microsoft.com/library/cc896553.aspx)  
  
 **SQL Server 2008:**  
  
-   [SQL Server 2008 のインストール (SQL Server ビデオ)](http://technet.microsoft.com/library/dd299415.aspx)  
  
-   [SQL Server 2008 のデータベース管理者向けセキュリティ概要](http://download.microsoft.com/download/a/c/d/acd8e043-d69b-4f09-bc9e-4168b65aaa71/SQL2008SecurityOverviewforAdmins.docx)  
  
 **System Center 2012 Configuration Manager (管理ポイント、配布ポイント):**  
  
-   [System Center 2012 Configuration Manager のサイト管理](http://technet.microsoft.com/library/gg681983.aspx)  
  
-   [Configuration Manager の単一サイトの計画と展開](http://technet.microsoft.com/library/bb680961.aspx)  
  
 **Windows コンピューター用の System Center 2012 Configuration Manager クライアント:**  
  
-   [System Center 2012 Configuration Manager のクライアントの展開](http://technet.microsoft.com/library/gg699391.aspx)  
  
<a name="troubleshooting"></a>   
## <a name="troubleshooting"></a>トラブルシューティング  
  
### <a name="log-file-locations"></a>ログ ファイルの場所  
 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] のセットアップ中に次のログ ファイルが作成されます。  
  
 %temp%\Microsoft .NET Framework 4.5*.txt   
 %temp%\Microsoft .NET Framework 4.5\*.html  
  
 [ログ収集ツール](http://www.microsoft.com/download/details.aspx?id=12493)を使用して [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] のログ ファイルを収集し、それらのファイルのサイズを縮小した圧縮キャビネット (.cab) ファイルを作成できます。  
  
<a name="return_codes"></a>   
### <a name="return-codes"></a>リターン コード  
 次の表は、[!INCLUDE[net_v45](../../../includes/net-v45-md.md)] の再頒布可能インストール プログラムから返される最も一般的なリターン コードを示しています。 これらのリターン コードは、すべてのバージョンのインストーラーで共通です。  
  
 詳細情報へのリンクについては、次の「[ダウンロードのエラー コード](#additional_error_codes)」セクションを参照してください。  
  
|リターン コード|説明|  
|-----------------|-----------------|  
|0|インストールは正常に終了しました。|  
|1602|ユーザーがインストールをキャンセルしました。|  
|1603|インストール中に致命的なエラーが発生しました。|  
|1641|インストールを完了するには再起動する必要があります。 このメッセージが表示された場合、操作は成功しました。|  
|3010|インストールを完了するには再起動する必要があります。 このメッセージが表示された場合、操作は成功しました。|  
|5100|ユーザーのコンピューターは、システム要件を満たしていません。|  
  
<a name="additional_error_codes"></a>   
### <a name="download-error-codes"></a>ダウンロードのエラー コード  
  
-   [Background Intelligent Transfer Service (BITS) のエラー コード](http://msdn.microsoft.com/library/aa362823.aspx)  
  
-   [URL モニカーのエラー コード](http://msdn.microsoft.com/library/ms775145.aspx)  
  
-   [WinHttp エラー コード](http://msdn.microsoft.com/library/aa383770.aspx)  
  
 その他のエラー コード:   
  
-   [Windows インストーラーのエラー コード](http://msdn.microsoft.com/library/aa368542.aspx)  
  
-   [Windows Update エージェントの結果コード](http://technet.microsoft.com/library/cc720442.aspx)  
  
## <a name="see-also"></a>関連項目  
 [配置ガイド (開発者向け)](../../../docs/framework/deployment/deployment-guide-for-developers.md)   
 [システム要件](../../../docs/framework/get-started/system-requirements.md)
