---
title: "統合言語クエリ (LINQ) (C#) | Microsoft Docs"
ms.custom: 
ms.date: 02-02-2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 19dd1782-905b-4a9d-a3e9-618453037fa2
caps.latest.revision: 4
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: a97cdc61caf2cd9d1f71a6763e4903bb5a5fdc54
ms.lasthandoff: 03/13/2017

---

# <a name="language-integrated-query-linq"></a>統合言語クエリ (LINQ)

統合言語クエリ (LINQ) は、C# 言語への直接的なクエリ機能の統合に基づくテクノロジのセットの名前です。 これまでは、データに対するクエリは、コンパイル時の型チェックや IntelliSense のサポートがない単純な文字列として表現されてきました。 さらに、SQL データベース、XML ドキュメント、さまざまな Web サービスといったデータ ソースの種類ごとに、異なるクエリ言語を習得する必要がありました。 LINQ では、クエリは、クラス、メソッド、イベントと同様に、ファースト クラスの言語コンストラクトです。

クエリを記述する開発者にとって、最も目立つ LINQ の "統合言語" 部分は、クエリ式です。 クエリ式は、宣言型の "*クエリ構文*" で記述されます。 クエリ構文を使用することで、フィルター処理、並べ替え、グループ化などのデータ ソースに対する操作を、最小限のコードで実行できます。 同一の基本的なクエリ式のパターンを使用して、SQL データベース、ADO .NET データセット、XML ドキュメントとストリーム、および .NET コレクション内のデータを照会して変換できます。

次の例は、完全なクエリ操作を示しています。 完全な操作には、データ ソースの作成、クエリ式の定義、および `foreach` ステートメントでのクエリの実行が含まれています。

[!code-cs[csProgGuideLINQ#11](../../../../../samples/snippets/csharp/concepts/linq/index_1.cs)]

## <a name="query-expression-overview"></a>クエリ式の概要

-   クエリ式を使用して、任意の LINQ 対応データ ソースのデータを照会して変換することができます。 たとえば、1 つのクエリで SQL データベースからデータを取得し、出力として XML ストリームを生成できます。  
  
-   クエリ式は、多くの使い慣れた C# 言語の構成体を使用するため、簡単に習得できます。  
  
-   クエリ式内の変数はすべて厳密に型指定されますが、多くの場合、型はコンパイラが推測できるため、明示的に指定する必要はありません。 詳細については、「[LINQ クエリ操作での型の関係](type-relationships-in-linq-query-operations.md)」を参照してください。  
  
-   クエリは、たとえば `foreach` ステートメントでクエリ変数を反復処理するまで実行されません。 詳細については、「[LINQ クエリの概要](introduction-to-linq-queries.md)」を参照してください。  
  
-   コンパイル時に、クエリ式は、C# 仕様に規定された規則に従って、標準クエリ演算子メソッドの呼び出しに変換されます。 クエリ構文を使用して表現できるすべてのクエリは、メソッド構文でも表現することができます。 ただし、ほとんどの場合、クエリ構文のほうが読みやすく、簡潔です。 詳細については、「[# 言語仕様](../../../language-reference/language-specification.md)」と「[標準クエリ演算子の概要](standard-query-operators-overview.md)」を参照してください。  
  
-   原則として、LINQ クエリを記述するときは、可能であれば常にクエリ構文を使用し、必要な場合にメソッド構文を使用することをお勧めします。 この 2 つの異なる形式の間には、セマンティックの違いもパフォーマンスの違いもありません。 多くの場合、クエリ式のほうが、メソッド構文で記述された同等の式よりも読みやすくなります。  
  
-   一部のクエリ操作 (<xref:System.Linq.Enumerable.Count%2A> や <xref:System.Linq.Enumerable.Max%2A> など) には同等のクエリ式の句がないため、メソッド呼び出しとして表現する必要があります。 メソッド構文は、さまざまな方法でクエリ構文と組み合わせることができます。 詳細については、「[LINQ でのクエリ構文とメソッド構文](query-syntax-and-method-syntax-in-linq.md)」を参照してください。  
  
-   クエリ式は、クエリの適用対象の種類によって、式ツリーまたはデリゲートにコンパイルすることができます。 <xref:System.Collections.Generic.IEnumerable%601> クエリは、デリゲートにコンパイルされます。 <xref:System.Linq.IQueryable> クエリと <xref:System.Linq.IQueryable%601> クエリは、式ツリーにコンパイルされます。 詳細については、「[式ツリー](../../../expression-trees.md)」を参照してください。  

## <a name="next-steps"></a>次のステップ

LINQ の詳細については、最初に「[クエリ式の基本](../../../linq/query-expression-basics.md)」で基本的な概念を理解してから、関心のある LINQ テクノロジのドキュメントを参照してください。   
-   XML ドキュメント: [LINQ to XML](linq-to-xml.md)  
  
-   ADO.NET Entity Framework: [LINQ to entities](http://msdn.microsoft.com/library/641f9b68-9046-47a1-abb0-1c8eaeda0e2d)  
  
-   .NET のコレクション、ファイル、文字列など: [LINQ to Objects ](linq-to-objects.md)

LINQ 全般をより深く理解するには、「[C# での LINQ](../../../linq/linq-in-csharp.md)」を参照してください。

C# での LINQ の使用を開始するには、「[LINQ の使用](../../../tutorials/working-with-linq.md)」チュートリアルを参照してください。



