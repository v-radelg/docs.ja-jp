---
title: "XML の処理オプション | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: 33ced8ee-1745-4e71-8dee-ebe70ec067c7
caps.latest.revision: 5
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 5
---
# XML の処理オプション
XML データの処理に使用できる Microsoft テクノロジの一覧については、次の表を参照してください。  
  
## .NET Framework のオプション  
  
|**オプション**|**処理の種類**|**説明**|  
|---------------|---------------|------------|  
|[LINQ to XML](../../../../ocs/visual-basic/programming-guide/concepts/linq/linq-to-xml.md) <br /> \(<xref:System.Xml.Linq> 名前空間\)|メモリ内|-   .NET Framework の統合言語クエリ \(LINQ\) テクノロジに基づいています。<br />-   オブジェクト、リレーショナル データ、および XML データに対して SQL と同じようにクエリを利用できます。<br />-   直観的なドキュメント作成および変換機能を提供します。<br />-   このオプションは、新しいコードを記述する場合に使用します。|  
|<xref:System.Xml.XmlReader?displayProperty=fullName>|ストリーム ベース|-   高速、非キャッシュ、前方参照専用の XML データ アクセス手段を提供します。<br />-   <xref:System.Xml.XmlReader.Create%2A?displayProperty=fullName> メソッドを使用してオブジェクトを作成し、<xref:System.Xml.XmlReaderSettings> クラスを使用して、そのオブジェクトで有効にする一連の機能を指定できます。|  
|<xref:System.Xml.XmlWriter?displayProperty=fullName>|ストリーム ベース|-   高速、非キャッシ、前方参照専用の XML データ生成手段を提供します。<br />-   <xref:System.Xml.XmlWriter.Create%2A?displayProperty=fullName> メソッドを使用してオブジェクトを作成し、<xref:System.Xml.XmlWriterSettings> クラスを使用して、そのオブジェクトで有効にする一連の機能を指定できます。|  
|<xref:System.Xml.XmlDocument?displayProperty=fullName>|メモリ内|-   [W3C ドキュメント オブジェクト モデル \(DOM\) 勧告の DOM Level 1 Core](http://www.w3.org/TR/REC-DOM-Level-1/level-one-core.html) および [DOM Level 2 Core](http://www.w3.org/TR/DOM-Level-2-Core/) を実装しています。<br />-   使い慣れた DOM モデルに基づくメソッドとプロパティを使用して、ノードを作成、挿入、削除、および変更できます。<br />-   このオプションは、W3C DOM を利用する既存のコードを変更する場合に使用します。|  
|<xref:System.Xml.XPath.XPathNavigator?displayProperty=fullName>|メモリ内|-   カーソル モデルを利用する複数の編集オプションとナビゲーション機能を提供します。<br />-   XML ドキュメントを <xref:System.Xml.XPath.XPathDocument> オブジェクトまたは <xref:System.Xml.XmlDocument> オブジェクトに格納できます。<br />-   XML の読み取り専用処理で優れたパフォーマンスを発揮します。<br />-   このオプションは、WXPath クエリや XSLT 変換を使用して既存のコードを変更する場合に使用します。|  
|<xref:System.Xml.Xsl.XslCompiledTransform>|メモリ内|-   XSL 変換を使用して XML データを変換するためのオプションを提供します。<br />-   [XSLT コンパイラ \(xsltc.exe\)](../../../../docs/standard/data/xml/xslt-compiler-xsltc-exe.md) を使用すると、プリコンパイルした変換をアプリで参照できます。|  
  
## Win32 オプションと COM ベースのオプション  
  
|**オプション**|**説明**|  
|---------------|------------|  
|[XmlLite](http://go.microsoft.com/fwlink/?LinkId=93723)|-   高性能 XML アプリの構築に役立つ高速、安全、非キャッシュ、前方参照専用の XML パーサー。<br />-   ダイナミック リンク ライブラリ \(DLL\) を使用できる任意の言語で動作します。C\+\+ の使用をお勧めします。|  
|[MSXML](http://go.microsoft.com/fwlink/?LinkId=93722)|-   Windows オペレーティング システムに付属する、XML 処理のための COM ベース テクノロジ。<br />-   DOM をネイティブで実装し、XPath と XSLT をサポートしています。<br />-   SAX2 イベントベース パーサーが含まれます。|  
  
## 参照  
 [DOM モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-dom-model.md)   
 [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)   
 [XSLT コンパイラ \(xsltc.exe\)](../../../../docs/standard/data/xml/xslt-compiler-xsltc-exe.md)