---
title: "タイミング動作の概要 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "動作, タイミング"
  - "タイミング動作"
ms.assetid: 5b714d46-bd46-48b8-b467-b4be89ba3091
caps.latest.revision: 20
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 14
---
# タイミング動作の概要
ここでは、アニメーションおよび他の <xref:System.Windows.Media.Animation.Timeline> オブジェクトのタイミング動作について説明します。  
  
<a name="autoTopLevelSectionsOUTLINE0"></a>   
<a name="prerequisites"></a>   
## 必要条件  
 このトピックを理解するには、基本的なアニメーション機能に精通している必要があります。  詳細については、「[アニメーションの概要](../../../../docs/framework/wpf/graphics-multimedia/animation-overview.md)」を参照してください。  
  
<a name="timelinetypes"></a>   
## タイムラインの型  
 <xref:System.Windows.Media.Animation.Timeline> は時間のセグメントを表します。  用意されているプロパティを使用して、そのセグメントの長さ、開始時間、繰り返し回数、時間の進行の速度などを指定できます。  
  
 Timeline クラスを継承するクラスには、アニメーションやメディアの再生などの追加機能が用意されています。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、次の <xref:System.Windows.Media.Animation.Timeline> の種類が用意されています。  
  
|タイムラインの型|Description|  
|--------------|-----------------|  
|<xref:System.Windows.Media.Animation.AnimationTimeline>|プロパティのアニメーション化のための出力値を生成する <xref:System.Windows.Media.Animation.Timeline> オブジェクトの抽象基本クラス。|  
|<xref:System.Windows.Media.MediaTimeline>|メディア ファイルから出力を生成します。|  
|<xref:System.Windows.Media.Animation.ParallelTimeline>|子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトをグループ化し、制御する <xref:System.Windows.Media.Animation.TimelineGroup> の型。|  
|<xref:System.Windows.Media.Animation.Storyboard>|含まれる Timeline オブジェクトの対象情報を持つ <xref:System.Windows.Media.Animation.ParallelTimeline> の型。|  
|<xref:System.Windows.Media.Animation.Timeline>|タイミング動作を定義する抽象基本クラス。|  
|<xref:System.Windows.Media.Animation.TimelineGroup>|他の <xref:System.Windows.Media.Animation.Timeline> オブジェクトを含めることができる <xref:System.Windows.Media.Animation.Timeline> オブジェクトの抽象クラス。|  
  
<a name="propertiesthatcontroltimelinelength"></a>   
## タイムラインの長さを制御するプロパティ  
 <xref:System.Windows.Media.Animation.Timeline> は時間のセグメントを表し、タイムラインの長さはさまざまな方法で示すことができます。  タイムラインの長さを示すいくつかの用語の定義を次の表に示します。  
  
|語句|Description|プロパティ||||  
|--------|-----------------|-----------|-|-|-|  
|単純継続時間|タイムラインが順方向の反復を 1 回完了するのに要する時間の長さ。|<xref:System.Windows.Media.Animation.Timeline.Duration%2A>||||  
|1 回の繰り返し|タイムラインが順方向で 1 回再生され、<xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティが true の場合に逆方向で 1 回再生されるのに要する時間の長さ。|<xref:System.Windows.Media.Animation.Timeline.Duration%2A>, <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A>||||  
|アクティブ期間|<xref:System.Windows.Media.Animation.RepeatBehavior> プロパティで指定したすべての繰り返しをタイムラインが完了するのに要する時間の長さ。|<xref:System.Windows.Media.Animation.Timeline.Duration%2A>, <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A>, <xref:System.Windows.Media.Animation.RepeatBehavior>||||  
  
