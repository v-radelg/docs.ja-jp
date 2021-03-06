---
title: "方法 : XML ストリームの代替要素名を指定する | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "クラス, オーバーライド"
  - "クラスのオーバーライド"
  - "XML シリアル化のオーバーライド"
  - "シリアル化, オーバーライド"
  - "XML シリアル化, オーバーライド"
  - "XML ストリーム, 代替要素名の指定"
ms.assetid: 5cc1c0b0-f94b-4525-9a41-88a582cd6668
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# 方法 : XML ストリームの代替要素名を指定する
[コード例](#cpconoverridingserializationofclasseswithxmlattributeoverridesclassanchor1)  
  
 [XmlSerializer](https://msdn.microsoft.com/en-us/library/system.xml.serialization.xmlserializer.aspx) を使用して、クラスのセットが同じである XML ストリームを複数生成できます。このような作業が必要になるのは、2 つの異なる XML Web サービスが、若干の違いのある同じ基本情報を要求するような場合です。たとえば、書籍の注文を処理する 2 つの XML Web サービスがあり、どちらにも ISBN 番号が必要であるとします。一方のサービスは \<ISBN\> タグを使用しますが、もう一方は \<BookID\> タグを使用します。`ISBN` という名前のフィールドを含む、`Book` という名前のクラスがあります。`Book` クラスのインスタンスがシリアル化されると、既定では、メンバー名 \(ISBN\) がタグ要素名として使用されます。最初の XML Web サービスの場合、この既定どおりで問題ありません。一方、2 つ目の XML Web サービスに XML ストリームを送信するには、タグの要素名が `BookID` になるようにシリアル化をオーバーライドする必要があります。  
  
### 代替要素名で XML ストリームを作成するには  
  
1.  [XmlElementAttribute](frlrfSystemXmlSerializationXmlElementAttributeClassTopic) クラスのインスタンスを作成します。  
  
2.  <xref:System.Xml.Serialization.XmlElementAttribute> の <xref:System.Xml.Serialization.XmlElementAttribute.ElementName%2A> を "BookID" に設定します。  
  
3.  [XmlAttributes](frlrfSystemXmlSerializationXmlAttributesClassTopic) クラスのインスタンスを作成します。  
  
4.  **XmlElementAttribute** オブジェクトを <xref:System.Xml.Serialization.XmlAttributes> の <xref:System.Xml.Serialization.XmlAttributes.XmlElements%2A> プロパティによってアクセスされるコレクションに追加します。  
  
5.  [XmlAttributeOverrides](frlrfSystemXmlSerializationXmlAttributeOverridesClassTopic) クラスのインスタンスを作成します。  
  
6.  **XmlAttributes** を <xref:System.Xml.Serialization.XmlAttributeOverrides> に追加し、オーバーライドするオブジェクトの型とオーバーライドされるメンバーの名前を渡します。  
  
7.  **XmlAttributeOverrides** を使用して、**XmlSerializer** クラスのインスタンスを作成します。  
  
8.  `Book` クラスのインスタンスを作成し、シリアル化または逆シリアル化します。  
  
## 使用例  
  
```vb  
Public Class SerializeOverride()  
    ' Creates an XmlElementAttribute with the alternate name.  
    Dim myElementAttribute As XmlElementAttribute = _  
    New XmlElementAttribute()  
    myElementAttribute.ElementName = "BookID"  
    Dim myAttributes As XmlAttributes = New XmlAttributes()  
    myAttributes.XmlElements.Add(myElementAttribute)  
    Dim myOverrides As XmlAttributeOverrides = New XmlAttributeOverrides()  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes)  
    Dim mySerializer As XmlSerializer = _  
    New XmlSerializer(GetType(Book), myOverrides)  
    Dim b As Book = New Book()  
    b.ISBN = "123456789"  
    ' Creates a StreamWriter to write the XML stream to.  
    Dim writer As StreamWriter = New StreamWriter("Book.xml")  
    mySerializer.Serialize(writer, b);  
End Class  
  
```  
  
```csharp  
public class SerializeOverride()  
{  
    // Creates an XmlElementAttribute with the alternate name.  
    XmlElementAttribute myElementAttribute = new XmlElementAttribute();  
    myElementAttribute.ElementName = "BookID";  
    XmlAttributes myAttributes = new XmlAttributes();  
    myAttributes.XmlElements.Add(myElementAttribute);  
    XmlAttributeOverrides myOverrides = new XmlAttributeOverrides();  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes);  
    XmlSerializer mySerializer =   
    new XmlSerializer(typeof(Book), myOverrides)  
    Book b = new Book();  
    b.ISBN = "123456789"  
    // Creates a StreamWriter to write the XML stream to.  
    StreamWriter writer = new StreamWriter("Book.xml");  
    mySerializer.Serialize(writer, b);  
}  
```  
  
 XML ストリームは、次のようになります。  
  
```  
<Book>  
    <BookID>123456789</BookID>  
</Book>  
```  
  
## 参照  
 [XML シリアル化および SOAP シリアル化](../../../docs/framework/serialization/xml-and-soap-serialization.md)   
 [XmlSerializer](https://msdn.microsoft.com/en-us/library/system.xml.serialization.xmlserializer.aspx)   
 [XmlElementAttribute](frlrfSystemXmlSerializationXmlElementAttributeClassTopic)   
 [XmlAttributes](frlrfSystemXmlSerializationXmlAttributesClassTopic)   
 [XmlAttributeOverrides](frlrfSystemXmlSerializationXmlAttributeOverridesClassTopic)   
 [方法 : オブジェクトをシリアル化する](../../../docs/framework/serialization/how-to-serialize-an-object.md)   
 [方法 : オブジェクトを逆シリアル化する](../../../docs/framework/serialization/how-to-deserialize-an-object.md)   
 [方法 : オブジェクトを逆シリアル化する](../../../docs/framework/serialization/how-to-deserialize-an-object.md)