---
title: "セキュリティ イベントの監査 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "セキュリティ イベントの監査 [WCF]"
ms.assetid: 5633f61c-a3c9-40dd-8070-1c373b66a716
caps.latest.revision: 27
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 27
---
# セキュリティ イベントの監査
[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] で作成されたアプリケーションでは、監査機能を使用してセキュリティ イベント \(成功、失敗、またはその両方\) をログに記録できます。これらのイベントは Windows システム イベント ログに書き込まれ、イベント ビューアーを使用して確認できます。  
  
 監査を使用すると、管理者は既に発生した攻撃や現在進行中の攻撃を検出できます。また、開発者がセキュリティ関連の問題をデバッグする際にも役立ちます。たとえば、認証またはポリシー チェックの構成エラーによって承認済みユーザーへのアクセスが拒否された場合、開発者は、イベント ログを検査することによって、このエラーの原因をすばやく発見し、取り出すことができます。  
  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のセキュリティ[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)」を参照してください。[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のプログラミング[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[基本的な WCF プログラミング](../../../../docs/framework/wcf/basic-wcf-programming.md)」を参照してください。  
  
## 監査レベルと動作  
 セキュリティ監査には次の 2 つのレベルがあります。  
  
-   呼び出し元を承認するサービス承認レベル。  
  
-   [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] がメッセージの有効性をチェックして呼び出し元を認証するメッセージ レベル。  
  
 どちらの監査レベルにおいても、監査の成功または失敗をチェックできます。これは *"監査動作"* と呼ばれます。  
  
## 監査ログの場所  
 監査のレベルと動作を決定したら、ユーザー \(または管理者\) は監査ログの場所を指定できます。監査ログの場所は、Default、Application、および Security の 3 つから選択できます。Default を指定した場合、ログの実際の場所は、ユーザーが使用しているシステムと、セキュリティ ログへの書き込みがサポートされているかどうかによって決まります。[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)]このトピックの「オペレーティング システム」を参照してください。  
  
 セキュリティ ログへの書き込みを行うには、`SeAuditPrivilege` が必要です。既定では、この権限は Local System アカウントと Network Service アカウントだけに与えられています。セキュリティ ログの `read` および `delete` 機能を管理するには、`SeSecurityPrivilege` が必要です。既定では、この権限は管理者だけに与えられています。  
  
 これに対し、アプリケーション ログは認証済みユーザーが読み書きできます。既定では、[!INCLUDE[wxp](../../../../includes/wxp-md.md)] は、監査イベントをアプリケーション ログに書き込みます。すべての認証済みユーザーに表示される個人情報をログに格納することもできます。  
  
## 監査エラーの抑制  
 監査中に監査エラーを表示しないように指定するオプションも用意されています。既定では、監査エラーはアプリケーションに影響を与えません。ただし、必要に応じて、このオプションを `false` に設定し、例外をスローすることもできます。  
  
## プログラミング監査  
 監査動作は、プログラムまたは構成を使用して指定できます。  
  
### 監査クラス  
 監査動作をプログラムで指定するときに使用するクラスとプロパティを次の表で説明します。  
  
|クラス|説明|  
|---------|--------|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>|サービス動作として監査を行うための設定オプションを有効にします。|  
|<xref:System.ServiceModel.AuditLogLocation>|書き込み先のログを指定するための列挙体。指定できる値は、Default、Application、および Security です。Default を選択した場合、実際のログの場所はオペレーティング システムによって決定されます。このトピックの「アプリケーションまたはセキュリティ イベント ログの選択」のセクションを参照してください。|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.MessageAuthenticationAuditLevel%2A>|メッセージ レベルで監査されるメッセージ認証イベントの種類を指定します。`None`、`Failure`、`Success` および `SuccessOrFailure` から選択できます。|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.ServiceAuthorizationAuditLevel%2A>|サービス レベルで監査されるサービス承認イベントの種類を指定します。`None`、`Failure`、`Success` および `SuccessOrFailure` から選択できます。|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A>|監査が失敗した場合に、クライアント要求をどのように処理するかを指定します。たとえば、`SeAuditPrivilege` を持たないサービスがセキュリティ ログへの書き込みを試行したとします。この場合、既定値の `true` が設定されていると、エラーは無視され、クライアント要求は正常に処理されます。|  
  
 監査イベントをログに記録するようにアプリケーションを設定する例については、「[方法 : セキュリティ イベントを監査する](../../../../docs/framework/wcf/feature-details/how-to-audit-wcf-security-events.md)」を参照してください。  
  
### 構成  
 [\<behaviors\>](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md)の下に [\<serviceSecurityAudit\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicesecurityaudit.md)を追加すると、構成を使用して監査動作を指定できます。この要素は、次のコードに示すように、[\<behavior\>](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)の下に指定する必要があります。  
  
```  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <behavior>  
        <!— auditLogLocation="Application" or "Security" -—>  
        <serviceSecurityAudit  
                  auditLogLocation="Application"  
                  suppressAuditFailure="true"  
                  serviceAuthorizationAuditLevel="Failure"  
                  messageAuthenticationAuditLevel="SuccessOrFailure" />   
      </behavior>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 監査が有効になっているが、`auditLogLocation` が指定されていない場合、セキュリティ ログへの書き込みをサポートしているプラットフォームでの既定のログ名は "セキュリティ" ログになります。それ以外の場合は、"アプリケーション" ログになります。 セキュリティ ログへの書き込みをサポートしているオペレーティング システムは [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] と [!INCLUDE[wv](../../../../includes/wv-md.md)] だけです。[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)]このトピックの「オペレーティング システム」を参照してください。  
  
## セキュリティについての考慮事項  
 監査が有効になっていることが悪意のあるユーザーに知られた場合、そのユーザーは監査エントリの書き込みにつながる無効なメッセージを送信する可能性があります。このような方法で監査ログに書き込みが行われると、監査システムに障害が発生します。これを防ぐには、<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> プロパティを `true` に設定し、イベント ビューアーのプロパティを使用して監査動作を制御します。[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)]Windows XP のイベント ビューアーでイベント ログの表示と管理を行う方法については、マイクロソフト サポート オンラインの「[Windows XP のイベント ビューアーでイベント ログの表示と管理を行う方法](http://go.microsoft.com/fwlink/?LinkId=89150)」を参照してください。  
  
 [!INCLUDE[wxp](../../../../includes/wxp-md.md)] のアプリケーション ログに書き込まれた監査イベントは、すべての認証済みユーザーに表示されます。  
  
## アプリケーションまたはセキュリティ イベント ログの選択  
 アプリケーション イベント ログとセキュリティ イベント ログのどちらにログを記録するかを選択するには、次の表の情報を参考にしてください。  
  
#### オペレーティング システム  
  
|システム|アプリケーション ログ|セキュリティ ログ|  
|----------|-----------------|---------------|  
|[!INCLUDE[wxpsp2](../../../../includes/wxpsp2-md.md)] 以降|サポート状況|サポートなし|  
|[!INCLUDE[ws2003sp1](../../../../includes/ws2003sp1-md.md)] および[!INCLUDE[wv](../../../../includes/wv-md.md)]|サポート状況|スレッド コンテキストが `SeAuditPrivilege` を持つ必要があります。|  
  
#### その他の要素  
 オペレーティング システム以外に、ログ記録の使用可能性を制御する設定を次の表に示します。  
  
|要因|アプリケーション ログ|セキュリティ ログ|  
|--------|-----------------|---------------|  
|監査ポリシーの監査|該当しません。|セキュリティ ログは、構成だけでなく、ローカル セキュリティ機関 \(LSA: Local Security Authority\) ポリシーによっても制御されます。\[オブジェクト アクセスの監査\] カテゴリも有効にする必要があります。|  
|既定のユーザー エクスペリエンス|すべての認証済みユーザーがアプリケーション ログに書き込むことができるため、アプリケーション プロセスでは追加のアクセス許可手順は必要ありません。|アプリケーション プロセス \(コンテキスト\) が `SeAuditPrivilege` を持つ必要があります。|  
  
## 参照  
 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>   
 <xref:System.ServiceModel.AuditLogLocation>   
 [セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)   
 [基本的な WCF プログラミング](../../../../docs/framework/wcf/basic-wcf-programming.md)   
 [方法 : セキュリティ イベントを監査する](../../../../docs/framework/wcf/feature-details/how-to-audit-wcf-security-events.md)   
 [\<serviceSecurityAudit\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicesecurityaudit.md)   
 [\<behaviors\>](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md)   
 [Windows Server AppFabric のセキュリティ モデル](http://go.microsoft.com/fwlink/?LinkID=201279&clcid=0x409)