---
title: "診断トレース  | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 28e77a63-d20d-4b6a-9caf-ddad86550427
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# 診断トレース 
トレースとは、アプリケーションの実行中に生成される特定のメッセージの発行です。トレースを使用するときには、送信されたメッセージを収集して記録するための機構が必要です。トレース メッセージは "リスナー" によって受け取られます。リスナーの目的は、トレース メッセージの収集、格納、およびルーティングを行うことです。リスナーにより、トレース出力が適切な場所 \(ログ、ウィンドウ、またはテキスト ファイル\) に送られます。  
  
 トレースを有効にすると、こうしたリスナーの 1 つである <xref:System.Diagnostics.DefaultTraceListener> が自動的に作成および初期化されます。トレース出力を別のソースに送るには、別のトレース リスナーを作成して初期化する必要があります。作成するリスナーには、個別の要求が反映されている必要があります。たとえば、すべてのトレース出力のテキスト レコードが必要である場合は、有効になったときにすべての出力を新しいテキスト ファイルに書き込むリスナーを作成します。また、アプリケーションの実行時に出力を表示するだけでよい場合は、すべての出力をコンソール ウィンドウに送るリスナーを作成します。<xref:System.Diagnostics.EventLogTraceListener> はイベント ログにトレース出力を転送し、<xref:System.Diagnostics.TextWriterTraceListener> はストリームにトレース出力を書き込みます。  
  
## トレースの有効化  
 トランザクション処理中にトレースを有効にするには、アプリケーションの構成ファイルを編集する必要があります。次に例を示します。  
  
```  
<configuration>  
<system.diagnostics>  
     <sources>  
          <source name="System.Transactions" switchValue="Warning">  
               <listeners>  
                    <add name="tx"   
                     type="System.Diagnostics.XmlWriterTraceListener"   
                     initializeData= "tx.log" />  
               </listeners>  
          </source>  
     </sources>  
</system.diagnostics>  
</configuration>  
  
```  
  
 <xref:System.Transactions> トレースは、"System.Transactions" という名前のソースに書き込まれます。`add` を使用して、使用するトレース リスナーの名前と種類を指定できます。この例の構成では、リスナーに "tx" という名前を付け、使用する種類として標準の .NET Framework トレース リスナー \(<xref:System.Diagnostics.XmlWriterTraceListener>\) を追加しています。`initializeData` を使用して、リスナーのログ ファイルの名前を設定します。さらに、単純なファイル名と完全修飾パスを置き換えることができます。  
  
 各トレース メッセージの種類は、その重大度を示すレベルに割り当てられます。アプリケーション ドメインのトレース レベルがイベント タイプのレベル以下である場合、そのメッセージが生成されます。トレース レベルは、構成ファイルの `switchValue` 設定で制御します。次の表に、診断トレース メッセージに関連するレベルの定義を示します。  
  
|トレース レベル|説明|  
|--------------|--------|  
|Critical \(重大\)|次のような重大な障害が発生しました。<br /><br /> -   ユーザー機能が即座に失われる可能性のあるエラー。<br />-   機能が失われるのを防ぐために管理者が対処する必要のあるイベント。<br />-   コードのハング。<br />-   このトレース レベルでは、他の重大なトレースを解釈するための十分なコンテキストも提供できます。これは、重大なエラーにつながる操作シーケンスの特定に役立ちます。|  
|Error \(エラー\)|ユーザー機能の損失につながるエラー \(無効な設定、ネットワークの動作など\) が発生しました。|  
|Warning \(警告\)|後続の処理でエラーまたは重大なエラーが起こる可能性のある状態が存在します \(割り当ての失敗、制限への接近など\)。ユーザー コードからのエラーの通常処理 \(トランザクションの中止、タイムアウト、認証の失敗など\) が警告を生成する場合もあります。|  
|Information \(情報\)|システム ステータスの監視と診断、パフォーマンスの計測、またはプロファイリングに有用なメッセージが生成されます。これには、トランザクションの作成またはコミット、重要な境界の超過、重要なリソースの割り当てなど、トランザクションと参加の有効期間イベントが含まれる場合があります。このような情報は、後で開発者が容量設計やパフォーマンス管理に利用できます。|  
  
## トレース コード  
 次の表は、<xref:System.Transactions> インフラストラクチャで生成されるトレース コードの一覧です。この表には、トレース コード識別子、トレースの <xref:System.Diagnostics.EventTypeFilter.EventType%2A> 列挙レベル、およびトレースの **TraceRecord** に含まれる追加データが示されています。さらに、そのトレースに対応するトレース レベルも  **TraceRecord** に保存されます。  
  