<a name="duration"></a>   
### Duration プロパティ  
 前述のように、タイムラインは時間のセグメントを表します。  そのセグメントの長さはタイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> によって決まります。  タイムラインは、期間の最後に到達すると、再生を停止します。  タイムラインに子タイムラインがある場合は、子も再生を停止します。  アニメーションの場合は、<xref:System.Windows.Media.Animation.Timeline.Duration%2A> は、アニメーションが開始値から終了値まで移行するのに要する時間を指定します。  タイムラインの継続時間は、1 回の反復の継続時間と繰り返しを含むアニメーションの合計再生時間を区別するために、単純継続時間と呼ばれることもあります。  継続時間は、有限の時間値、あるいは特殊な値 <xref:System.Windows.Duration.Automatic%2A> または <xref:System.Windows.Duration.Forever%2A> を使用して指定できます。  アニメーションの継続時間は、アニメーションが値の間を遷移できるように、<xref:System.Windows.Duration.TimeSpan%2A> 値に対して解決される必要があります。  
  
 <xref:System.Windows.Media.Animation.Timeline.Duration%2A> が 5 秒間の <xref:System.Windows.Media.Animation.DoubleAnimation> の例を次に示します。  
  
  
  
 <xref:System.Windows.Media.Animation.Storyboard> や <xref:System.Windows.Media.Animation.ParallelTimeline> などのコンテナー タイムラインの既定の期間は <xref:System.Windows.Duration.Automatic%2A> であり、最後の子が再生を停止した時点で自動的に終了することを意味します。  <xref:System.Windows.Media.Animation.Timeline.Duration%2A> が 5 秒 \(すべての子 <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトの完了に要する時間の長さ\) に解決される <xref:System.Windows.Media.Animation.Storyboard> の例を次に示します。  
  
  
  
 コンテナー タイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> を <xref:System.Windows.Duration.TimeSpan%2A> 値に設定すると、その再生時間を子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトの再生時間よりも強制的に長くしたり短くしたりすることができます。  <xref:System.Windows.Media.Animation.Timeline.Duration%2A> をコンテナー タイムラインの子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトの長さを下回る値に設定すると、子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトは、コンテナー タイムラインの停止時に再生を停止します。  次の例では、前の例の <xref:System.Windows.Media.Animation.Storyboard> の <xref:System.Windows.Media.Animation.Timeline.Duration%2A> を 3 秒に設定します。  その結果、最初の <xref:System.Windows.Media.Animation.DoubleAnimation> が 3 秒後 \(ターゲットの四角形の幅を 60 にアニメーション化した時点\) に進行を停止します。  
  
  
  
<a name="repeatinganimations"></a>   
### RepeatBehavior プロパティ  
 <xref:System.Windows.Media.Animation.Timeline> コントロールの <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティは、単純継続時間を繰り返す回数を制御します。  <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用して、タイムラインの再生回数 \(反復 <xref:System.Windows.Media.Animation.RepeatBehavior.Count%2A>\) またはタイムラインの再生時間の合計 \(繰り返し <xref:System.Windows.Media.Animation.RepeatBehavior.Duration%2A>\) を指定することができます。  いずれの場合も、アニメーションは、要求されたカウントまたは期間を満たすのに必要な回数だけ実行を繰り返します。  既定では、タイムラインの反復カウントは `1.0` です。これは、再生回数が 1 回で、繰り返されないことを意味します。  
  
 <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用し、反復カウントを指定して <xref:System.Windows.Media.Animation.DoubleAnimation> の再生時間を単純継続時間の 2 倍にする例を次に示します。  
  
  
  
 <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用し、<xref:System.Windows.Media.Animation.DoubleAnimation> の再生時間を単純継続時間の半分にする例を次に示します。  
  
  
  
 <xref:System.Windows.Media.Animation.Timeline> の <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを <xref:System.Windows.Media.Animation.RepeatBehavior.Forever%2A> に設定すると、<xref:System.Windows.Media.Animation.Timeline> は、対話的に停止されるかタイミング システムによって停止されるまで繰り返されます。  <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> プロパティを使用して <xref:System.Windows.Media.Animation.DoubleAnimation> を無限に再生する例を次に示します。  
  
  
  
 他の例については、「[アニメーションを反復する](../../../../docs/framework/wpf/graphics-multimedia/how-to-repeat-an-animation.md)」を参照してください。  
  
