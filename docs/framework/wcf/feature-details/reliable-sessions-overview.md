---
title: "信頼できるセッションの概要 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
ms.assetid: a7fc4146-ee2c-444c-82d4-ef6faffccc2d
caps.latest.revision: 30
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 30
---
# 信頼できるセッションの概要
[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] の SOAP リライアブル メッセージ機能では、SOAP エンドポイント間でエンドツーエンドのメッセージ転送に信頼性が提供されます。これにより、信頼性の低いネットワーク上でも、トランスポート エラーや SOAP メッセージ レベルのエラーに対処できます。具体的には、SOAP またはトランスポート中継局を経由して送信されるメッセージに関して、1 回だけの配信や \(オプションで\) 順序保証の配信がセッション ベースで提供されます。セッション ベースの配信では、セッション中のメッセージがグループ化され、オプションでメッセージの順序保証が適用されます。  
  
 ここでは、信頼できるセッションについての概要、使用方法と使用する状況、およびこれをセキュリティで保護する方法について説明します。  
  
## WCF の信頼できるセッション  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] の信頼できるセッションは、WS\-ReliableMessaging プロトコルの定義に準拠した SOAP リライアブル メッセージ機能の実装です。  
  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] の SOAP リライアブル メッセージ機能は、メッセージング エンドポイントを分離する中継局の数や種類に関係なく、2 つのエンドポイント間でエンドツーエンドの信頼できるセッションを実現します。これには SOAP を使用しないトランスポート手段 \(HTTP プロキシなど\)、またはエンドポイント間でメッセージをやりとりする場合に必要となる SOAP を使用する手段 \(SOAP ベースのルーターやブリッジなど\) が含まれます。信頼できるセッション チャネルでは "インタラクティブな" 通信がサポートされるので、これらのチャネルによって接続されている複数のサービスを同時に実行したり、短い待ち時間 \(比較的短い間隔\) でメッセージを処理することができます。つまりこれらのコンポーネントは同時に進行または失敗するので、コンポーネント間の分離は提供されません。  
  
 信頼できるセッションでは次の 2 種類のエラーがマスクされます。  
  
-   SOAP メッセージ レベルのエラー \(メッセージの喪失または重複、および送信順序と異なる順序で到着するメッセージを含む\)  
  
-   トランスポート エラー  
  
 信頼できるセッションは、WS\-ReliableMessaging プロトコルとメモリ内転送ウィンドウを実装することにより、トランスポート エラーが発生した場合に SOAP メッセージ レベルのエラーをマスクし、接続を再確立します。  
  
 信頼できるセッションは、TCP が IP パケットに提供する信頼性を SOAP メッセージに提供します。TCP ソケット接続では、ノード間で IP パケットが 1 回のみ、正しい順序で転送されます。信頼できるチャネルでも、これと同じタイプの信頼できる転送が提供されますが、次の点で TCP ソケットの信頼性とは異なります。  
  
-   信頼性は、任意のサイズのバイト パケットに対してではなく、SOAP メッセージ レベルで提供されます。  
  
-   信頼性は、TCP を経由する転送に対してだけでなく、トランスポート中立の形で提供されます。  
  
-   信頼性は、特定のトランスポート セッション \(たとえば、TCP 接続によって提供されるセッション\) に関連付けられておらず、信頼できるセッションの有効期間中、複数のトランスポート セッションを同時にまたは連続して使用できます。  
  
-   信頼できるセッションは、送信側と受信側の SOAP エンドポイント間の接続に必要なトランスポート接続の数に関係なく、両者の間で提供されます。つまり、TCP の信頼性はトランスポート接続が終了した時点で終わるのに対し、信頼できるセッションではエンドツーエンドの信頼性が提供されます。  
  
## 信頼できるセッションとバインディング  
 前述のように、信頼できるセッションはトランスポート中立なので、多数のトランスポート上で実装できます。また、信頼できるセッションは、要求\/応答や双方向など複数のメッセージ交換パターンで確立できます。このため [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] の信頼できるセッションは、一連のバインディングに対するプロパティの 1 つとして公開されています。  
  
 信頼できるセッションは、次のバインディングを使用するエンドポイントで使用できます。  
  
-   HTTP ベース トランスポートの標準バインディング  
  
    -   `WsHttpBinding` : 要求\/応答または一方向のコントラクトを公開します。  
  
    -   要求\/応答または一方向のサービス コントラクトで信頼できるセッションを使用しているときに使用できます。  
  
    -   `WsDualHttpBinding` : 双方向、要求\/応答、または一方向のコントラクトを公開します。  
  
    -   `WsFederationHttpBinding` : 要求\/応答または一方向のコントラクトを公開します。  
  
-   TCP ベース トランスポートの標準バインディング  
  
    -   `NetTcpBinding` : 双方向、要求\/応答、または一方向のコントラクトを公開します。  
  
 信頼できるセッションは、HTTPS \(問題点の詳細については、後述の「信頼できるセッションとセキュリティ」を参照\) や名前付きパイプ バインディングなどのカスタム バインディングを作成することによって、他のバインディングでも使用できます。  
  
 信頼できるセッションは、多様な種類のチャネルに重ねることができ、その結果形成された信頼できるセッションのチャネル形状はそれぞれ異なります。クライアントとサーバーの両方でサポートされる、信頼できるセッション チャネルの種類は、使用される土台のチャネルの種類によって異なります。次の表では、基になるチャネルの種類ごとに、クライアントでサポートされるセッション チャネルの種類を示します。  
  
