---
title: "DataSet スキーマの推論プロセスの概要 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: fd0891c8-d068-4e30-a76f-7c375f078bf7
caps.latest.revision: 4
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 4
---
# DataSet スキーマの推論プロセスの概要
推論プロセスでは、まず、テーブルとして推論する XML ドキュメントの要素を決定します。  XML ドキュメントの残りの要素から、それらのテーブルの列が推論によって決定されます。  入れ子状のテーブルの場合は、入れ子になった <xref:System.Data.DataRelation> オブジェクトと <xref:System.Data.ForeignKeyConstraint> オブジェクトが生成されます。  
  
 推論規則について、次に簡単に説明します。  
  
-   属性を持つ要素は、テーブルとして推論されます。  
  
-   子要素を持つ要素は、テーブルとして推論されます。  
  
-   繰り返し出現する要素は、単一のテーブルとして推論されます。  
  
-   ドキュメント \(ルート\) 要素に属性がなく、列として推論される子要素もない場合、その要素は <xref:System.Data.DataSet> として推論されます。  それ以外の場合は、ドキュメント要素はテーブルとして推論されます。  
  
-   属性は、列として推論されます。  
  
-   属性または子要素を持たず、繰り返し出現することもない要素は、列として推論されます。  
  
-   テーブルとして推論される要素が、同じくテーブルとして推論される他の要素の内部に入れ子になっている場合は、その 2 つのテーブル間に入れ子になった **DataRelation** が作成されます。  その場合、**TableName\_Id** という名前の新しい主キー列がその両方のテーブルに追加され、**DataRelation**  によって使用されます。  この **TableName\_Id** 列を使用して、この 2 つのテーブル間に **ForeignKeyConstraint** が作成されます。  
  
-   テーブルとして推論される要素に、テキストは含まれているが子要素は含まれていない場合は、**TableName\_Text** という名前の新しい列が各要素のテキストに作成されます。  テーブルとして推論される要素にテキストだけでなく、子要素もある場合、テキストは無視されます。  
  
## 参照  
 [XML からの DataSet リレーショナル構造の推論](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/inferring-dataset-relational-structure-from-xml.md)   
 [XML からの DataSet の読み込み](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/loading-a-dataset-from-xml.md)   
 [XML の DataSet スキーマ情報の読み込み](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/loading-dataset-schema-information-from-xml.md)   
 [DataSet での XML の使用](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/using-xml-in-a-dataset.md)   
 [DataSets、DataTables、および DataViews](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/index.md)   
 [ADO.NET Managed Providers and DataSet Developer Center \(ADO.NET マネージ プロバイダーと DataSet デベロッパー センター\)](http://go.microsoft.com/fwlink/?LinkId=217917)