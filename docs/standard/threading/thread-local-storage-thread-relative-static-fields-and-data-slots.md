---
title: "Thread Local Storage: Thread-Relative Static Fields and Data Slots | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "threading [.NET Framework], local storage"
  - "threading [.NET Framework], thread-relative static fields"
  - "local thread storage"
  - "TLS"
ms.assetid: c633a4dc-a790-4ed1-96b5-f72bd968b284
caps.latest.revision: 13
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 13
---
# Thread Local Storage: Thread-Relative Static Fields and Data Slots
1 つのスレッドとアプリケーション ドメインに固有のデータを格納するには、マネージ スレッド ローカル ストレージ \(TLS: Thread Local Storage\) を使用します。  .NET Framework は、マネージ TLS の使用に関して、スレッド相対静的フィールドとデータ スロットという 2 つの機構を備えています。  
  
-   コンパイル時に要件を正確に予測できる場合は、スレッド相対静的フィールド \(Visual Basic ではスレッド相対 `Shared` フィールド\) を使用します。  スレッド相対静的フィールドは、最適なパフォーマンスを提供します。  また、コンパイル時に型チェックを利用することもできます。  
  
-   実行時にならないと実際の要件がわからない場合は、データ スロットを使用します。  データ スロットは、スレッド相対静的フィールドと比べると低速で使いにくく、データは <xref:System.Object> 型として格納されるため、使用前に適切な型にキャストする必要があります。  
  
 アンマネージ C\+\+ では、`TlsAlloc` を使用してスロットを動的に割り当て、`__declspec(thread)` で変数をスレッド相対ストレージに割り当てることを宣言します。  スレッド相対静的フィールドおよびデータ スロットには、この動作のマネージ バージョンが用意されています。  
  
 [!INCLUDE[net_v40_long](../../../includes/net-v40-long-md.md)] では、<xref:System.Threading.ThreadLocal%601?displayProperty=fullName> クラスを使用して、最初に利用されるときに初期化されるスレッドローカル オブジェクトを作成できます。  詳細については、「[限定的な初期化](../../../docs/framework/performance/lazy-initialization.md)」を参照してください。  
  
## マネージ TLS でのデータの一意性  
 スレッド相対静的フィールドとデータ スロットのどちらを使用しても、マネージ TLS でのデータはスレッドとアプリケーション ドメインの組み合わせに対して一意になります。  
  
-   アプリケーション ドメインでは、両方のスレッドが同じフィールドまたはスロットを使用していたとしても、一方のスレッドが、もう一方のスレッドからのデータを変更することはできません。  
  
-   あるスレッドが、複数のアプリケーション ドメインで使用される 1 つのフィールドまたはスロットにアクセスする場合、値はアプリケーション ドメインごとに維持されます。  
  
 たとえば、スレッドがスレッド相対静的フィールドの値を設定し、別のアプリケーション ドメインに移動して、設定したフィールドの値を取得した場合、2 番目のアプリケーション ドメインで取得した値は、最初のアプリケーション ドメインでの値とは異なります。  2 番目のアプリケーション ドメインのフィールドに新しい値を設定しても、最初のアプリケーション ドメインにあるフィールドの値には影響しません。  
  
 同様に、スレッドが、異なる 2 つのアプリケーション ドメインで 2 つの名前付きデータ スロットを取得した場合、最初のアプリケーション ドメインのデータと 2 番目のアプリケーション ドメインのデータは、それぞれ独立性を確保します \(無関係な状態が維持されます\)。  
  
## スレッド相対静的フィールド  
 スレッドとアプリケーション ドメインの組み合わせに対し、データの一部が常に一意であることがわかっている場合は、静的フィールドに <xref:System.ThreadStaticAttribute> 属性を適用します。  フィールドの使用法は、他の静的フィールドと同様です。  フィールドのデータは、使用する各スレッドに対して一意です。  
  
 スレッド相対静的フィールドは、データ スロットと比較してパフォーマンスが優れており、コンパイル時の型チェックも可能です。  
  
 すべてのクラス コンストラクター コードはそのフィールドにアクセスする最初のコンテキストの最初のスレッドで実行されることに注意してください。  同じアプリケーション ドメイン内にある、その他のスレッドまたはコンテキストについては、これらのフィールドが参照型の場合は `null` \(Visual Basic では `Nothing`\) に、値型の場合は既定値に初期化されます。  したがって、クラス コンストラクターを使用してスレッド相対静的フィールドを初期化しないでください。  スレッド相対静的フィールドの初期化を回避し、これらのフィールドは `null` \(`Nothing`\) または既定値に初期化されると仮定します。  
  
## データ スロット  
 .NET Framework には、スレッドとアプリケーション ドメインの組み合わせに対して一意の、動的なデータ スロットが用意されています。  このデータ スロットには 2 つのタイプがあります。名前付きスロットと名前のないスロットです。  いずれも、<xref:System.LocalDataStoreSlot> 構造体を使用して実装されます。  
  
-   名前付きデータ スロットを作成するには、<xref:System.Threading.Thread.AllocateNamedDataSlot%2A?displayProperty=fullName> メソッドまたは <xref:System.Threading.Thread.GetNamedDataSlot%2A?displayProperty=fullName> メソッドを使用します。  既存の名前付きスロットへの参照を取得するには、その名前を <xref:System.Threading.Thread.GetNamedDataSlot%2A> メソッドに渡します。  
  
-   名前のないデータ スロットを作成するには、<xref:System.Threading.Thread.AllocateDataSlot%2A?displayProperty=fullName> メソッドを使用します。  
  
 名前付きスロットと名前のないスロットはいずれも、<xref:System.Threading.Thread.SetData%2A?displayProperty=fullName> メソッドと <xref:System.Threading.Thread.GetData%2A?displayProperty=fullName> メソッドを使用して、スロット内のデータの設定や取得を行います。  これらは静的メソッドであり、現在実行中であるスレッドのデータを操作する役割を果たします。  
  
 名前付きスロットは、必要なときに <xref:System.Threading.Thread.GetNamedDataSlot%2A> メソッドにスロットの名前を渡してスロットを取得できるという利点があります \(名前のないスロットの場合は、スロットへの参照を維持する必要があります\)。  ただし、他のコンポーネントがそのスレッド相対ストレージに対して同じ名前を使用しており、1 つのスレッドが、あるコンポーネントとその他のコンポーネントからコードを実行する場合、この 2 つのコンポーネントが互いのデータを破損する可能性があります \(このシナリオでは、2 つのコンポーネントが同じアプリケーション ドメイン内で実行されており、同じデータを共有するようには設計されていないことを前提としています\)。  
  
## 参照  
 <xref:System.ContextStaticAttribute>   
 <xref:System.Threading.Thread.GetNamedDataSlot%2A?displayProperty=fullName>   
 <xref:System.ThreadStaticAttribute>   
 <xref:System.Runtime.Remoting.Messaging.CallContext>   
 [Threading](../../../docs/standard/threading/index.md)