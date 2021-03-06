---
title: "方法 : トランザクション サービスを作成する | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1bd2e4ed-a557-43f9-ba98-4c70cb75c154
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# 方法 : トランザクション サービスを作成する
このサンプルでは、トランザクション サービスを作成する際のさまざまな側面と、サービス操作を調整するためにクライアントが起動するトランザクションの使用について説明します。  
  
### トランザクション サービスの作成  
  
1.  サービス コントラクトを作成し、<xref:System.ServiceModel.TransactionFlowOption> 列挙型の適切な設定を使用して操作に注釈を付け、受信トランザクションの要件を指定します。<xref:System.ServiceModel.TransactionFlowAttribute> は、実装するサービス クラスにも置くことができることに注意してください。こうすると、これらのトランザクション設定を使用するインターフェイスを 1 回実装するだけで済むため、すべての実装で設定を行う必要がありません。  
  
    ```  
    [ServiceContract]  
    public interface ICalculator  
    {  
        [OperationContract]  
        // Use this to require an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Mandatory)]  
        double Add(double n1, double n2);  
        [OperationContract]  
        // Use this to permit an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        double Subtract(double n1, double n2);  
    }  
    ```  
  
2.  実装クラスを作成し、オプションとして <xref:System.ServiceModel.ServiceBehaviorAttribute> を使用して <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> および <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> を指定します。多くの場合、<xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> の既定値である 60 秒、および <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> の既定値である `Unspecified` が適切な設定値となります。操作ごとに、<xref:System.ServiceModel.OperationBehaviorAttribute> 属性を使用して、メソッド内で実行される処理が、<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性の値に応じたトランザクション スコープの範囲内で行われる必要があるかどうかを指定できます。この場合、`Add` メソッドで使用されるトランザクションは、クライアントからフローしてくる必須の受信トランザクションと同じになり、`Subtract` メソッドで使用されるトランザクションは、受信トランザクションと同じになるか \(クライアントからフローしてくる場合\)、暗黙的にローカルで新たに作成されたトランザクションになります。  
  
    ```  
    [ServiceBehavior(  
        TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable,  
        TransactionTimeout = "00:00:45")]  
    public class CalculatorService : ICalculator  
    {  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Adding {0} to {1}", n1, n2));  
            return n1 + n2;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Subtracting {0} from {1}", n2, n1));  
            return n1 - n2;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
            // This is where the transaction provides specific benefit  
            // - changes to the database will be committed only when  
            // the transaction completes.  
        }  
    }  
    ```  
  
