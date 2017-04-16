---
title: "DataContractResolver | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6c200c02-bc14-4b8d-bbab-9da31185b805
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# DataContractResolver
このサンプルでは、<xref:System.Runtime.Serialization.DataContractResolver> クラスを使用して、シリアル化プロセスおよび逆シリアル化プロセスをカスタマイズする方法を示します。このサンプルでは、シリアル化および逆シリアル化の際に CLR 型と xsi:type 表現との間にマッピングを行うために DataContractResolver を使用する方法を示します。  
  
## サンプルの詳細  
 サンプルでは、次の CLR 型を定義します。  
  
```csharp  
using System;  
using System.Runtime.Serialization;  
  
namespace Types  
{  
    [DataContract]  
    public class Customer  
    {  
        [DataMember]  
        public string Name { get; set; }  
    }  
  
    [DataContract]  
    public class VIPCustomer : Customer  
    {  
        [DataMember]  
        public string VipInfo { get; set; }  
    }  
  
    [DataContract]  
    public class RegularCustomer : Customer  
    {  
    }  
  
    [DataContract]  
    public class PreferredVIPCustomer : VIPCustomer  
    {  
    }  
}  
  
```  
  
 サンプルでは、このアセンブリを読み込み、これらの各型を抽出して、型をシリアル化および逆シリアル化します。次の例で示すように、<xref:System.Runtime.Serialization.DataContractResolver> は <xref:System.Runtime.Serialization.DataContractResolver> 派生クラスのインスタンスを <xref:System.Runtime.Serialization.DataContractSerializer> コンストラクターに渡すことによってシリアル化プロセスに追加されます。  
  
```csharp  
this.serializer = new DataContractSerializer(typeof(Object), null, int.MaxValue, false, true, null, new MyDataContractResolver(assembly));  
  
```  
  
 次のコード例に示すように、サンプルでは CLR 型をシリアル化しています。  
  
```csharp  
Assembly assembly = Assembly.Load(new AssemblyName("Types"));  
  
public void serialize(Type type)  
{  
    Object instance = Activator.CreateInstance(type);  
  
    Console.WriteLine("----------------------------------------");  
    Console.WriteLine();  
    Console.WriteLine("Serializing type: {0}", type.Name);  
    Console.WriteLine();  
    this.buffer = new StringBuilder();  
    using (XmlWriter xmlWriter = XmlWriter.Create(this.buffer))  
    {  
        try  
        {  
            this.serializer.WriteObject(xmlWriter, instance);  
        }  
        catch (SerializationException error)  
        {  
            Console.WriteLine(error.ToString());  
        }  
    }  
    Console.WriteLine(this.buffer.ToString());  
}  
  
```  
  
 次のコード例に示すように、サンプルでは xsi:types を逆シリアル化しています。  
  
```csharp  
public void deserialize(Type type)  
{  
    Console.WriteLine();  
    Console.WriteLine("Deserializing type: {0}", type.Name);  
    Console.WriteLine();  
    using (XmlReader xmlReader = XmlReader.Create(new StringReader(this.buffer.ToString())))  
    {  
        Object obj = this.serializer.ReadObject(xmlReader);  
    }  
}  
  
```  
  
 カスタム <xref:System.Runtime.Serialization.DataContractResolver> は <xref:System.Runtime.Serialization.DataContractSerializer> コンストラクターに渡されるため、CLR 型を同等の `xsi:type` にマッピングするためのシリアル化時に <xref:System.Runtime.Serialization.DataContractResolver.TryResolveType%2A> が呼び出されます。同様に、`xsi:type` を同等の CLR 型にマッピングするための逆シリアル化時に <xref:System.Runtime.Serialization.DataContractResolver.ResolveName%2A> が呼び出されます。このサンプルでは、<xref:System.Runtime.Serialization.DataContractResolver> は、次の例のように定義されます。  
  
 <xref:System.Runtime.Serialization.DataContractResolver> から派生するクラスのコード例を次に示します。  
  
```  
class MyDataContractResolver : DataContractResolver  
{  
    private Dictionary<string, XmlDictionaryString> dictionary = new Dictionary<string, XmlDictionaryString>();  
    Assembly assembly;  
  
    public MyDataContractResolver(Assembly assembly)  
    {  
        this.assembly = assembly;  
    }  
  
    // Used at deserialization  
    // Allows users to map xsi:type name to any Type   
    public override Type ResolveName(string typeName, string typeNamespace, DataContractResolver knownTypeResolver)  
    {  
        XmlDictionaryString tName;  
        XmlDictionaryString tNamespace;  
        if (dictionary.TryGetValue(typeName, out tName) && dictionary.TryGetValue(typeNamespace, out tNamespace))  
        {  
            return this.assembly.GetType(tNamespace.Value + "." + tName.Value);  
        }  
        else  
        {  
            return null;  
        }  
    }  
  
    // Used at serialization  
    // Maps any Type to a new xsi:type representation  
    public override void ResolveType(Type dataContractType, DataContractResolver knownTypeResolver, out XmlDictionaryString typeName, out XmlDictionaryString typeNamespace)  
    {  
        string name = dataContractType.Name;  
        string namesp = dataContractType.Namespace;  
        typeName = new XmlDictionaryString(XmlDictionary.Empty, name, 0);   
        typeNamespace = new XmlDictionaryString(XmlDictionary.Empty, namesp, 0);  
        if (!dictionary.ContainsKey(dataContractType.Name))  
        {  
            dictionary.Add(name, typeName);  
        }  
        if (!dictionary.ContainsKey(dataContractType.Namespace))  
        {  
            dictionary.Add(namesp, typeNamespace);  
        }  
    }  
}  
  
```  
  
 サンプルの一部として、Types プロジェクトでは、このサンプルで使用するすべての型を含むアセンブリが生成されます。このプロジェクトは、シリアル化される型を追加、削除、または変更するために使用します。  
  
#### このサンプルを使用するには  
  
1.  [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] を使用して、DCRSample.sln ソリューション ファイルを開きます。  
  
2.  ソリューションを実行するには、F5 キーを押します。  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。続行する前に、次の \(既定の\) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合は、「[.NET Framework 4 向けの Windows Communication Foundation \(WCF\) および Windows Workflow Foundation \(WF\) のサンプル](http://go.microsoft.com/fwlink/?LinkId=150780)」にアクセスして、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] および [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをすべてダウンロードしてください。このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\DataContractResolver`  
  
## 参照  
 [データ コントラクト リゾルバーの使用](../../../../docs/framework/wcf/feature-details/using-a-data-contract-resolver.md)