|TraceCode|EventType|TraceRecord の追加データ|  
|---------------|---------------|------------------------|  
|TransactionCreated|情報|TransactionTraceId|  
|TransactionPromoted|情報|Local TransactionTraceId、Distributed TransactionTraceId|  
|EnlistmentCreated|情報|TransactionTraceId、EnlistmentTraceId、EnlistmentType \(durable\/volatile\)、EnlistmentOptions|  
|EnlistmentCallbackNegative|警告|TransactionTraceId、EnlistmentTraceId、<br /><br /> Callback \(forcerollback\/aborted\/indoubt\)|  
|TransactionRollbackCalled|警告|TransactionTraceId|  
|TransactionAborted|警告|TransactionTraceId|  
|TransactionInDoubt|警告|TransactionTraceId|  
|TransactionScopeCreated|情報|TransactionScopeResult \(次のようになります\)<br /><br /> -   新しいトランザクション。<br />-   渡されたトランザクション。<br />-   渡された依存トランザクション。<br />-   現在のトランザクションを使用。<br />-   トランザクションなし。<br /><br /> 新しい現在の TransactionTraceId|  
|TransactionScopeDisposed|情報|スコープの "想定される" 現在のトランザクションの TransactionTraceId。|  
|TransactionScopeIncomplete|警告|スコープの "想定される" 現在のトランザクションの TransactionTraceId。|  
|TransactionScopeNestedIncorrectly|警告|スコープの "想定される" 現在のトランザクションの TransactionTraceId。|  
|TransactionScopeCurrentTransactionChanged|警告|古い \(現在の\) TransactionTraceId、他の TransactionTraceId|  
|TransactionScopeTimeout|警告|スコープの "想定される" 現在のトランザクションの TransactionTraceId。|  
|DependentCloneCreated|情報|TransactionTraceId、作成される依存トランザクションの種類 \(RollbackIfNotComplete\/BlockCommitUntilComplete\)|  
|DependentCloneComplete|情報|TransactionTraceId|  
|RecoveryComplete|情報|リソース マネージャー GUID \(ベースから\)|  
|Reenlist|情報|リソース マネージャー GUID \(ベースから\)|  
|TransactionSerialized|情報|TransactionTraceId|  
|TransactionException|エラー|例外メッセージ|  
|InvalidOperationException|エラー|例外メッセージ|  
|InternalError|重大|例外メッセージ|  
|TransferEvent||トランザクションが逆シリアル化されるか、または <xref:System.Transactions> トランザクションから分散トランザクションに昇格される場合、ExecutionContext の現在の ActivityID および分散トランザクション ID が書き込まれます。<br /><br /> DTC がマネージ コードにコールバックする場合、コールバックの間、分散トランザクション ID が ExecutionContext の ActivityID として設定されます。|  
|ConfiguredDefaultTimeoutAdjusted|警告|追加データなし|  
|TransactionTimeout|警告|タイムアウトするトランザクションの TransactionTraceId|  
  
 上の各追加データ項目に対する XML スキーマの形式は次のとおりです。  
  
### TransactionTraceIdentifier  
 `<TransactionTraceIdentifier>`  
  
 `<TransactionIdentifier >`  
  
 `string representation of transaction id`  
  
 `</TransactionIdentifier>`  
  
 `< CloneIdentifier >`  
  
 `the clone id number`  
  
 `</CloneIdentifier>`  
  
 `</TransactionTraceIdentifier>`  
  
### EnlistmentTraceIdentifier  
 `<EnlistmentTraceIdentifier>`  
  
 `<ResourceManagerId>`  
  
 `string form of guid`  
  
 `</ResourceManagerId>`  
  
 `<TransactionTraceIdentifier>`  
  
 `<TransactionIdentifier >`  
  
 `string representation of transaction id`  
  
 `</TransactionIdentifier>`  
  
 `<CloneIdentifier >`  
  
 `the clone id number`  
  
 `</CloneIdentifier>`  
  
 `<TransactionTraceIdentifier>`  
  
 `<EnlistmentIdentifier>`  
  
 `the enlistment id number`  
  
 `</EnlistmentIdentifier>`  
  
 `</EnlistmentTraceIdentifier>`  
  
### リソース マネージャー識別子  
 `<ResourceManagerId>`  
  
 `string form of guid`  
  
 `</ResourceManagerId>`  
  
## トレースのセキュリティに関する問題  
 管理者がトレース機能を有効にすると、既定で、パブリックに表示可能なトレース ログに機密情報が書き込まれる可能性があります。潜在的なセキュリティの脅威を緩和するため、共有およびファイル システムのアクセス許可によって管理される安全な場所にトレース ログを保存することを考慮する必要があります。