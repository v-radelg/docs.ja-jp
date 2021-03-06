---
title: "方法: HTTP 固有のプロパティにアクセスする | Microsoft Docs"
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
ms.assetid: f8848c7e-f5c5-4d42-b86d-9951ff8f4146
caps.latest.revision: 8
author: "mcleblanc"
ms.author: "markl"
manager: "markl"
caps.handback.revision: 8
---
# 方法: HTTP 固有のプロパティにアクセスする
このオプションは、HTTP の **Keep\-alive** の動作を消し Web サーバーからプロトコルのバージョン番号を取得する方法を示します。  
  
## 使用例  
  
```vb  
Dim HttpWReq As HttpWebRequest= _  
    CType(WebRequest.Create("http://www.contoso.com"), HttpWebRequest)  
' Turn off connection keep-alives.  
HttpWReq.KeepAlive = False  
  
Dim HttpWResp As HttpWebResponse = _  
    CType(HttpWReq.GetResponse(), HttpWebResponse)  
  
' Get the HTTP protocol version number returned by the server.  
Dim ver As String = HttpWResp.ProtocolVersion.ToString()  
HttpWResp.Close()  
```  
  
```csharp  
HttpWebRequest HttpWReq =   
    (HttpWebRequest)WebRequest.Create("http://www.contoso.com");  
// Turn off connection keep-alives.  
HttpWReq.KeepAlive = false;  
  
HttpWebResponse HttpWResp = (HttpWebResponse)HttpWReq.GetResponse();  
  
// Get the HTTP protocol version number returned by the server.  
String ver = HttpWResp.ProtocolVersion.ToString();  
HttpWResp.Close();  
```  
  
## コードのコンパイル  
 この例には、次の項目が必要です。  
  
-   **\[System.Net\]** の名前空間への参照。  
  
## 参照  
 [プロキシを介したインターネットへのアクセス](../../../docs/framework/network-programming/accessing-the-internet-through-a-proxy.md)   
 [アプリケーション プロトコルの使用](../../../docs/framework/network-programming/using-application-protocols.md)   
 [HTTP](../../../docs/framework/network-programming/http.md)