<a name="autoreverseproperty"></a>   
### AutoReverse プロパティ  
 <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティは、順方向の反復が完了するたびに <xref:System.Windows.Media.Animation.Timeline> を逆方向に再生するかどうかを指定します。  次の例では、<xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティを `true` に設定します。その結果、0 から 100 までアニメーション化された後、100 から 0 までアニメーション化されます。  合計 10 秒間再生されます。  
  
  
  
 <xref:System.Windows.Media.Animation.RepeatBehavior.Count%2A> 値を使用して <xref:System.Windows.Media.Animation.Timeline> の <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> を指定し、その <xref:System.Windows.Media.Animation.Timeline> の <xref:System.Windows.Media.Animation.Timeline.AutoReverse%2A> プロパティを `true` に設定した場合、1 回の繰り返しには順方向の反復と逆方向の反復が 1 回ずつ含まれます。  次の例では、前の例の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> で <xref:System.Windows.Media.Animation.RepeatBehavior.Count%2A> 値を 2 に設定します。  その結果、<xref:System.Windows.Media.Animation.DoubleAnimation> は 20 秒間 \(順方向に 5 秒間、逆方向に 5 秒間、順方向に再度 5 秒間、および逆方向に 5 秒間\) 再生されます。  
  
  
  
 コンテナー タイムラインに子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトが含まれる場合、コンテナー タイムラインが反転するとその子オブジェクトも反転します。  その他の例については、「[タイムラインを自動的に反転するかどうかを指定する](../../../../docs/framework/wpf/graphics-multimedia/how-to-specify-whether-a-timeline-automatically-reverses.md)」を参照してください。  
  
<a name="timelinebegin"></a>   
## BeginTime プロパティ  
 <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> プロパティを使用すると、タイムラインを開始するタイミングを指定できます。  タイムラインの開始時間は、親タイムラインを基準とした相対値になります。  開始時間を 0 秒に設定すると、タイムラインはその親が開始されると直ちに開始されます。その他の値に設定すると、親タイムラインの再生開始時点と子タイムラインの再生時点の間のオフセットが作成されます。  たとえば、開始時間を 2 秒に設定すると、タイムラインは、その親が 2 秒の時点に到達すると再生を開始します。  既定では、すべてのタイムラインの開始時間は 0 秒です。  タイムラインの開始時間は、`null` に設定することもできます。この場合、タイムラインは開始されません。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] では、[x:Null のマークアップ拡張機能](../../../../docs/framework/xaml-services/x-null-markup-extension.md)を使用して null を指定します。  
  
 開始時間は、<xref:System.Windows.Media.Animation.Timeline.RepeatBehavior%2A> の設定によってタイムラインが繰り返されるたびに適用されるわけではないことに注意してください。  <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> が 10 秒、<xref:System.Windows.Media.Animation.RepeatBehavior> が <xref:System.Windows.Media.Animation.RepeatBehavior.Forever%2A> に設定されたアニメーションを作成した場合、アニメーションを最初に再生する前に 10 秒の遅延が発生しますが、後続の各繰り返しでは発生しません。  ただし、アニメーションの親タイムラインの再開または繰り返しが行われた場合は、10 秒の遅延が発生します。  
  
 <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> プロパティは、タイムラインをずらす際に役立ちます。  2 つの子 <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトを含む <xref:System.Windows.Media.Animation.Storyboard> を作成する例を次に示します。  最初のアニメーションの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> は 5 秒で、2 番目のアニメーションの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> は 3 秒です。  この例では、2 番目の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> を 5 秒に設定し、最初の <xref:System.Windows.Media.Animation.DoubleAnimation> の終了後に再生を開始できるようにします。  
  
  
  
