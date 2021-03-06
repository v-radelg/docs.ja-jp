---
title: "CommandBuilder でのコマンドの生成 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6e3fb8b5-373b-4f9e-ab03-a22693df8e91
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# CommandBuilder でのコマンドの生成
`SelectCommand` プロパティが実行時に動的に指定される場合、たとえばクエリ ツールを使用してユーザーの記述したクエリ構文を解釈する場合は、適切な `InsertCommand`、`UpdateCommand`、または `DeleteCommand` をデザイン時に指定することはできません。  <xref:System.Data.DataTable> を単一データベース テーブルに割り当てたり、単一データベースから生成する場合は、<xref:System.Data.Common.DbCommandBuilder> オブジェクトを利用して自動的に <xref:System.Data.Common.DbDataAdapter> の `DeleteCommand`、`InsertCommand`、および `UpdateCommand` を生成できます。  
  
 コマンドを自動的に生成するための最低限の条件として、`SelectCommand` プロパティを設定する必要があります。  `SelectCommand` プロパティで取得したテーブル スキーマによって、自動的に生成される INSERT、UPDATE、DELETE の各ステートメントの構文が決定されます。  
  
 <xref:System.Data.Common.DbCommandBuilder> は、INSERT、UPDATE、DELETE の各コマンドを生成するために `SelectCommand` を実行する必要があります。  その結果、データ ソースへの追加のトリップが必要になるため、パフォーマンスが低下する可能性があります。  最適のパフォーマンスを実現するには、明示的にコマンドを指定し、<xref:System.Data.Common.DbCommandBuilder> を使用しないようにします。  
  
 `SelectCommand` は少なくとも 1 つの主キーまたは一意の列を返す必要があります。  どちらも存在しない場合は、`InvalidOperation` 例外が生成され、コマンドは生成されません。  
  
 `DataAdapter` との関連付けが行われていて、`DataAdapter` の `InsertCommand`、`UpdateCommand`、`DeleteCommand` の各プロパティが null 参照である場合、<xref:System.Data.Common.DbCommandBuilder> は自動的にこれらのプロパティを生成します。  プロパティに対して既に `Command` が存在する場合は、既存の `Command` が使用されます。  
  
 複数のテーブルを結合して作成したデータベース ビューは、単一データベース テーブルとは見なされません。  この場合は、<xref:System.Data.Common.DbCommandBuilder> を使用してコマンドを自動的に生成できないため、コマンドを明示的に指定する必要があります。  `DataSet` に対する更新を元のデータ ソースに反映させるコマンドを明示的に設定する方法については、「[DataAdapter によるデータ ソースの更新](../../../../docs/framework/data/adonet/updating-data-sources-with-dataadapters.md)」を参照してください。  
  
 出力パラメーターを `DataSet` の更新行に割り当てることが必要な場合があります。  一般的なタスクの 1 つは、データ ソースの自動的に生成された ID フィールドまたはタイムスタンプの値を取得することです。  <xref:System.Data.Common.DbCommandBuilder> は、既定では更新行の列に出力パラメーターを割り当てません。  その場合は、コマンドを明示的に指定する必要があります。  自動的に生成された ID フィールドを挿入行の列に割り当てる例については、「[ID 値および Autonumber 値の取得](../../../../docs/framework/data/adonet/retrieving-identity-or-autonumber-values.md)」を参照してください。  
  
## コマンドの自動生成規則  
 コマンドの自動生成規則を次の表に示します。  
  
|コマンド|規則|  
|----------|--------|  
|`InsertCommand`|<xref:System.Data.DataRow.RowState%2A> が <xref:System.Data.DataRowState> に設定されているテーブル内のすべての行に対して、データ ソースの行を挿入します。  更新可能なすべての列に対して値を挿入します \(ただし、ID、式、タイムスタンプなどの列は除きます\)。|  
|`UpdateCommand`|`RowState` が <xref:System.Data.DataRowState> に設定されているテーブル内のすべての行に対して、データ ソースの行を更新します。  ID や式などの更新不可能な列を除く、すべての列の値を更新します。  データ ソースの列の値と該当行の主キー列の値が一致し、さらにデータ ソースのその他の列がその行の元の値と一致する、すべての行を更新します。  詳細については、このトピックで後述する「更新および削除のオプティミスティック同時実行制御」を参照してください。|  
|`DeleteCommand`|`RowState` が <xref:System.Data.DataRowState> に設定されているテーブル内のすべての行に対して、データ ソースの行を削除します。  列の値とその行の主キー列の値が一致し、さらにデータ ソースのその他の列がその行の元の値と一致する、すべての行を削除します。  詳細については、このトピックで後述する「更新および削除のオプティミスティック同時実行制御」を参照してください。|  
  
## 更新および削除のオプティミスティック同時実行制御  
 UPDATE ステートメントおよび DELETE ステートメントに対するコマンドの自動生成ロジックは、*オプティミスティック同時実行制御*に基づいています。これはつまり、編集時にレコードがロックされず、他のユーザーまたはプロセスがそのレコードをいつでも変更できることを意味します。  レコードは SELECT ステートメントによって返された後、UPDATE ステートメントまたは DELETE ステートメントの実行前に変更されている可能性もあるため、自動的に生成される UPDATE ステートメントまたは DELETE ステートメントには、元のすべての値を含み、データ ソースから削除されていない行だけを更新するように指定した WHERE 句が含まれます。  これにより、新しいデータが上書きされるのを防ぎます。  自動的に生成された更新コマンドが削除済みの行、または <xref:System.Data.DataSet> にある元の値が含まれていない行を更新しようとすると、コマンドはどのレコードにも反映されずに、<xref:System.Data.DBConcurrencyException> がスローされます。  
  
 元の値とは関係なく UPDATE または DELETE を実行する場合は、`DataAdapter` に明示的に `UpdateCommand` を設定し、コマンドの自動生成は行わないでください。  
  
