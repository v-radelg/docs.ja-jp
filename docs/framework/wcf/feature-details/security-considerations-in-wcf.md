---
title: "WCF でのセキュリティの考慮事項 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "セキュリティ [WCF]"
  - "WCF, セキュリティ"
  - "Windows Communication Foundation, セキュリティ"
ms.assetid: 42055ee0-6d0c-443d-9d89-788dfc345d6d
caps.latest.revision: 49
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 49
---
# WCF でのセキュリティの考慮事項
このセクションの各トピックでは、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] アプリケーションの設計時に考慮する必要のあるセキュリティ関連の各種項目を示します。  
  
## このセクションの内容  
 [情報の漏えい](../../../../docs/framework/wcf/feature-details/information-disclosure.md)  
 情報が開示または攻撃されるさまざまな方法、およびそれを軽減する方法について説明します。  
  
 [権限の昇格](../../../../docs/framework/wcf/feature-details/elevation-of-privilege.md)  
 最初に付与されたものよりも高い承認アクセス許可を攻撃者に与えることの影響、およびそれを軽減する方法について説明します。  
  
 [サービス拒否](../../../../docs/framework/wcf/feature-details/denial-of-service.md)  
 システムがメッセージを適切に処理できない場合の影響、およびそれを軽減する方法について説明します。  
  
 [改変](../../../../docs/framework/wcf/feature-details/tampering.md)  
 メッセージの改ざんやメッセージの配信先の変更、およびそれを軽減する方法について説明します。  
  
 [リプレイ攻撃](../../../../docs/framework/wcf/feature-details/replay-attacks.md)  
 攻撃者がメッセージのストリームを送受信者間でコピーし、そのストリームを 1 つ以上の第三者に対してリプレイすることによる影響、およびそれを軽減する方法について説明します。  
  
 [セキュリティで保護されたセッションに関するセキュリティの検討](../../../../docs/framework/wcf/feature-details/security-considerations-for-secure-sessions.md)  
 セキュリティで保護されたセッションを実装する場合に、セキュリティに影響を及ぼす次の項目について説明します。  
  
 [サポートされていないシナリオ](../../../../docs/framework/wcf/feature-details/unsupported-scenarios.md)  
 セキュリティの特定の側面がサポートされないため、避けたり配慮したりする必要のあるさまざまなシナリオを示します。  
  
## 関連項目  
 <xref:System.IdentityModel.Tokens>  
  
 <xref:System.IdentityModel.Claims>  
  
 <xref:System.ServiceModel.Security>  
  
 <xref:System.ServiceModel>  
  
## 関連項目  
 [セキュリティ ガイドラインとベスト プラクティス](../../../../docs/framework/wcf/feature-details/security-guidance-and-best-practices.md)  
  
## 参照  
 [セキュリティ](../../../../docs/framework/wcf/feature-details/security.md)