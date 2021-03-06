---
title: "Object Identity | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: c788f2f9-65cc-4455-9907-e8388a268e00
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# Object Identity
実行時のオブジェクトは、一意の ID を持ちます。  2 つの変数が同じオブジェクトを参照している場合、実際それらの変数は、そのオブジェクトの同じインスタンスを参照しています。  したがって、一方の変数から加えた変更は、もう一方の変数から直ちに参照できます。  
  
 リレーショナル データベースのテーブルの行は、一意の ID を持ちません。  それぞれの行は一意の主キーを持つので、同じキー値を持つ行が存在することはありません。  しかしこれは、データベース テーブルの中だけでの制限です。  
  
 実際には、データベースの外にデータを取り出して、別の層に移し、そこでアプリケーションが処理を行うのが普通です。  [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] がサポートするのはこのモデルです。  データベースからデータを行として取り出す場合、同じデータを表す 2 つの行が、実際に同じ行インスタンスに対応付けられることは求めません。  ある特定の顧客についてのクエリを 2 回行うと、2 行のデータが取得されます。どちらの行も、内容は同じ情報です。  
  
 オブジェクトの場合、求める動作は大きく異なります。  <xref:System.Data.Linq.DataContext> に対して同じ情報を繰り返し要求した場合、同じオブジェクト インスタンスが返されることを期待します。  そのような動作を期待するのは、オブジェクトはアプリケーションにとって特別な意味を持つものであり、オブジェクトらしい動作が求められるためです。  オブジェクトは階層やグラフとして設計されています。  そして、そのように取得されることを期待します。同じデータを何回か要求したからといって、レプリケートされたインスタンスをいくつも返されることは期待しません。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] では、オブジェクト ID は <xref:System.Data.Linq.DataContext> によって管理されます。  データベースから新しい行を取得すると、主キーに基づいてその行が ID テーブルに記録され、新しいオブジェクトが作成されます。  それと同じ行を取得した場合、最初のオブジェクト インスタンスがアプリケーションに返されます。  <xref:System.Data.Linq.DataContext> は、このような方法で、データベースにおける ID の概念 \(つまり主キー\) を、言語における ID の概念 \(つまりインスタンス\) に置き換えます。  アプリケーションが参照するオブジェクトは、常に最初に取得した状態です。  新しいデータが異なる場合、破棄されます。  詳細については、「[Retrieving Objects from the Identity Cache](../../../../../../docs/framework/data/adonet/sql/linq/retrieving-objects-from-the-identity-cache.md)」を参照してください。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、オプティミスティック更新をサポートするために、この方法を使用してローカル オブジェクトの整合性を維持します。  オブジェクトが最初に作成された後で生じた変更は、アプリケーションが加えた変更だけなので、アプリケーションの意図は明確です。  その間に外部の者によって変更が加えられた場合、`SubmitChanges()` を呼び出した時点で把握されます。  
  
> [!NOTE]
>  クエリで要求されたオブジェクトが、既に取得済みであることが簡単に判別できる場合、クエリは実行されません。  ID テーブルは、それまでに取得した全オブジェクトのキャッシュの役割を果たします。  
  
## 例  
  
### オブジェクトのキャッシュの例 1  
 この例では、同じクエリを 2 回実行した場合、メモリ内にある同じオブジェクトへの参照をそのつど受け取ります。  
  
 [!code-csharp[DLinqObjectIdentity#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqObjectIdentity/cs/Program.cs#1)]
 [!code-vb[DLinqObjectIdentity#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqObjectIdentity/vb/Module1.vb#1)]  
  
### オブジェクトのキャッシュの例 2  
 この例では、データベースの同じ行を返す、異なるクエリを実行した場合、メモリ内にある同じオブジェクトへの参照をそのつど受け取ります。  
  
 [!code-csharp[DLinqObjectIdentity#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqObjectIdentity/cs/Program.cs#2)]
 [!code-vb[DLinqObjectIdentity#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqObjectIdentity/vb/Module1.vb#2)]  
  
## 参照  
 [Background Information](../../../../../../docs/framework/data/adonet/sql/linq/background-information.md)