---
title: "永続参加要素 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: f84d2d5d-1c1b-4f19-be45-65b552d3e9e3
caps.latest.revision: 14
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 14
---
# 永続参加要素
永続参加要素は、アプリケーション ホストによってトリガーされる永続化操作 \(保存または読み込み\) に参加できます。[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] には **PersistenceParticipant** および **PersistenceIOParticipant** の 2 つの抽象クラスが付属しており、これらを使用して永続参加要素を作成できます。永続参加要素はこれらのクラスの 1 つから派生し、目的のメソッドを実装して、このクラスのインスタンスを <xref:System.ServiceModel.Activities.WorkflowServiceHost> の <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> コレクションに追加します。アプリケーション ホストは、ワークフロー インスタンスを永続化するときにこのようなワークフロー拡張機能を必要とする場合があり、適切なときに永続参加要素の適切なメソッドを呼び出します。  
  
 次の一覧では、永続化 \(保存\) 操作のさまざまな段階で永続化サブシステムが実行するタスクについて説明します。3 番目および 4 番目の段階で永続参加要素が使用されます。参加要素が IO 参加要素 \(IO 操作にも参加する参加要素\) の場合、その参加要素は 6 番目の段階でも使用されます。  
  
1.  ワークフロー状態、ブックマーク、マップされた変数、およびタイムスタンプなどの組み込みの値を収集します。  
  
2.  ワークフロー インスタンスに関連付けられた拡張コレクションに追加された永続参加要素を収集します。  
  
3.  すべての永続参加要素によって実装された <xref:System.Activities.Persistence.PersistenceParticipant.CollectValues%2A> メソッドを呼び出します。  
  
4.  すべての永続参加要素によって実装された <xref:System.Activities.Persistence.PersistenceParticipant.MapValues%2A> メソッドを呼び出します。  
  
5.  ワークフローを永続ストアに永続化または保存します。  
  
6.  すべての永続 IO 参加要素の <xref:System.Activities.Persistence.PersistenceIOParticipant.BeginOnSave%2A> メソッドを呼び出します。参加要素が IO 参加要素でない場合は、このタスクをスキップします。永続化がトランザクションである場合、Transaction.Current プロパティにトランザクションが提供されます。  
  
7.  すべての永続参加要素が完了するのを待機します。すべての参加要素がインスタンス データの永続化に成功したら、トランザクションをコミットします。  
  
 永続参加要素は **PersistenceParticipant** クラスから派生し、**CollectValues** および **MapValues** メソッドを実装できます。永続 IO 参加要素は **PersistenceIOParticipant** クラスから派生し、**CollectValues** および **MapValues** メソッドの実装に加えて **BeginOnSave** を実装できます。  
  
 それぞれの段階の完了後に次の段階が開始されます。たとえば、最初の段階では、**すべての**永続参加要素から値が収集されます。2 番目の段階では、最初の段階で収集されたすべての値がマップのためにすべての永続参加要素に提供されます。3 番目の段階では、1 番目および 2 番目の段階で収集してマップされたすべての値が永続参加要素に提供されます。  
  
 次の一覧では、読み込み操作のさまざまな段階で永続化サブシステムが実行するタスクについて説明します。4 番目の段階で永続参加要素が使用されます。永続 IO 参加要素 \(IO 操作にも参加する永続参加要素\) は 3 番目の段階でも使用されます。  
  
1.  ワークフロー インスタンスに関連付けられた拡張コレクションに追加された永続参加要素を収集します。  
  
2.  永続ストアからワークフローを読み込みます。  
  
3.  すべての永続 IO 参加要素の <xref:System.Activities.Persistence.PersistenceIOParticipant.BeginOnLoad%2A> を呼び出して、すべての永続参加要素が完了するのを待機します。永続化がトランザクションである場合、Transaction.Current にトランザクションが提供されます。  
  
4.  永続ストアから取得されたデータに基づいて、メモリ内のワークフロー インスタンスを読み込みます。  
  
5.  各永続参加要素の <xref:System.Activities.Persistence.PersistenceParticipant.PublishValues%2A> メソッドを呼び出します。  
  
 永続参加要素は **PersistenceParticipant** クラスから派生し、**PublishValues** メソッドを実装できます。永続 IO 参加要素は **PersistenceIOParticipant** クラスから派生し、**PublishValues** メソッドの実装に加えて **BeginOnLoad** メソッドを実装できます。  
  
 ワークフロー インスタンスを読み込む場合、永続化プロバイダーはそのインスタンスのロックを作成します。これにより、各インスタンスが複数ノードのシナリオでは、複数のホストによって読み込まれることはありません。ロックされたワークフロー インスタンスを読み込もうとすると、次のような例外が表示されます: 例外 「System.ServiceModel.Persistence.InstanceLockException: 要求された操作は、たとえばロック 「00000000\-0000\-0000\-0000\-000000000000'」が取得できなかったため、完了できませんでした。このエラーが発生するのは、次のいずれかが発生した場合です。  
  
-   複数ノード シナリオで、インスタンスが他のホストによって読み込まれます。これらの種類の競合を解決する方法がいくつかあります。ロックと再試行を所有するノードへ処理を転送するか、または他のホストでの作業を保存できないようにする読み込みを強制します。  
  
-   単一ノード シナリオで、ホストがクラッシュしました。ホストが再度起動されるとき \(プロセスのリサイクル、または新しい永続化プロバイダー ファクトリの作成\)、新しいホストが、ロックの期限がまだ切れていないために古いホストによってまだロックされているインスタンスの読み込みを試みます。  
  
-   単一ノード シナリオで、当該インスタンスがあるポイントで中止され、別のホスト ID を持つ新しい永続化プロバイダー インスタンスが作成されます。  
  
 ロックのタイムアウト値の既定値は 5 分です。呼び出したときに、別のタイムアウト値を指定できます <xref:System.ServiceModel.Persistence.PersistenceProvider.Load%2A>。  
  
## このセクションの内容  
  
-   [カスタム永続参加要素を作成する方法](../../../docs/framework/windows-workflow-foundation//how-to-create-a-custom-persistence-participant.md)  
  
## 参照  
 [ストア拡張](../../../docs/framework/windows-workflow-foundation//store-extensibility.md)