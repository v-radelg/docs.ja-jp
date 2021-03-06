---
title: "&lt;endpointBehaviors&gt; の &lt;behavior&gt; | Microsoft Docs"
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
ms.assetid: b90ca3bc-3c22-4174-b903-e3a39898bd27
caps.latest.revision: 19
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 19
---
# &lt;endpointBehaviors&gt; の &lt;behavior&gt;
エンドポイントの動作設定のコレクションを含む `behavior` 要素。  各動作には、それぞれの `name` によってインデックスが付けられます。  エンドポイントは、この名前を使用して各動作にリンクできます。  [!INCLUDE[netfx40_short](../../../../../includes/netfx40-short-md.md)] 以降では、バインディングおよび動作に名前を付ける必要はありません。  既定の構成、および名前のないバインディングと動作の詳細については、「[簡略化された構成](../../../../../docs/framework/wcf/simplified-configuration.md)」および「[WCF サービスの簡略化された構成](../../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md)」を参照してください。  
  
## 構文  
  
```  
  
<system.ServiceModel>  
  <behaviors>  
    <endpointBehaviors>  
       <behavior name="String" />  
    </endpointBehaviors>  
  </behaviors>  
</system.ServiceModel>  
```  
  
## 属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### 属性  
  
|属性|説明|  
|--------|--------|  
|name|動作の構成名を含む一意の文字列。  この値は、要素の識別文字列として機能するため、一意のユーザー定義の文字列である必要があります。  [!INCLUDE[netfx40_short](../../../../../includes/netfx40-short-md.md)] 以降では、バインディングおよび動作に名前を付ける必要はありません。  既定の構成、および名前のないバインディングと動作の詳細については、「[簡略化された構成](../../../../../docs/framework/wcf/simplified-configuration.md)」および「[WCF サービスの簡略化された構成](../../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md)」を参照してください。|  
  
### 子要素  
  
|要素|説明|  
|--------|--------|  
|[\<clientCredentials\>](../../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)|サービスに対するクライアントの認証に使用される資格情報を指定します。|  
|[\<callbackDebug\>](../../../../../docs/framework/configure-apps/file-schema/wcf/callbackdebug.md)|[!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)] コールバック オブジェクトのサービス デバッグを指定します。|  
|[\<callbackTimeouts\>](../../../../../docs/framework/configure-apps/file-schema/wcf/callbacktimeouts.md)|クライアント コールバックのタイムアウトを指定します。|  
|[\<clientVia\>](../../../../../docs/framework/configure-apps/file-schema/wcf/clientvia.md)|メッセージの経路を指定します。|  
|[\<dataContractSerializer\>](../../../../../docs/framework/configure-apps/file-schema/wcf/datacontractserializer.md)|DataContractSerializer 用の構成データが含まれています。|  
|[\<dispatcherSynchronization\>](../../../../../docs/framework/configure-apps/file-schema/wcf/dispatchersynchronization.md)|サービスが非同期に応答を返すことができるようにするエンドポイントの動作を指定します。|  
|[\<enableWebScript\>](../../../../../docs/framework/configure-apps/file-schema/wcf/enablewebscript.md)|ASP.NET AJAX Web ページからサービスを使用できるようにするエンドポイントの動作を有効にします。  この動作は、必ず \<webHttpBinding\> 標準バインディングまたは \<webMessageEncoding\> バインディング要素と組み合わせて使用する必要があります。|  
|[\<endpointDiscovery\>](../../../../../docs/framework/configure-apps/file-schema/wcf/endpointdiscovery.md)|エンドポイントのさまざまな探索設定を指定します \(探索可能性、スコープ、メタデータに対するカスタム拡張など\)。|  
|[\<soapProcessing\>](../../../../../docs/framework/configure-apps/file-schema/wcf/soapprocessing.md)|異なるバインディングの種類およびメッセージ バージョンの間でメッセージのマーシャリングに使用されるクライアント エンドポイントの動作を定義します。|  
|[\<synchronousReceive\>](../../../../../docs/framework/configure-apps/file-schema/wcf/synchronousreceive-element.md)|サービスまたはクライアント アプリケーションでメッセージを受信する場合のランタイム動作を指定します。  属性や子要素はありません。|  
|[\<transactedBatching\>](../../../../../docs/framework/configure-apps/file-schema/wcf/transactedbatching.md)|受信操作でトランザクション バッチがサポートされるかどうかを指定します。|  
|[\<webHttp\>](../../../../../docs/framework/configure-apps/file-schema/wcf/webhttp.md)|構成によってエンドポイントに WebHttpBehavior を指定します。  この動作を \<webHttpBinding\> 標準バインディングと組み合わせて使用すると、[!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] サービスの Web プログラミング モデルが有効になります。|  
  
### 親要素  
  
|要素|説明|  
|--------|--------|  
|[\<endpointBehaviors\>](../../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md)|エンドポイント動作要素のコレクション。|