3.  構成ファイルでバインディングを構成して、トランザクション コンテキストのフローを指定し、そのとき使用されるプロトコルを指定します。[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][ServiceModel トランザクションの構成](../../../../docs/framework/wcf/feature-details/servicemodel-transaction-configuration.md).具体的には、エンドポイント要素の `binding` 属性でバインド型を指定します。次のサンプル構成のように、[\<endpoint\>](http://msdn.microsoft.com/ja-jp/13aa23b7-2f08-4add-8dbf-a99f8127c017) 要素には `transactionalOleTransactionsTcpBinding` という名前のバインディング構成を参照する `bindingConfiguration` 属性が含まれます。  
  
    ```  
    <service name="CalculatorService">  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     次の構成で示すように、トランザクション フローは、`transactionFlow` 属性を使用して構成レベルで有効にすることができ、トランザクション プロトコルは、`transactionProtocol` 属性を使用して指定できます。  
  
    ```  
    <bindings>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### 複数のトランザクション プロトコルのサポート  
  
1.  [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] を使用して作成されたクライアントとサービスが関係するシナリオの場合、最適なパフォーマンスを得るためには OleTransactions プロトコルを使用する必要があります。ただし、サード パーティのプロトコル スタックとの相互運用性が必要なシナリオでは、WS\-AT \(WS\-AtomicTransaction\) プロトコルが有用です。次の構成ファイルの例で示すように、プロトコル固有の適切なバインディングを持つ複数のエンドポイントを用意することで、両方のプロトコルを受け入れるように [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] サービスを構成できます。  
  
    ```  
    <service name="CalculatorService">  
      <endpoint address="http://localhost:8000/CalcService"  
        binding="wsHttpBinding"  
        bindingConfiguration="transactionalWsatHttpBinding"  
        contract="ICalculator"  
        name="WSAtomicTransaction_endpoint" />  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     トランザクション プロトコルは、`transactionProtocol` 属性を使用して指定します。ただし、このバインディングは WS\-AT プロトコルのみを使用するため、この属性は、システム指定の `wsHttpBinding` には存在しません。  
  
    ```  
    <bindings>  
      <wsHttpBinding>  
        <binding name="transactionalWsatHttpBinding"  
          transactionFlow="true" />  
      </wsHttpBinding>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### トランザクションの完了の制御  
  
1.  既定では [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] の操作は、未処理の例外がスローされなかった場合、トランザクションを自動的に完了します。この動作を変更するには、<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> プロパティと <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A> メソッドを使用します。ある操作を他の操作と同じトランザクション内で行う必要がある場合 \(借方と貸方の操作など\)、次の `Debit` 操作の例に示すように、<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> プロパティを `false` に設定することで自動完了の動作を無効にできます。`Debit` 操作で使用されるトランザクションは、`Credit1` 操作に示すように <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> プロパティが `true` に設定されているメソッドが呼び出されるまで、または `Credit2` 操作に示すように、<xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A> メソッドを呼び出してトランザクションの完了が明示的に示されるまで、完了しません。2 つの貸方操作は説明のために示されています。一般には単一の貸方処理が使用されます。  
  
    ```  
    [ServiceBehavior]  
    public class CalculatorService : IAccount  
    {  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Debit(double n)  
        {  
            // Perform debit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = true)]  
        public void Credit1(double n)  
        {  
            // Perform credit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Credit2(double n)  
        {  
            // Perform alternate credit operation  
  
            OperationContext.Current.SetTransactionComplete();  
            return;  
        }  
    }  
    ```  
  
2.  トランザクションの関連付けを行うため、<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> プロパティを `false` に設定するには、セッションの多いバインディングを使用する必要があります。この要件は、<xref:System.ServiceModel.ServiceContractAttribute> の `SessionMode` プロパティで指定します。  
  
    ```  
    [ServiceContract(SessionMode = SessionMode.Required)]  
    public interface IAccount  
    {  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Debit(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit1(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit2(double n);  
    }  
    ```  
  
### トランザクション サービス インスタンスの有効期間の制御  
  
1.  [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] では、<xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A> プロパティを使用して、トランザクションが完了したときに基になるサービス インスタンスを解放するかどうかを指定します。構成が変更されていない限り、これは既定で `true` に設定されているため、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は効率的で予測可能な "ジャスト イン タイム" アクティベーション動作を示します。後続するトランザクションでサービスを呼び出すと、前回のトランザクションの状態が残らない新規のサービス インスタンスが必ず呼び出されます。これは通常は便利ですが、トランザクションの完了後もサービス インスタンス内に状態を保持する必要がある場合もあります。この例としては、必要な状態やリソースへのハンドルの取得または再構成に負荷がかかる場合があります。これを実行するには、<xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A> プロパティを `false` に設定します。このように設定することで、インスタンスとこれに関連する任意の状態が、後続する呼び出しからも利用できるようになります。この設定を使用する場合は、状態とトランザクションを消去して完了するタイミングと方法を入念に考慮する必要があります。`runningTotal` 変数を使用してインスタンスを保持することで、これを行う方法を次のサンプルに示します。  
  
    ```  
    [ServiceBehavior(TransactionIsolationLevel = [ServiceBehavior(  
        ReleaseServiceInstanceOnTransactionComplete = false)]  
    public class CalculatorService : ICalculator  
    {  
        double runningTotal = 0;  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Adding {0} to {1}", n, runningTotal));  
            runningTotal = runningTotal + n;  
            return runningTotal;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Subtracting {0} from {1}", n, runningTotal));  
            runningTotal = runningTotal - n;  
            return runningTotal;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
        }  
    }  
    ```  
  
    > [!NOTE]
    >  インスタンスの有効期間はサービスの内部に属する動作であり、<xref:System.ServiceModel.ServiceBehaviorAttribute> プロパティを介して制御できるため、インスタンスの動作を設定するためにサービス構成やサービス コントラクトを変更する必要はありません。また、ネットワーク上にもこれに相当する表現はありません。