<a name="fillbehaviorproperty"></a>   
## FillBehavior プロパティ  
 <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティは、<xref:System.Windows.Media.Animation.Timeline> がアクティブな継続時間全体の末尾に到達したときに、タイムラインを停止するかその最終的な値を保持するかを指定します。  <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> が <xref:System.Windows.Media.Animation.FillBehavior> に設定されたアニメーションは、その出力値を "保持" します。アニメーション化されているプロパティは、アニメーションの最終的な値を保持します。  値を <xref:System.Windows.Media.Animation.FillBehavior> に設定すると、アニメーションは、終了後にターゲット プロパティへの作用を停止します。  
  
 2 つの子 <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトを含む <xref:System.Windows.Media.Animation.Storyboard> を作成する例を次に示します。  両方の <xref:System.Windows.Media.Animation.DoubleAnimation> オブジェクトで、<xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> が 0 から 100 までアニメーション化されます。  アニメーション化されていないときの <xref:System.Windows.Shapes.Rectangle> 要素の <xref:System.Windows.FrameworkElement.Width%2A> 値は、500 [デバイス非依存ピクセル](GTMT)です。  
  
-   最初の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティは、既定値の <xref:System.Windows.Media.Animation.FillBehavior> に設定されています。  その結果、四角形の幅は、<xref:System.Windows.Media.Animation.DoubleAnimation> の終了後に 100 のまま保持されます。  
  
-   2 番目の <xref:System.Windows.Media.Animation.DoubleAnimation> の <xref:System.Windows.Media.Animation.Timeline.FillBehavior%2A> プロパティは、<xref:System.Windows.Media.Animation.FillBehavior> に設定されています。  その結果、2 番目の <xref:System.Windows.Shapes.Rectangle> の <xref:System.Windows.FrameworkElement.Width%2A> は、<xref:System.Windows.Media.Animation.DoubleAnimation> の終了後に 500 に戻ります。  
  
  
  
<a name="speedproperties"></a>   
## タイムラインの速度を制御するプロパティ  
 <xref:System.Windows.Media.Animation.Timeline> クラスでは、その速度を指定するための 3 つのプロパティが提供されます。  
  
-   <xref:System.Windows.Media.Animation.Timeline.SpeedRatio%2A> \- <xref:System.Windows.Media.Animation.Timeline> の時間の進行の速度 \(親に対する相対的な速度\) を指定します。  1 を超える値を指定すると <xref:System.Windows.Media.Animation.Timeline> とその子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトの速度が上がり、0 ～ 1 の値を指定するとその速度が下がります。  値 1 は、<xref:System.Windows.Media.Animation.Timeline> がその親と同じ速度で進行することを示します。  コンテナー タイムラインの <xref:System.Windows.Media.Animation.Timeline.SpeedRatio%2A> 設定は、すべての子 <xref:System.Windows.Media.Animation.Timeline> オブジェクトに影響します。  
  
-   <xref:System.Windows.Media.Animation.Timeline.AccelerationRatio%2A> \- 加速に費やされるタイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> の比率を指定します。  例については、「[アニメーションを加速または減速させる](../../../../docs/framework/wpf/graphics-multimedia/how-to-accelerate-or-decelerate-an-animation.md)」を参照してください。  
  
-   <xref:System.Windows.Media.Animation.Timeline.DecelerationRatio%2A> \- 減速に費やされるタイムラインの <xref:System.Windows.Media.Animation.Timeline.Duration%2A> の比率を指定します。  例については、「[アニメーションを加速または減速させる](../../../../docs/framework/wpf/graphics-multimedia/how-to-accelerate-or-decelerate-an-animation.md)」を参照してください。  
  
## 参照  
 [アニメーションの概要](../../../../docs/framework/wpf/graphics-multimedia/animation-overview.md)   
 [アニメーションとタイミング システムの概要](../../../../docs/framework/wpf/graphics-multimedia/animation-and-timing-system-overview.md)   
 [タイミング イベントの概要](../../../../docs/framework/wpf/graphics-multimedia/timing-events-overview.md)   
 [方法のトピック](../../../../docs/framework/wpf/graphics-multimedia/animation-and-timing-how-to-topics.md)   
 [アニメーションのタイミング動作のサンプル](http://go.microsoft.com/fwlink/?LinkID=159970)