|サポートされる信頼できるセッション チャネルの種類 \(基になるチャネルの種類でサポートされる `TChannel` の値\)|`IRequestChannel`|`IRequestSessionChannel`|`IDuplexChannel`|`IDuplexSessionChannel`|  
|---------------------------------------------------------------------|-----------------------|------------------------------|----------------------|-----------------------------|  
|`IOutputSessionChannel`|○|○|○|○|  
|`IRequestSessionChannel`|○|○|×|×|  
|`IDuplexSessionChannel`|×|×|○|○|  
  
 サポートされるチャネルの種類は、<xref:System.ServiceModel.Channels.ReliableSessionBindingElement.BuildChannelFactory%60%601%28System.ServiceModel.Channels.BindingContext%29> メソッドに渡されるジェネリック パラメーター `TChannel` に使用できる値です。  
  
 次の表では、基になるチャネルの種類ごとに、サーバーでサポートされるセッション チャネルの種類を示します。  
  
|サポートされる信頼できるセッション チャネルの種類 \(基になるチャネルの種類でサポートされる `TChannel` の値\)|`IReplyChannel`|`IReplySessionChannel`|`IDuplexChannel`|`IDuplexSessionChannel`|  
|---------------------------------------------------------------------|---------------------|----------------------------|----------------------|-----------------------------|  
|`IInputSessionChannel`|○|○|○|○|  
|`IReplySessionChannel`|○|○|×|×|  
|`IDuplexSessionChannel`|×|×|○|○|  
  
 サポートされるチャネルの種類は、<xref:System.ServiceModel.Channels.ReliableSessionBindingElement.BuildChannelListener%60%601%28System.ServiceModel.Channels.BindingContext%29> メソッドに渡されるジェネリック パラメーター `TChannel` に使用できる値です。  
  
## 信頼できるセッションとセキュリティ  
 双方 \(サービスとクライアント\) が認証され、交換されるメッセージが改ざんされない環境で通信を行うには、信頼できるセッションをセキュリティで保護することが重要です。さらに、個別の信頼できるセッションの整合性を確保することも重要です。信頼できるセッションのセキュリティ保護は、セキュリティ セッション チャネルで管理および提示されるセキュリティ コンテキストにバインドすることによって行います。セキュリティ チャネルによって、セキュリティ セッションが可能になります。セッション確立中に交換されるセキュリティ トークンは、信頼できるセッションでメッセージをセキュリティで保護するために使用されます。  
  
 信頼できるセッションが TCP\-S を経由する場合、信頼できるセッションに TCP セッションが関連付けられます。このため、トランスポート セキュリティによって、信頼できるセッションにセキュリティが確実に関連付けられます。この場合、接続の再確立は無効になります。  
  
 唯一の例外は、HTTPS の使用時です。SSL \(Secure Sockets Layer\) セッションは信頼できるセッションにバインドされません。この場合、セキュリティ コンテキスト \(SSL セッション\) を共有しているセッションどうしは相互に保護されないので、脅威につながります。アプリケーションによっては現実的な脅威となる可能性もあります。  
  
## 信頼できるセッションの使用  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] の信頼できるセッションを使用するには、信頼できるセッションをサポートするバインディングを使用してエンドポイントを作成します。[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] によって提供される、信頼できるセッションが有効化されたシステム指定のバインディングの 1 つを使用するか、信頼できるセッションを有効にしたカスタム バインディングを作成します。信頼できるセッションが既定でサポートおよび有効化されているシステム定義のバインディングは、次のとおりです。  
  
-   <xref:System.ServiceModel.WSDualHttpBinding>  
  
 信頼できるセッションがオプションとしてサポートされ、既定では有効化されていないシステム指定のバインディングは、次のとおりです。  
  
-   <xref:System.ServiceModel.WSHttpBinding>  
  
-   <xref:System.ServiceModel.WSFederationHttpBinding>  
  
-   <xref:System.ServiceModel.NetTcpBinding>  
  
 カスタム バインディングを作成する方法の例については、「[方法 : カスタムの信頼できるセッションによる HTTPS を使用したバインディングを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-reliable-session-binding-with-https.md)」を参照してください。  
  
 信頼できるセッションをサポートする [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] バインディングの詳細については、「[システム標準のバインディング](../../../../docs/framework/wcf/system-provided-bindings.md)」を参照してください。  
  
## 信頼できるセッションを使用する状況  
 アプリケーションで信頼できるセッションの使用が求められる状況を理解することが重要です。[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] では、双方のエンドポイントがどちらもアクティブである場合に、信頼できるセッションがサポートされます。アプリケーションで、エンドポイントの一方が一定期間使用できないことが必要である場合は、信頼性を実現するためにキューを使用できます。  
  
 2 つのエンドポイントを TCP 経由で接続する必要がある場合、TCP ではパケットが正しい順序で 1 回だけ届くことが保証されているため、信頼できるメッセージ交換を行うために信頼できるセッションを使用する必要はなく、TCP だけで十分な場合があります。  
  
 ただし、次のいずれかの特性があるシナリオでは、信頼できるセッションの使用を真剣に検討する必要があります。  
  
-   SOAP ルーターなどの SOAP 中継局  
  
-   プロキシ中継局またはトランスポート ブリッジ  
  
-   断続的な接続  
  
-   HTTP を経由するセッション  
  
## 参照  
 [サービスとクライアントを構成するためのバインディングの使用](../../../../docs/framework/wcf/using-bindings-to-configure-services-and-clients.md)   
 [WS 信頼できるセッション](../../../../docs/framework/wcf/samples/ws-reliable-session.md)