## コマンドの自動生成ロジックの制限事項  
 コマンドの自動生成には次の制限事項が適用されます。  
  
### リレーションシップのないテーブルに限定  
 コマンドの自動生成ロジックでは、データ ソースの他のテーブルへのリレーションシップを考慮せずに、独立したテーブルを対象として INSERT、UPDATE、または DELETE の各ステートメントを生成します。  その結果、`Update` を呼び出してデータベースの外部キー制約に関係する列に対する変更を発行すると、エラーが発生する場合があります。  このような例外を防ぐには、外部キー制約に関係する列の更新には <xref:System.Data.Common.DbCommandBuilder> を使用せず、更新操作を実行するステートメントを明示的に指定します。  
  
### テーブル名と列名  
 列名またはテーブル名にスペース、ピリオド \(.\)、疑問符 \(?\)、引用符、その他の英数字以外の特殊文字が含まれていると、それらの文字が角かっこで囲まれていても、コマンドの自動生成ロジックはエラーになる場合があります。  プロバイダーによって異なりますが、QuotePrefix パラメーターと QuoteSuffix パラメーターを設定すると、生成ロジックではスペースを処理できる場合があっても、特殊文字をエスケープできません。  *catalog.schema.table* の形式をとる、テーブルの完全修飾名はサポートされています。  
  
## CommandBuilder による SQL ステートメントの自動生成  
 `DataAdapter` に対して SQL ステートメントを自動的に生成するには、まず `DataAdapter` の `SelectCommand` プロパティを設定します。次に、`CommandBuilder` オブジェクトを作成し、`CommandBuilder` で SQL ステートメントを自動的に生成する `DataAdapter` を引数として指定します。  
  
```vb  
' Assumes that connection is a valid SqlConnection object   
' inside of a Using block.  
Dim adapter As SqlDataAdapter = New SqlDataAdapter( _  
  "SELECT * FROM dbo.Customers", connection)  
Dim builder As SqlCommandBuilder = New SqlCommandBuilder(adapter)  
builder.QuotePrefix = "["  
builder.QuoteSuffix = "]"  
```  
  
```csharp  
// Assumes that connection is a valid SqlConnection object  
// inside of a using block.  
SqlDataAdapter adapter = new SqlDataAdapter(  
  "SELECT * FROM dbo.Customers", connection);  
SqlCommandBuilder builder = new SqlCommandBuilder(adapter);  
builder.QuotePrefix = "[";  
builder.QuoteSuffix = "]";  
```  
  
## SelectCommand の変更  
 INSERT、UPDATE、または DELETE の各コマンドを自動生成した後に `SelectCommand` の `CommandText` を変更すると、例外が発生することがあります。  変更された `SelectCommand.CommandText` に、INSERT、UPDATE、または DELETE の各コマンドの自動生成時に使用した `SelectCommand.CommandText` と矛盾するスキーマ情報が含まれている場合、後続の `DataAdapter.Update` メソッド呼び出しでアクセスする列は `SelectCommand` によって参照された現在のテーブルには存在しない可能性があり、例外が発生します。  
  
 `CommandBuilder` の `RefreshSchema` メソッドを呼び出すことで、自動的にコマンドを生成する `CommandBuilder` が使用するスキーマ情報を更新できます。  
  
 どのコマンドが自動的に生成されたかを確認するには、`CommandBuilder` オブジェクトの `GetInsertCommand`、`GetUpdateCommand`、`GetDeleteCommand` の各メソッドを使用して、自動的に生成されたコマンドへの参照を取得し、関連付けられているコマンドの `CommandText` プロパティを確認します。  
  
 自動的に生成された更新コマンドをコンソールに出力するコード サンプルを次に示します。  
  
```  
Console.WriteLine(builder.GetUpdateCommand().CommandText)  
```  
  
 次の例では、`custDS` データセットに `Customers` テーブルを作成し直します。  **RefreshSchema** メソッドを呼び出し、新しい列情報を使用して、自動的に生成されたコマンドを更新します。  
  
```vb  
' Assumes an open SqlConnection and SqlDataAdapter inside of a Using block.  
adapter.SelectCommand.CommandText = _  
  "SELECT CustomerID, ContactName FROM dbo.Customers"  
builder.RefreshSchema()  
  
custDS.Tables.Remove(custDS.Tables("Customers"))  
adapter.Fill(custDS, "Customers")  
```  
  
```csharp  
// Assumes an open SqlConnection and SqlDataAdapter inside of a using block.  
adapter.SelectCommand.CommandText =   
  "SELECT CustomerID, ContactName FROM dbo.Customers";  
builder.RefreshSchema();  
  
custDS.Tables.Remove(custDS.Tables["Customers"]);  
adapter.Fill(custDS, "Customers");  
```  
  
## 参照  
 [コマンドとパラメーター](../../../../docs/framework/data/adonet/commands-and-parameters.md)   
 [コマンドの実行](../../../../docs/framework/data/adonet/executing-a-command.md)   
 [DbConnection、DbCommand、および DbException](../../../../docs/framework/data/adonet/dbconnection-dbcommand-and-dbexception.md)   
 [ADO.NET Managed Providers and DataSet Developer Center \(ADO.NET マネージ プロバイダーと DataSet デベロッパー センター\)](http://go.microsoft.com/fwlink/?LinkId=217917)