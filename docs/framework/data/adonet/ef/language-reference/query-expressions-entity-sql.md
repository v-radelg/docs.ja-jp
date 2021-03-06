---
title: "クエリ式 (Entity SQL) | Microsoft Docs"
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
ms.assetid: c36f327b-e230-48d4-bbd5-78dc6478c447
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# クエリ式 (Entity SQL)
クエリ式とは、さまざまなクエリ演算子を組み合わせて 1 つの構文にしたものです。  [!INCLUDE[esql](../../../../../../includes/esql-md.md)] には、[リテラル](../../../../../../docs/framework/data/adonet/ef/language-reference/literals-entity-sql.md)、[パラメーター](../../../../../../docs/framework/data/adonet/ef/language-reference/parameters-entity-sql.md)、[変数](../../../../../../docs/framework/data/adonet/ef/language-reference/variables-entity-sql.md)、演算子、[関数](../../../../../../docs/framework/data/adonet/ef/language-reference/functions-entity-sql.md)、set 演算子などのさまざまな種類の式が用意されています。  詳細については、「[Entity SQL リファレンス](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)」を参照してください。  
  
## 句  
 クエリ式は、オブジェクトのコレクションに連続した操作を適用する一連の句で構成されます。  それらは [SELECT](../../../../../../docs/framework/data/adonet/ef/language-reference/select-entity-sql.md)、[FROM](../../../../../../docs/framework/data/adonet/ef/language-reference/from-entity-sql.md)、[WHERE](../../../../../../docs/framework/data/adonet/ef/language-reference/where-entity-sql.md)、[GROUP BY](../../../../../../docs/framework/data/adonet/ef/language-reference/group-by-entity-sql.md)、[HAVING](../../../../../../docs/framework/data/adonet/ef/language-reference/having-entity-sql.md)、および [ORDER BY](../../../../../../docs/framework/data/adonet/ef/language-reference/order-by-entity-sql.md) など、標準の SQL SELECT ステートメントにあるものと同じ句に基づきます。  
  
## スコープ  
 FROM 句で定義された名前は、出現順 \(左から右の順\) に FROM スコープに導入されます。  JOIN リストでは、式は既にリストで定義されている名前を参照できます。  FROM 句で指定された要素のパブリック プロパティは FROM スコープには追加されません。それらは常に別名で修飾された名前で参照する必要があります。  通常は、SELECT 式のすべての部分が FROM スコープに含まれると見なされます。  
  
## 参照  
 [Entity SQL リファレンス](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)