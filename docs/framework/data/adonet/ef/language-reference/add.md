---
title: "+ (加算) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
ms.assetid: 51769b02-a8f7-4177-9e99-bbd10e77092c
caps.latest.revision: 4
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 4
---
# + (加算)
2 つの値を加算します。  
  
## 構文  
  
```  
  
expression  
+  
expression  
  
```  
  
## 引数  
 `expression`  
 任意の数値データ型の有効な式。  
  
## 戻り値の型  
 2 つの引数の暗黙の型の昇格の結果であるデータ型。 暗黙の型の昇格について詳しくは、「[型システム](../../../../../../docs/framework/data/adonet/ef/language-reference/type-system-entity-sql.md)」をご覧ください。  
  
## 解説  
 EDM.String 型の場合は、値が連結されます。  
  
## 使用例  
 次の Entity SQL クエリでは、\+ 算術演算子を使用して 2 つの数値を加算します。 このクエリは、AdventureWorks Sales Model に基づいています。 このクエリをコンパイルして実行するには、次の手順を実行します。  
  
1.  「[StructuralType 結果を返すクエリの実行方法](../../../../../../docs/framework/data/adonet/ef/how-to-execute-a-query-that-returns-structuraltype-results.md)」の手順に従います。  
  
2.  次のクエリを引数として `ExecuteStructuralTypeQuery` メソッドに渡します。  
  
 [!code-csharp[DP EntityServices Concepts 2#ADD](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#add)]  
  
## 参照  
 [Entity SQL リファレンス](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)   
 [概念モデルの型 \(CSDL\)](http://msdn.microsoft.com/ja-jp/987b995f-e429-4569-9559-b4146744def4)