---
title: "相互運用性の概要 (C# プログラミング ガイド) | Microsoft Docs"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- COM interop
- C# language, interoperability
- C++ Interop
- interoperability, about interoperability
- platform invoke
ms.assetid: c025b2e0-2357-4c27-8461-118f0090aeff
caps.latest.revision: 43
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: Human Translation
ms.sourcegitcommit: fe32676f0e39ed109a68f39584cf41aec5f5ce90
ms.openlocfilehash: 5084c4af3334c39f844fec67a1ab05dd9443bf27
ms.contentlocale: ja-jp
ms.lasthandoff: 05/10/2017

---
# <a name="interoperability-overview-c-programming-guide"></a>相互運用性の概要 (C# プログラミング ガイド)
C# マネージ コードとアンマネージ コード間で相互運用を可能にする方法について説明します。  
  
## <a name="platform-invoke"></a>プラットフォーム呼び出し  
 "*プラットフォーム呼び出し*" とは、Microsoft Win32 API にあるような、ダイナミックリンク ライブラリ (DLL) で実装されているアンマネージ関数をマネージ コードで呼び出すことを可能にするサービスです。 これはエクスポートされた関数を見つけて呼び出し、必要に応じて相互運用の境界を越えて、その引数 (整数、文字列、配列、構造体、その他) をマーシャリングします。  
  
 詳細については、「[アンマネージ DLL 関数の処理](../../../framework/interop/consuming-unmanaged-dll-functions.md)」と「[方法: プラットフォーム呼び出しを使用して Wave ファイルを再生する](../../../csharp/programming-guide/interop/how-to-use-platform-invoke-to-play-a-wave-file.md)」を参照してください。  
  
> [!NOTE]
>  [共通言語ランタイム](../../../standard/clr.md) (CLR) が、システム リソースへのアクセスを管理します。 CLR の外部のアンマネージ コードを呼び出すと、このセキュリティ メカニズムがバイパスされるため、セキュリティ リスクが生じます。 たとえば、アンマネージ コードがアンマネージ コード内のリソースを直接呼び出した場合、CLR のセキュリティ機構がバイパスされます。 詳細については、[.NET Framework セキュリティ](http://go.microsoft.com/fwlink/?LinkId=37122)に関する記事を参照してください。  
  
## <a name="c-interop"></a>C++ Interop  
 It Just Works (IJW) とも呼ばれる C++ interop を使用してネイティブ C++ クラスをラップすると、このクラスを C# またはその他の .NET Framework 言語で作成されたコードで使用できるようになります。 これを行うには、C++ コードを記述して、ネイティブ DLL または COM コンポーネントをラップします。 他の .NET Framework 言語とは異なり、[!INCLUDE[vcprvc](../../../csharp/programming-guide/interop/includes/vcprvc_md.md)] には相互運用性サポートが備えられています。これにより、マネージ コードとアンマネージ コードは同じアプリケーション内、また同じファイルでも共存できるようになります。 C++ コードは、マネージ アセンブリを生成する **/clr** コンパイラ スイッチを使用して構築できます。 最後に、C# プロジェクトのアセンブリへの参照を追加し、他のマネージ クラスを使用するときと同じように、ラップされたオブジェクトを使用します。  
  
## <a name="exposing-com-components-to-c"></a>C# への COM コンポーネントの公開  
 C# プロジェクトから COM コンポーネントを使用することができます。 一般的な手順は次のとおりです。  
  
1.  使用する COM コンポーネントを探して登録します。 regsvr32.exe を使用して、COM DLL の登録または登録解除を行います。  
  
2.  プロジェクトに、COM コンポーネントまたはタイプ ライブラリへの参照を追加します。  
  
     参照を追加する際、[!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)] は [Tlbimp.exe (Type Library Importer)](http://msdn.microsoft.com/library/ec0a8d63-11b3-4acd-b398-da1e37e97382) を使用します。これにより、タイプ ライブラリを入力として取得し、.NET Framework 相互運用アセンブリを出力します。 このアセンブリは、ランタイム呼び出し可能ラッパー (RCW) とも呼ばれ、タイプ ライブラリ内の COM クラスとインターフェイスをラップする、マネージ クラスとインターフェイスを含みます。 [!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)] は、生成されたアセンブリへの参照をプロジェクトに追加します。  
  
3.  RCW で定義されているクラスのインスタンスを作成します。 これにより、COM オブジェクトのインスタンスが作成されます。  
  
4.  他のマネージ オブジェクトを使用するときと同様に、オブジェクトを使用します。 オブジェクトがガベージ コレクションによって解放されるとき、COM オブジェクトのインスタンスもメモリから解放されます。  
  
 詳細については、「[.NET Framework への COM コンポーネントの公開](http://msdn.microsoft.com/library/e78b14f1-e487-43cd-9c6d-1a07483f1730)」を参照してください。  
  
## <a name="exposing-c-to-com"></a>COM への C# の公開  
 COM クライアントは、適切に公開されている C# 型を使用できます。 C# 型を公開する基本的な手順は次のとおりです。  
  
1.  C# プロジェクトに相互運用属性を追加します。  
  
     アセンブリ COM は、[!INCLUDE[csprcs](../../../csharp/includes/csprcs_md.md)] プロジェクト プロパティを変更することで参照できるようになります。 詳細については、「[[アセンブリ情報] ダイアログ ボックス](https://docs.microsoft.com/visualstudio/ide/reference/assembly-information-dialog-box)」を参照してください。  
  
2.  COM タイプ ライブラリを生成し、COM の使用状況に登録します。  
  
     [!INCLUDE[csprcs](../../../csharp/includes/csprcs_md.md)] プロジェクト プロパティを変更して、C# アセンブリが COM 相互運用に自動的に登録されるようにできます。 [!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)] は `/tlb` コマンド ライン スイッチを使用して [Regasm.exe (Assembly Registration Tool)](http://msdn.microsoft.com/library/e190e342-36ef-4651-a0b4-0e8c2c0281cb) を使用します。これにより、マネージ アセンブリが入力として出力され、タイプ ライブラリを生成できます。 タイプ ライブラリは、アセンブリ内の `public` 型を記述し、レジストリ エントリを追加することで、COM クライアントがマネージ クラスを作成できるようにします。  
  
 詳細については、「[COM への .NET Framework コンポーネントの公開](http://msdn.microsoft.com/library/e42a65f7-1e61-411f-b09a-aca1bbce24c6)」と「[COM クラスの例](../../../csharp/programming-guide/interop/example-com-class.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目  
 [相互運用パフォーマンスの向上](http://go.microsoft.com/fwlink/?LinkId=99564)   
 [COM 相互運用の概要](http://go.microsoft.com/fwlink/?LinkId=112406)   
 [マネージ コードとアンマネージ コード間でのマーシャリング](http://go.microsoft.com/fwlink/?LinkId=112398)   
 [アンマネージ コードとの相互運用](https://msdn.microsoft.com/library/sd10k43k)   
 [高度な COM 相互運用性](http://msdn.microsoft.com/en-us/3ada36e5-2390-4d70-b490-6ad8de92f2fb)   
 [C# プログラミング ガイド](../../../csharp/programming-guide/index.md)
