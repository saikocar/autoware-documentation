コンポーネント設計の計画
目的
自動運転システムの計画コンポーネントは、自動運転車両の目標軌道 (経路と速度) を生成する際に重要な役割を果たします。安全と交通ルールの順守を確保し、特定の使命を果たします。

このドキュメントでは、Autoware 内の計画要件と設計の概要を説明し、開発者が計画コンポーネントの設計と拡張性を理解するのに役立ちます。

このドキュメントは 2 つの部分に分かれており、最初の部分では高レベルの要件と設計について説明し、後半の部分では実際の実装と提供される機能に焦点を当てます。

目標と非目標
私たちの目標は、単に自動運転システムの開発だけにとどまりません。ユーザーが個々のニーズに合わせて自動運転機能を強化できる「自動運転プラットフォーム」の提供を目指します。

Autoware では、高い拡張性、機能モジュール性、明確に定義されたインターフェイスを重視するマイクロオートノミー アーキテクチャの概念を利用しています。

これを念頭に置いて、Planning コンポーネントの設計ポリシーは、すべての複雑な自動運転シナリオに対処すること (これは非常に困難な問題であるため) ではなく、カスタマイズ可能で簡単に拡張可能な Planning 開発プラットフォームを提供することに重点を置いています。このアプローチにより、プラットフォームが幅広いニーズに対応できるようになり、最終的には多くの複雑なユースケースが解決されると考えています。

この方針を明確にするために、目標と非目標を次のように定義します。

目標:

単純な ODD を定義できるように、基本的な関数が提供されています。
機能を拡張する前に、計画コンポーネントは自動運転に必要な重要な機能を提供する必要があります。これには、移動、停止、方向転換などの基本操作に加え、比較的安全で単純な状況での車線変更や障害物の回避の処理も含まれます。
機能はユーザー主導の拡張のためにモジュール化されています
このシステムは、拡張機能を備えたさまざまな運用設計ドメイン (ODD) に適応するように設計されています。プラグインに似たモジュール化により、さまざまなレベルの自動運転やさまざまな車両または環境アプリケーション（例：Lv4/Lv2 自動運転、公共/私道走行、大型車両、小型ロボットなど）など、多様なニーズに合わせたシステムの作成が可能になります。 ）。
障害物のない私道など、特定の ODD の機能を削減することも重要な側面です。このモジュール式アプローチにより、特定のユーザーのニーズに合わせて消費電力やセンサー要件を削減できます。
この機能は人間のオペレーターの判断によって拡張可能です
オペレーター支援を組み込むことは、機能拡張の重要な側面です。これは、システムが人間のサポートを受けながら、複雑で困難なシナリオに適応できることを意味します。特定の種類の演算子はここでは定義されていません。それは、プロトタイプの開発段階で車両に同乗する人かもしれないし、自動運転サービス中の緊急時に接続される遠隔操作者かもしれない。
目標以外:

Planning コンポーネントは、サードパーティのモジュールで拡張できるように設計されています。したがって、以下は Autoware の計画コンポーネントの目標ではありません。

ユーザーが必要とするすべての機能をデフォルトで提供します。
自動運転システムの完全な機能と性能特性を提供する。
常に人間の能力を超えた性能を提供すること、あるいは絶対的な安全性を確保すること。
これらの側面は、自動運転「プラットフォーム」という当社のビジョンに特有のものであり、一般的な自動運転計画コンポーネントには当てはまらない場合があります。

ハイレベルなデザイン
この図は、計画コンポーネントの高レベルのアーキテクチャを示しています。これは理想的な設計を表しており、現在の実装は異なる場合があります。実装の詳細については、このドキュメントの後半のセクションで説明します。

全体計画アーキテクチャ

マイクロオートノミー アーキテクチャの原則に従って、モジュール式システム フレームワークを採用しました。計画ドメイン内の機能は、特定の使用例に応じて動的または静的に適応できるモジュールとして実装されます。これには、車線変更、交差点処理、横断歩道などのモジュールが含まれます。

計画コンポーネントは、いくつかのサブコンポーネントで構成されます。

ミッションプランニング: 地図データを利用して現在地から目的地までのルートを計算するモジュールです。その機能は、フリート管理システム (FMS) やカーナビゲーションのルート計画に似ています。
計画モジュール: これらのモジュールは、ターゲットの軌道、ウィンカー信号などを含む、割り当てられたミッションに対する車両の動作を計画します。これらのモジュールは、動作と動作のカテゴリに分類されます。
行動: 安全でルールに準拠したルートの計算、車線変更、交差点進入、停止線での停止の決定の管理に重点を置きます。
モーション: 動作モジュールと連携して、車両の動きと乗り心地を考慮して車両の軌道を決定します。これには、ルート形成と速度計算のための横方向および縦方向の計画が含まれます。
検証: 緊急対応機能を備え、計画された軌道の安全性と適切性を保証します。計画された軌道が不適切な場合、緊急プロトコルがトリガーされるか、代替経路が生成されます。
ハイライト
この高レベル設計の重要な側面は次のとおりです。

各機能の変調
ルート生成、車線変更、交差点管理などの重要な計画機能がモジュール化されています。これらのモジュールには標準化されたインターフェイスが付属しており、追加や変更が簡単に行えます。これらのインターフェイスの詳細については、後続のセクションで説明します。各モジュールを有効/無効にする方法の詳細については、Planning の実装ドキュメントを参照してください。

ミッション計画サブコンポーネントの分離
ミッションプランニングは、FMS (フリート管理システム) などの既存のサービスに通常見られる機能の代替として機能します。高レベルの設計で定義されたインターフェイスを遵守することで、サードパーティのサービスとの統合が容易になります。

検証サブコンポーネントの分離
計画コンポーネントの拡張可能な性質を考慮すると、すべての機能にわたって一貫した安全レベルを確保することは困難です。したがって、検証機能はコアの計画モジュールから独立して管理され、計画モジュールの任意の変更に対しても安全性のベースラインを維持します。

HMI用インターフェース（ヒューマン・マシン・インターフェース）
HMI は人間のオペレーターとスムーズに連携できるように設計されています。これらのインターフェイスにより、車内または遠隔地にかかわらず、計画コンポーネントとオペレーター間の調整が可能になります。

計画とその他のコンポーネントの分離に関するトレードオフ
Autoware の包括的な設計では、計画、認識、ローカリゼーション、制御などのコンポーネントを分離することで、サードパーティ モジュールとの連携が容易になります。ただし、この分離にはパフォーマンスと拡張性の間のトレードオフが伴います。たとえば、Perception コンポーネントは、Planning から分離されているため、不要なオブジェクトを処理する可能性があります。同様に、計画と制御を分離すると、計画中に車両のダイナミクスを考慮する際に課題が生じる可能性があります。これらの問題を軽減するには、インターフェイス情報を強化するか、計算量を増やす必要がある場合があります。

機能をカスタマイズする
計画コンポーネント設計の重要な機能は、外部モジュールと統合できることです。以下の図は、外部機能を組み込むためのさまざまな方法を示しています。

新しいモジュールを追加する方法

1. 計画コンポーネントへの新しいモジュールの追加
ユーザーは、既存の Planning 機能を新しいモジュールで拡張または置き換えることができます。このアプローチは一般的に機能を拡張するために使用され、目的の ODD にない機能の追加や既存の機能の簡素化を可能にします。

ただし、これらの機能を追加するには、よく整理されたモジュール インターフェイスが必要です。2023 年 11 月の時点では、理想的なモジュラー システムは完全には確立されておらず、いくつかの制限があります。詳細については、「リファレンス実装」セクション「現在の実装での機能のカスタマイズ」および「計画」の実装ドキュメントを参照してください。

2. 計画のサブコンポーネントの置き換え
サブコンポーネント レベルでのコラボレーションと拡張に興味を持つユーザーもいるかもしれません。これには、ミッション計画を既存の FMS サービスに置き換えたり、既存の検証機能を利用しながらサードパーティの軌道生成モジュールを組み込んだりすることが含まれる可能性があります。

計画コンポーネントの内部インターフェイスに準拠すると、このレベルでのコラボレーションと拡張が可能になります。既存の計画機能との複雑な連携は制限される場合がありますが、特定の計画コンポーネント機能と外部モジュール間の統合が可能になります。

3. 計画コンポーネント全体の置き換え
自動運転計画システムを開発している組織や研究機関は、独自の計画ソリューションと Autoware の知覚モジュールまたは制御モジュールの統合に興味があるかもしれません。これは、コンポーネント間で定義された堅牢で安定したインターフェイスに従って、Planning システム全体を置き換えることによって実現できます。ただし、既存の計画モジュールと直接調整できない場合があることに注意することが重要です。

コンポーネントインターフェース
このセクションでは、計画コンポーネントとその内部モジュールの入力と出力について説明します。現在の実装については、「計画コンポーネント インターフェイス」ページを参照してください。

計画コンポーネントへの入力
地図から
ベクトル マップ: ルート計画のための車線接続情報、参照パスを生成するための車線形状、交通ルール関連情報など、環境に関するすべての静的情報が含まれます。
知覚から
検出物体情報:歩行者や他の車両など、事前に知ることができない物体に関するリアルタイムの情報を提供します。計画コンポーネントは、これらの物体との衝突を回避するための操縦を計画します。
検出された障害物情報: 障害物の位置に関するリアルタイム情報を提供します。これは、検出されたオブジェクトよりも原始的なもので、緊急停止やその他の安全対策に使用されます。
占有マップ情報: 歩行者や他の車両の存在に関するリアルタイムの情報と、遮蔽されたエリアの情報を提供します。
信号機認識結果：各信号機の現在の状態をリアルタイムで提供します。計画コンポーネントは、計画された経路に関連する情報を抽出し、交差点で停止するかどうかを決定します。
ローカリゼーションから
車両運動情報: 自車両の位置、速度、加速度、およびその他の運動関連データが含まれます。
システムから
動作モード: 車両が自律モードで動作しているかどうかを示します。
ヒューマン マシン インターフェイス (HMI) から
機能の実行: 人間のオペレーターによる、車線変更や交差点への進入などの自動運転操作の実行/承認を可能にします。
API レイヤーから
目的地 (目標): 計画コンポーネントが到達することを目指す最終的な位置を表します。
チェックポイント: 目的地までのルート上の中間点を表します。これはルート計算時に使用されます。
速度制限: 車両の最高速度制限を設定します。
計画コンポーネントからの出力
制御するには
軌道: コントロール コンポーネントが従う必要があるポーズ、ツイスト、加速のスムーズなシーケンスを提供します。軌跡の長さは通常 10 秒、分解能は 0.1 秒です。
方向指示器: 計画された操作に基づいて、右、左、ハザードなどの車両の方向指示器を制御します。
システムへ
診断: 計画コンポーネントの状態をレポートし、処理が正しく実行されているかどうか、安全な計画が生成されているかどうかを示します。
ヒューマン・マシン・インターフェース(HMI)へ
機能実行の可用性: 車線変更や交差点への進入など、実行できる操作または必要な操作のステータスを示します。
軌道候補: ユーザーの実行後に実行される可能性のある軌道を示します。
API層へ
計画要因: 現在の計画動作の背後にある理由に関する情報を提供します。これには、回避すべき目標物の位置、停止の決定に至った障害物、およびその他の関連情報が含まれる場合があります。
計画コンポーネントの内部インターフェース
ミッションプランニングからシナリオプランニングまで
ルート: 出発地から目的地までたどる必要がある経路のガイダンスを提供します。この経路は、地図上に定義されたレーンIDなどの情報に基づいて決定されます。ルート レベルでは、どの特定の車線を使用するかは明示的に示されず、ルートには複数の車線が含まれる場合があります。
行動計画から動作計画へ
パス: 車両がたどる大まかな位置と速度を提供します。これらのパス ポイントは通常、約 1 メートルの間隔で定義されます。他の間隔距離も可能ですが、計画コンポーネントの精度やパフォーマンスに影響を与える可能性があります。
走行可能エリア: 車線内や物理的に走行可能なエリアなど、車両が走行できる領域を定義します。モーション プランナーがこの定義された領域内で最終的な軌道を計算することを前提としています。
シナリオの計画から検証まで
軌道: コントロール コンポーネントが追従しようとする目的の位置、速度、加速度を定義します。軌道点は軌道速度に基づいて約0.1秒間隔で定義されます。
コンポーネントを制御するための検証
軌道: 上記と同じですが、追加の安全上の考慮事項がいくつかあります。
きめ細かなデザイン
サポートされている機能
特徴	説明	要件	形	デモンストレーション
ルート計画	自車位置から目的地までのルートを計画します。リファレンス実装はMission Planner

にあり、ノードを起動することで有効になります。mission_planner	- レーンレットマップ（レーンレットの運転）	ルート計画	
ルートからのパス計画	指定されたルートからたどる経路を計画します。

参照実装はBehavior Path Plannerにあります。	- レーンレットマップ（レーンレットの運転）	車線追従	
障害物回避	ステアリング操作で障害物を回避するように経路を計画します。

参照実装は、Avoidance、Obstacleavoidance Plannerにあります。パラメータ内のフラグを有効にする:launch obstacle_avoidance_planner true	- オブジェクト情報	障害物回避	デモビデオ
デモビデオ
パスの平滑化	スムーズなステアリングを実現するために経路を計画します。

参照実装は障害物回避プランナーにあります。	- レーンレットマップ（走行レーンレット）	パスの平滑化	デモビデオ
デモビデオ
狭所走行	走行可能エリア内で走行するための経路を計画します。また、走行可能領域内を走行できない場合は、車両を停止し、走行可能領域から出ないようにしてください。

参照実装は障害物回避プランナーにあります。	- 車線図（高精度車線境界線）	狭いスペースでの運転	デモビデオ
デモビデオ
車線変更	目的地に到達するための車線変更の経路を計画します。

リファレンス実装はLane Changeにあります。	- レーンレットマップ（レーンレットの運転）	車線変更	デモビデオ
デモビデオ
引っ張る	路肩に車を停めるための経路を計画します。

参照実装はGoal Plannerにあります。	- レーンレットマップ（路肩レーン）	引っ張る	デモビデオ:
シンプルなプルオーバー アーク 前方プルオーバーアーク後方プルオーバー
デモビデオ

デモビデオ

デモビデオ
引き出す	車寄せのための経路を路肩から開始するように計画します。

参照実装はPull Out Moduleにあります。	- レーンレットマップ（路肩レーン）	引き出す	デモビデオ:
シンプルな引き出し、 後方引き出し
デモビデオ

デモビデオ
パスシフト	外部からの指示に応じて横方向の経路を計画します。

リファレンス実装はSide Shift Moduleにあります。	- なし	サイドシフト	
障害物停止	経路上の障害物に合わせて停止する速度を計画します。

リファレンス実装は、Obstacle Stop Planner、Obstacle Cruise Plannerにあります。launch obstacle_stop_plannerおよび有効フラグ: TODO、launch obstacle_cruise_plannerおよび有効フラグ:TODO	- オブジェクト情報	障害物停止	デモビデオ
デモビデオ
障害物の減速	経路の周囲にある障害物に対して減速するように速度を計画します。

リファレンス実装は、Obstacle Stop Planner、Obstacle Cruise Plannerにあります。	- オブジェクト情報	障害物減速	デモビデオ
デモビデオ
アダプティブクルーズコントロール	自車両の前を走行する車両に追従するように速度を計画します。

リファレンス実装は、Obstacle Stop Planner、Obstacle Cruise Plannerにあります。	- オブジェクト情報	アダプティブクルーズ	
割り込み車両には減速する	車両が自車線に割り込むリスクを回避するために速度を計画してください。

リファレンス実装はObstacle Cruise Plannerにあります。	- オブジェクト情報	割り込む	
起動時のサラウンドチェック	車両の周囲に障害物がある場合に移動を防止するために速度を計画します。

リファレンス実装はSurround Obstacle Checkerにあります。パラメータのフラグを有効にする:use_surround_obstacle_check true tier4_planning_component.launch.xml <	- オブジェクト情報	サラウンドチェック	デモビデオ
デモビデオ
カーブの減速	カーブでは減速するように速度を計画します。

リファレンス実装はMotion Velocity Smootherにあります。	- なし	カーブでの減速	
障害物に対するカーブの減速	経路周囲の障害物との衝突の危険を考慮して、カーブでは減速するように速度を計画します。

リファレンス実装はObstacle Velocity Limiterにあります。	- オブジェクト情報
- レーンレットマップ（静的障害物）	カーブ上の障害物減速	デモビデオ
デモビデオ
横断歩道	横断歩道に近づいたり歩いたりする歩行者が停止または減速する速度を計画します。

参照実装はCrosswalk モジュールにあります。	- オブジェクト情報
- レーンレットマップ（横断歩道）	横断歩道	デモビデオ
デモビデオ
交差点対向車確認	対向車との危険を避けるために、交差点での右折または左折の速度を計画します。

参照実装はIntersection モジュールにあります。	・オブジェクト情報
・レーンレットマップ（交差点車線、譲り車線）	交差点	デモビデオ
デモビデオ
交差点死角チェック	死角の後ろから来る他の車両やバイクとの危険を避けるために、交差点で右折または左折するときの速度を計画してください。

リファレンス実装はBlind Spot Moduleにあります。	・オブジェクト情報
・レーンレットマップ（交差点車線）	盲点	デモビデオ
デモビデオ
交差点のオクルージョンチェック	オクルージョンエリアから車両が来る可能性があるリスクを回避するために、交差点での右折または左折の速度を計画します。

参照実装はIntersection モジュールにあります。	・オブジェクト情報
・レーンレットマップ（交差点車線）	交差オクルージョン	デモビデオ
デモビデオ
交差点の渋滞検知	渋滞で前方に車両が停止している場合、交差点に進入しないように交差点の速度を計画してください。

参照実装はIntersection モジュールにあります。	・オブジェクト情報
・レーンレットマップ（交差点車線）	交差点渋滞	デモビデオ
デモビデオ
信号機	信号に従って交差点の速度を計画します。

リファレンス実装はTraffic Light Moduleにあります。	- 信号機の色情報	信号機	デモビデオ
デモビデオ
振れチェック	近くの物体が経路に飛び出してしまう可能性を考慮して、速度を減速するように計画します。

リファレンス実装はRun Out Moduleにあります。	- オブジェクト情報	なくなる	デモビデオ
デモビデオ
停止線	停止線で停止するように計画速度を設定します。

リファレンス実装はStop Line Moduleにあります。	- レーンレットマップ（停止線）	停止線	デモビデオ
デモビデオ
オクルージョンスポットチェック	大型車両の後ろなどからオブジェクトがオクルージョン エリアから飛び出してくる場合に減速するように速度を計画します。

リファレンス実装はOcclusion Spot Moduleにあります。	- オブジェクト情報
- レーンレットマップ（プライベートレーン/パブリックレーン）	オクルージョンスポット	デモビデオ
デモビデオ
停車禁止エリア	消防署出入り口前など、停止禁止区域では停止しないように速度を計画してください。

リファレンス実装はNo Stopping Area モジュールにあります。	- レーンレットマップ（停車エリアなし）	一時停止禁止区域	
私有地から公道への合流	歩行者や他の車両との衝突の危険を避けるために、私道から公道に入るときの速度を計画してください。

リファレンス実装はMerge from Private Area モジュールにあります。	- オブジェクト情報
- レーンレットマップ（プライベートレーン/パブリックレーン）	WIP	
スピードバンプ	スピードバンプでは減速するように速度を計画します。

リファレンス実装はSpeed Bump Moduleにあります。	- レーンレットマップ（スピードバンプ）	スピードバンプ	デモビデオ
デモビデオ
検知エリア	指定された検出エリアに物体が存在する場合、対応する停止位置で停止するように計画速度を設定します。

参照実装は検出エリア モジュールにあります。	- レーンレットマップ（検知エリア）	検出エリア	デモビデオ
デモビデオ
走行可能な車線なし	ODD (運用設計ドメイン) で指定されたエリアを出る前に停止するように速度を計画するか、ODD 車線外で自律モードが開始された場合に車両を停止します。

リファレンス実装はNo Drivable Lane Moduleにあります。	- Laneletマップ（走行可能な車線なし）	走行不能車線	
車線逸脱時の衝突検知	自車両が自分の車線から逸脱したときに、別の車線を走行している他の車両との衝突を避けるために速度を計画します。

リファレンス実装はOut of Lane Moduleにあります。	・オブジェクト情報
・レーンレットマップ（走行車線）	WIP	
駐車場	駐車エリアでの特定の目標に向けて経路と速度を計画します。

リファレンス実装はFree Space Plannerにあります。	- オブジェクト情報
- レーンレットマップ（駐車場）	駐車場	デモビデオ
デモビデオ
自動緊急ブレーキ (AEB)	前方の物体との衝突が予想される場合は、緊急停止してください。この機能は最終的な安全層として期待されており、ローカリゼーションまたは知覚システムに障害が発生した場合でも機能するはずであることに注意してください。

リファレンス実装はOut of Lane Moduleにあります。	- プリミティブオブジェクト	aeb	
最小リスク行動 (MRM)	危険な事象が発生した場合には、適切な MRM (最小リスク行動) 指示を提供します。例えば、センサーの異常が発見された場合、状況に応じて緊急ブレーキ、適度な停止、路肩寄せなどの指示を出します。

リファレンス実装は TODO 内にあります	- TODO	WIP	
軌道の検証	計画された軌道が安全であることを確認してください。安全でない場合は、軌道を変更する、軌道の送信を停止する、自動運転システムに報告するなどの適切な措置を講じます。

参照実装はPlanning Validatorにあります。	- なし	軌道検証	
レーンマップ生成の実行中	手動運転で記録された位置特定データからレーンマップを生成します。

リファレンス実装は WIP にあります	- なし	WIP	
ランニングレーンの最適化	車両の運動学を考慮してマップの中心線 (基準パス) を最適化して滑らかにします。

参照実装はStatic Centerline Optimizerにあります。	- Laneletマップ（走行車線）	WIP	
リファレンス実装
次の図は、計画コンポーネントのリファレンス実装を示しています。新しいモジュールを追加したり、機能を拡張したりすることで、さまざまな ODD をサポートできます。

一部の実装は、実装の難しさのために高レベルのアーキテクチャ設計に準拠しておらず、更新が必要であることに注意してください。

リファレンス実装

詳細は各パッケージの設計資料をご参照ください。

Mission_planner : 地図情報をもとにスタートからゴールまでのルートを計算します。
behaviour_path_planner : 交通規則に基づいて経路と走行可能エリアを計算します。
車線追従
車線変更
回避
引っ張る
引き出す
サイドシフト
behaviour_velocity_planner : 交通ルールに基づいて最大速度を計算します。
検知エリア
盲点
横断歩道
停止線
交通信号
交差点
停止エリアなし
仮想トラフィックライト
オクルージョンスポット
なくなる
Disaster_avoidance_planner : 障害物と走行可能領域の制約の下で経路形状を計算します
around_obstacle_checker : 自車両の周囲に障害物がある場合に車両を停止させ続けます。車両停止時のみ作動します。
障害物_stop_planner : 軌道上またはその近くに障害物がある場合、停止、減速、適応クルーズ (車に追従) の状況に応じて軌道ポイントの最大速度を計算します。
停止
減速する
アダプティブクルーズ
costmap_generator : 動的オブジェクトとレーン情報からパス生成のためのコストマップを生成します。
freespace_planner : フリースペース シーンの実現可能性 (曲率など) を考慮して軌道を計算します。アルゴリズムについては、ここで説明します。
scenario_selector : 現在のシナリオに従って軌道を選択します。
external_velocity_limit_selector : 複数の候補から適切な速度制限を取得します。
motion_velocity_smoother : 速度、加速度、ジャーク制約を考慮して最終速度を計算します。
現在の実装における重要な情報
上位設計との大きな違いは、「シナリオ層の導入」と「動作と動作の明確な分離」です。これらは、現在のパフォーマンスと実装上の課題により導入されました。これらを高レベル設計の一部として定義するか、実装の一部として改善するかは議論の問題です。

シナリオプランニングレイヤーの紹介
きちんと構造化された車線での運転と、駐車場などの空きスペースでの運転との間のインターフェースには、異なる要件があります。たとえば、レーン ドライビングではマップ ID を使用してルートを処理できますが、これは空きスペースでの計画には適していません。計画サブコンポーネントをシナリオ レベル (車線走行、駐車など) で切り替えるメカニズムにより、インターフェイスの柔軟な設計が可能になりますが、異なるシナリオ間でモジュールを再利用するという欠点があります。

行動と運動の分離
プランニングの古典的なアプローチの 1 つは、アクションを決定する「行動」と、最終的な動きを決定する「モーション」に分割することです。ただし、機能の分離が進むとパフォーマンスが低下する傾向があるため、この分離はパフォーマンスとのトレードオフを意味します。たとえば、Behavior は、Motion が最終的に実行する計算についての事前知識なしで意思決定を行う必要があるため、一般に保守的な意思決定が行われます。一方で、動作と動作を統合すると動作性能と意思決定が相互依存するため、地域の交通ルールに合わせて意思決定機能のみを拡張したい場合など、拡張性の点で課題が生じます。

この背景を理解するには、以前に説明したこの文書が役立つかもしれません。

現在の実装の機能をカスタマイズする
現在の実装ではモジュール レベルの機能を追加することは可能ですが、すべての機能に対応する統一インターフェイスは提供されていません。現在の実装におけるモジュール レベルでの拡張方法について簡単に説明します。

参照実装-新しいモジュールの追加

Behavior_velocity_planner または Behavior_path_plnner に新しいモジュールを追加します
Behavior_path_plannerやBehavior_velocity_plannerなどの ROS ノードには、プラグインを通じて使用できるモジュール インターフェイスがあります。これらのROSノードで定義されたモジュールインターフェースに従ってモジュールを追加することで、モジュールの動的なロード/アンロードが可能になります。モジュールを追加する具体的な方法については、各パッケージのドキュメントを参照してください。

計画コンポーネントに新しい ros ノードを追加します。
モーションプランニングにモジュールを追加する場合、モジュールをROSノードとして作成し、プランニングコンポーネントに統合する必要があります。現在の構成では、上流で計算された目標軌道に情報を追加する処理が行われており、このプロセスにROS Nodeを導入することで機能拡張が可能となっています。

シナリオを追加または置き換える
現在の実装では、複数のモジュールを一括して切り替える方法として、シナリオレベルの切り替えロジックを導入しています。これにより、新しいシナリオ (高速道路での運転など) を追加できます。

シナリオを ros ノードとして作成し、scenario_selector ros ノードをそれに合わせることで統合が完了します。この利点は、他のシナリオ (車線走行など) の実装に影響を与えることなく、重要な新機能を導入できることです。ただし、シナリオの切り替えによるシナリオ レベルの調整のみが可能であり、既存の計画モジュール レベルでの調整は可能ではありません。
# Planning component design

## Purpose

The Planning Component in autonomous driving systems plays a crucial role in generating a target trajectory (path and speed) for autonomous vehicles. It ensures safety and adherence to traffic rules, fulfilling specific missions.

This document outlines the planning requirements and design within Autoware, aiding developers in comprehending the design and extendibility of the Planning Component.

The document is divided into two parts: the first part discusses high-level requirements and design, and the latter part focuses on actual implementations and functionalities provided.

## Goals and non-goals

Our objective extends beyond merely developing an autonomous driving system. We aim to offer an "autonomous driving platform" where users can enhance autonomous driving functionalities based on their individual needs.

In Autoware, we utilize the [microautonomy architecture](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-concepts) concept, which emphasizes high extensibility, functional modularity, and clearly defined interfaces.

With this in mind, the design policy for the Planning Component is focused not on addressing every complex autonomous driving scenario (as that is a very challenging problem), but on **providing a customizable and easily extendable Planning development platform**. We believe this approach will allow the platform to meet a wide range of needs, ultimately solving many complex use cases.

To clarify this policy, the Goals and Non-Goals are defined as follows:

**Goals:**

- **The basic functions are provided so that a simple ODD can be defined**
  - Before extending its functionalities, the Planning Component must provide the essential features necessary for autonomous driving. This encompasses basic operations like moving, stopping, and turning, as well as handling lane changes and obstacle avoidance in relatively safe and simple contexts.
- **The functionality is modularized for user-driven extension**
  - The system is designed to adapt to various Operational Design Domains (ODDs) with extended functionalities. Modularization, akin to plug-ins, allows for creating systems tailored to diverse needs, such as different levels of autonomous driving and varied vehicular or environmental applications (e.g., Lv4/Lv2 autonomous driving, public/private road driving, large vehicles, small robots).
  - Reducing functionalities for specific ODDs, like obstacle-free private roads, is also a key aspect. This modular approach allows for reductions in power consumption or sensor requirements, aligning with specific user needs.
- **The capability is extensible with the decision of human operators**
  - Incorporating operator assistance is a critical aspect of functional expansion. It means that the system can adapt to complex and challenging scenarios with human support. The specific type of operator is not defined here. It might be a person accompanying in the vehicle during the prototype development phase or a remote operator connected in emergencies during autonomous driving services.

**Non-goals:**

The Planning Component is designed to be extended with third-party modules. Consequently, the following are not the goals of Autoware's Planning Component:

- To provide all user-required functionalities by default.
- To provide complete functionality and performance characteristic of an autonomous driving system.
- To provide performance that consistently surpasses human capabilities or ensures absolute safety.

These aspects are specific to our vision of an autonomous driving "platform" and may not apply to a typical autonomous driving Planning Component.

## High level design

This diagram illustrates the high-level architecture of the Planning Component. This represents an idealized design, and current implementations might vary. Further details on implementations are provided in the latter sections of this document.

![overall-planning-architecture](image/high-level-planning-diagram.drawio.svg)

Following the principles of microautonomy architecture, we have adopted a modular system framework. The functions within the Planning domain are implemented as modules, dynamically or statically adaptable according to specific use cases. This includes modules for lane changes, intersection handling, and pedestrian crossings, among others.

The Planning Component comprises several sub-components:

- **Mission Planning**: This module calculates routes from the current location to the destination, utilizing map data. Its functionality is similar to that of Fleet Management Systems (FMS) or car navigation route planning.
- **Planning Modules**: These modules plans the vehicle's behavior for the assigned mission, including target trajectory, blinker signaling, etc. They are divided into Behavior and Motion categories:
  - **Behavior**: Focuses on calculating safe and rule-compliant routes, managing decisions for lane changes, intersection entries, and stoppings at a stop line.
  - **Motion**: Works in cooperate with Behavior modules to determine the vehicle's trajectory, considering its motion and ride comfort. It includes lateral and longitudinal planning for route shaping and speed calculation.
- **Validation**: Ensures the safety and appropriateness of the planned trajectories, with capabilities for emergency response. In cases where a planned trajectory is unsuitable, it triggers emergency protocols or generates alternative paths.

### Highlights

Key aspects of this high-level design include:

#### Modulation of each function

Essential Planning functions, such as route generation, lane changes, and intersection management, are modularized. These modules come with standardized interfaces, enabling easy addition or modification. More details on these interfaces will be discussed in subsequent sections. You can see the details about how to enable/disable each module in [the implementation documentation of Planning](https://autowarefoundation.github.io/autoware.universe/main/planning/#how-to-enable-or-disable-planning-module).

#### Separation of Mission Planning sub-component

Mission Planning serves as a substitute for functions typically found in existing services like FMS (Fleet Management System). Adherence to defined interfaces in the high-level design facilitates easy integration with third-party services.

#### Separation of Validation sub-component

Given the extendable nature of the Planning Component, ensuring consistent safety levels across all functions is challenging. Therefore, the Validation function is managed independently from the core planning modules, maintaining a baseline of safety even for arbitrary changes of the planning modules.

#### Interface for HMI (Human Machine Interface)

The HMI is designed for smooth cooperation with human operators. These interfaces enable coordination between the Planning Component and operators, whether in-vehicle or remote.

#### Trade-offs for the separation of planning and other components

In Autoware's overarching design, the separation of components like Planning, Perception, Localization, and Control facilitates cooperation with third-party modules. However, this separation entails trade-offs between performance and extensibility. For instance, the Perception component might process unnecessary objects due to its separation from Planning. Similarly, separating planning and control can pose challenges in accounting for vehicle dynamics during planning. To mitigate these issues, we might need to enhance interface information or increase computational efforts.

## Customize features

A significant feature of the Planning Component design is its ability to integrate with external modules. The diagram below shows various methods for incorporating external functionalities.

![how-to-add-new-modules](image/how-to-add-new-modules.drawio.svg)

### 1. Adding New Modules to the Planning Component

Users can augment or replace existing Planning functionalities with new modules. This approach is commonly used for extending features, allowing for the addition of capabilities absent in the desired ODD or simplification of existing features.

However, adding these functionalities requires well-organized module interfaces. As of November 2023, an ideal modular system is not fully established, presenting some limitations. For more information, please refer to the Reference Implementation section [Customize features in the current implementation](#customize-features-in-the-current-implementation) and [the implementation documentation of Planning](https://autowarefoundation.github.io/autoware.universe/main/planning/#how-to-enable-or-disable-planning-module).

### 2. Replacing Sub-components of Planning

Collaboration and extension at the sub-component level may interest some users. This could involve replacing Mission Planning with an existing FMS service or incorporating a third-party trajectory generation module while utilizing the existing Validation functionality.

Adhering to the [Internal interface in the planning component](#internal-interface-in-the-planning-component), collaboration and extension at this level are feasible. While complex coordination with existing Planning features may be limited, it allows for integration between certain Planning Component functionalities and external modules.

### 3. Replacing the Entire Planning Component

Organizations or research entities developing autonomous driving Planning systems might be interested in integrating their proprietary Planning solutions with Autoware's Perception or Control modules. This can be achieved by replacing the entire Planning system, following the robust and stable interfaces defined between components. It is important to note, however, that direct coordination with existing Planning modules might not be possible.

## Component interface

This section describes the inputs and outputs of the Planning Component and of its internal modules. See the [Planning Component Interface](../../autoware-interfaces/components/planning.md) page for the current implementation.

### Input to the planning component

- **From Map**
  - Vector map: Contains all static information about the environment, including lane connection information for route planning, lane geometry for generating a reference path, and traffic rule-related information.
- **From Perception**
  - Detected object information: Provides real-time information about objects that cannot be known in advance, such as pedestrians and other vehicles. The Planning Component plans maneuvers to avoid collisions with these objects.
  - Detected obstacle information: Supplies real-time information about the location of obstacles, which is more primitive than Detected Object and used for emergency stops and other safety measures.
  - Occupancy map information: Offers real-time information about the presence of pedestrians and other vehicles and occluded area information.
  - Traffic light recognition result: Provides the current state of each traffic light in real time. The Planning Component extracts relevant information for the planned path and determines whether to stop at intersections.
- **From Localization**
  - Vehicle motion information: Includes the ego vehicle's position, velocity, acceleration, and other motion-related data.
- **From System**
  - Operation mode: Indicates whether the vehicle is operating in Autonomous mode.
- **From Human Machine Interface (HMI)**
  - Feature execution: Allows for executing/authorizing autonomous driving operations, such as lane changes or entering intersections, by human operators.
- **From API Layer**
  - Destination (Goal): Represents the final position that the Planning Component aims to reach.
  - Checkpoint: Represents a midpoint along the route to the destination. This is used during route calculation.
  - Velocity limit: Sets the maximum speed limit for the vehicle.

### Output from the planning component

- **To Control**
  - Trajectory: Provides a smooth sequence of pose, twist, and acceleration that the Control Component must follow. The trajectory is typically 10 seconds long with a 0.1-second resolution.
  - Turn Signals: Controls the vehicle's turn indicators, such as right, left, hazard, etc. based on the planned maneuvers.
- **To System**
  - Diagnostics: Reports the state of the Planning Component, indicating whether the processing is running correctly and whether a safe plan is being generated.
- **To Human Machine Interface (HMI)**
  - Feature execution availability: Indicates the status of operations that can be executed or are required, such as lane changes or entering intersections.
  - Trajectory candidate: Shows the potential trajectory that will be executed after the user's execution.
- **To API Layer**
  - Planning factors: Provides information about the reasoning behind the current planning behavior. This may include the position of target objects to avoid, obstacles that led to the decision to stop, and other relevant information.

### Internal interface in the planning component

- **Mission Planning to Scenario Planning**
  - Route: Offers guidance for the path that needs to be followed from the starting point to the destination. This path is determined based on information such as lane IDs defined on the map. At the route level, it doesn't explicitly indicate which specific lanes to take, and the route can contain multiple lanes.
- **Behavior Planning to Motion Planning**
  - Path: Provides a rough position and velocity to be followed by the vehicle. These path points are usually defined with an interval of about 1 meter. Although other interval distances are possible, it may impact the precision or performance of the planning component.
  - Drivable area: Defines regions where the vehicle can drive, such as within lanes or physically drivable areas. It assumes that the motion planner will calculate the final trajectory within this defined area.
- **Scenario Planning to Validation**
  - Trajectory: Defines the desired positions, velocities, and accelerations which the Control Component will try to follow. Trajectory points are defined at intervals of approximately 0.1 seconds based on the trajectory velocities.
- **Validation to Control Component**
  - Trajectory: Same as above but with some additional safety considerations.

## Detailed design

### Supported features

| Feature                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Requirements                                                                | Figure                                                                          | Demonstration                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Route Planning                               | Plan route from the ego vehicle position to the destination.<br> <br> Reference implementation is in [Mission Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/mission_planner/), enabled by launching the `mission_planner` node.                                                                                                                                                                                                                                                                          | - Lanelet map (driving lanelets)                                            | ![route-planning](image/features-route-planning.drawio.svg)                     |
| Path Planning from Route                     | Plan path to be followed from the given route. <br> <br> Reference implementation is in [Behavior Path Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/).                                                                                                                                                                                                                                                                                                                            | - Lanelet map (driving lanelets)                                            | ![lane-follow](image/features-lane-follow.drawio.svg)                           |
| Obstacle Avoidance                           | Plan path to avoid obstacles by steering operation. <br> <br> Reference implementation is in [Avoidance](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/docs/behavior_path_planner_avoidance_design/), [Obstacle Avoidance Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_avoidance_planner/). Enable flag in parameter: `launch obstacle_avoidance_planner true`                                                                                    | - objects information                                                       | ![obstacle-avoidance](image/features-avoidance.drawio.svg)                      | [Demonstration Video](https://youtu.be/A_V9yvfKZ4E) <br> [![Demonstration Video](https://img.youtube.com/vi/A_V9yvfKZ4E/0.jpg)](https://www.youtube.com/watch?v=A_V9yvfKZ4E)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Path Smoothing                               | Plan path to achieve smooth steering. <br> <br> Reference implementation is in [Obstacle Avoidance Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_avoidance_planner/).                                                                                                                                                                                                                                                                                                                           | - Lanelet map (driving lanelet)                                             | ![path-smoothing](image/features-path-smoothing.drawio.svg)                     | [Demonstration Video](https://youtu.be/RhyAF26Ppzs) <br> [![Demonstration Video](https://img.youtube.com/vi/RhyAF26Ppzs/0.jpg)](https://www.youtube.com/watch?v=RhyAF26Ppzs)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Narrow Space Driving                         | Plan path to drive within the drivable area. Furthermore, when it is not possible to drive within the drivable area, stop the vehicle to avoid exiting the drivable area. <br> <br> Reference implementation is in [Obstacle Avoidance Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_avoidance_planner/).                                                                                                                                                                                       | - Lanelet map (high-precision lane boundaries)                              | ![narrow-space-driving](image/features-narrow-space-driving.drawio.svg)         | [Demonstration Video](https://youtu.be/URzcLO2E1vY) <br> [![Demonstration Video](https://img.youtube.com/vi/URzcLO2E1vY/0.jpg)](https://www.youtube.com/watch?v=URzcLO2E1vY)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Lane Change                                  | Plan path for lane change to reach the destination. <br> <br> Reference implementation is in [Lane Change](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/docs/behavior_path_planner_lane_change_design/).                                                                                                                                                                                                                                                                                   | - Lanelet map (driving lanelets)                                            | ![lane-change](image/features-lane-change.drawio.svg)                           | [Demonstration Video](https://youtu.be/0jRDGQ84cD4) <br> [![Demonstration Video](https://img.youtube.com/vi/0jRDGQ84cD4/0.jpg)](https://www.youtube.com/watch?v=0jRDGQ84cD4)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Pull Over                                    | Plan path for pull over to park at the road shoulder. <br> <br> Reference implementation is in [Goal Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/docs/behavior_path_planner_goal_planner_design/).                                                                                                                                                                                                                                                                               | - Lanelet map (shoulder lane)                                               | ![pull-over](image/features-pull-over.drawio.svg)                               | Demonstration Videos: <br> [Simple Pull Over](https://youtu.be/r3-kAmTb4hc) <br> [![Demonstration Video](https://img.youtube.com/vi/r3-kAmTb4hc/0.jpg)](https://www.youtube.com/watch?v=r3-kAmTb4hc) <br> [Arc Forward Pull Over](https://youtu.be/ornbzkWxRWU) <br> [![Demonstration Video](https://img.youtube.com/vi/ornbzkWxRWU/0.jpg)](https://www.youtube.com/watch?v=ornbzkWxRWU) <br> [Arc Backward Pull Over](https://youtu.be/if-0tG3AkLo) <br> [![Demonstration Video](https://img.youtube.com/vi/if-0tG3AkLo/0.jpg)](https://www.youtube.com/watch?v=if-0tG3AkLo) |
| Pull Out                                     | Plan path for pull over to start from the road shoulder. <br> <br> Reference implementation is in [Pull Out Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/docs/behavior_path_planner_start_planner_design/#:~:text=WIP-,Path%20Generation,-%23).                                                                                                                                                                                                                                    | - Lanelet map (shoulder lane)                                               | ![pull-out](image/features-pull-out.drawio.svg)                                 | Demonstration Video: <br> [Simple Pull Out](https://youtu.be/xOjnPqoHup4) <br> [![Demonstration Video](https://img.youtube.com/vi/xOjnPqoHup4/0.jpg)](https://www.youtube.com/watch?v=xOjnPqoHup4) <br> [Backward Pull Out](https://youtu.be/iGieijPcPcQ) <br> [![Demonstration Video](https://img.youtube.com/vi/iGieijPcPcQ/0.jpg)](https://www.youtube.com/watch?v=iGieijPcPcQ)                                                                                                                                                                                            |
| Path Shift                                   | Plan path in lateral direction in response to external instructions. <br> <br> Reference implementation is in [Side Shift Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/docs/behavior_path_planner_side_shift_design/).                                                                                                                                                                                                                                                             | - None                                                                      | ![side-shift](image/features-side-shift.drawio.svg)                             |
| Obstacle Stop                                | Plan velocity to stop for an obstacle on the path. <br> <br> Reference implementation is in [Obstacle Stop Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_stop_planner/), [Obstacle Cruise Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_cruise_planner/). `launch obstacle_stop_planner` and enable flag: `TODO`, `launch obstacle_cruise_planner` and enable flag: `TODO`                                                                             | - objects information                                                       | ![obstacle-stop](image/features-obstacle-stop.drawio.svg)                       | [Demonstration Video](https://youtu.be/d8IRW_xArcE) <br> [![Demonstration Video](https://img.youtube.com/vi/d8IRW_xArcE/0.jpg)](https://www.youtube.com/watch?v=d8IRW_xArcE)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Obstacle Deceleration                        | Plan velocity to decelerate for an obstacle located around the path. <br> <br> Reference implementation is in [Obstacle Stop Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_stop_planner/), [Obstacle Cruise Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_cruise_planner/).                                                                                                                                                                            | - objects information                                                       | ![obstacle-decel](image/features-obstacle-decel.drawio.svg)                     | [Demonstration Video](https://youtu.be/gvN1otgeaaw) <br> [![Demonstration Video](https://img.youtube.com/vi/gvN1otgeaaw/0.jpg)](https://www.youtube.com/watch?v=gvN1otgeaaw)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Adaptive Cruise Control                      | Plan velocity to follow the vehicle driving in front of the ego vehicle. <br> <br> Reference implementation is in [Obstacle Stop Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_stop_planner/), [Obstacle Cruise Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_cruise_planner/).                                                                                                                                                                        | - objects information                                                       | ![adaptive-cruise](image/features-adaptive-cruise.drawio.svg)                   |
| Decelerate for cut-in vehicles               | Plan velocity to avoid a risk for cutting-in vehicle to ego lane. <br> <br> Reference implementation is in [Obstacle Cruise Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_cruise_planner/).                                                                                                                                                                                                                                                                                                     | - objects information                                                       | ![cut-in](image/features-cut-in.drawio.svg)                                     |
| Surround Check at starting                   | Plan velocity to prevent moving when an obstacle exists around the vehicle. <br> <br> Reference implementation is in [Surround Obstacle Checker](https://autowarefoundation.github.io/autoware.universe/main/planning/surround_obstacle_checker/). Enable flag in parameter: `use_surround_obstacle_check true` in [tier4_planning_component.launch.xml](https://github.com/autowarefoundation/autoware_launch/blob/2850d7f4e20b173fde2183d5323debbe0067a990/autoware_launch/launch/components/tier4_planning_component.launch.xml#L8) < | - objects information                                                       | ![surround-check](image/features-surround-check.drawio.svg)                     | [Demonstration Video](https://youtu.be/bbGgtXN3lC4) <br> [![Demonstration Video](https://img.youtube.com/vi/bbGgtXN3lC4/0.jpg)](https://www.youtube.com/watch?v=bbGgtXN3lC4)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Curve Deceleration                           | Plan velocity to decelerate the speed on a curve. <br> <br> Reference implementation is in [Motion Velocity Smoother](https://autowarefoundation.github.io/autoware.universe/main/planning/motion_velocity_smoother/).                                                                                                                                                                                                                                                                                                                   | - None                                                                      | ![decel-on-curve](image/features-decel-on-curve.drawio.svg)                     |
| Curve Deceleration for Obstacle              | Plan velocity to decelerate the speed on a curve for a risk of obstacle collision around the path. <br> <br> Reference implementation is in [Obstacle Velocity Limiter](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_velocity_limiter/).                                                                                                                                                                                                                                                                | - objects information <br> - Lanelet map (static obstacle)                  | ![decel-on-curve-obstacles](image/features-decel-on-curve-obstacles.drawio.svg) | [Demonstration Video](https://youtu.be/I-oFgG6kIAs) <br> [![Demonstration Video](https://img.youtube.com/vi/I-oFgG6kIAs/0.jpg)](https://www.youtube.com/watch?v=I-oFgG6kIAs)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Crosswalk                                    | Plan velocity to stop or decelerate for pedestrians approaching or walking on a crosswalk. <br> <br> Reference implementation is in [Crosswalk Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_crosswalk_module/).                                                                                                                                                                                                                                                                        | - objects information <br> - Lanelet map (pedestrian crossing)              | ![crosswalk](image/features-crosswalk.drawio.svg)                               | [Demonstration Video](https://youtu.be/tUvthyIL2W8) <br> [![Demonstration Video](https://img.youtube.com/vi/tUvthyIL2W8/0.jpg)](https://www.youtube.com/watch?v=tUvthyIL2W8)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Intersection Oncoming Vehicle Check          | Plan velocity for turning right/left at intersection to avoid a risk with oncoming other vehicles. <br> <br> Reference implementation is in [Intersection Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_intersection_module/).                                                                                                                                                                                                                                                          | - objects information <br> - Lanelet map (intersection lane and yield lane) | ![intersection](image/features-intersection.drawio.svg)                         | [Demonstration Video](https://youtu.be/SGD07Hqg4Hk) <br> [![Demonstration Video](https://img.youtube.com/vi/SGD07Hqg4Hk/0.jpg)](https://www.youtube.com/watch?v=SGD07Hqg4Hk)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Intersection Blind Spot Check                | Plan velocity for turning right/left at intersection to avoid a risk with other vehicles or motorcycles coming from behind blind spot. <br> <br> Reference implementation is in [Blind Spot Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_blind_spot_module/).                                                                                                                                                                                                                          | - objects information <br> - Lanelet map (intersection lane)                | ![blind-spot](image/features-blind-spot.drawio.svg)                             | [Demonstration Video](https://youtu.be/oaTCJRafDGA) <br> [![Demonstration Video](https://img.youtube.com/vi/oaTCJRafDGA/0.jpg)](https://www.youtube.com/watch?v=oaTCJRafDGA)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Intersection Occlusion Check                 | Plan velocity for turning right/left at intersection to avoid a risk with the possibility of coming vehicles from occlusion area. <br> <br> Reference implementation is in [Intersection Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_intersection_module/).                                                                                                                                                                                                                           | - objects information <br> - Lanelet map (intersection lane)                | ![intersection-occlusion](image/features-intersection-occlusion.drawio.svg)     | [Demonstration Video](https://youtu.be/bAHXMB7kbFc) <br> [![Demonstration Video](https://img.youtube.com/vi/bAHXMB7kbFc/0.jpg)](https://www.youtube.com/watch?v=bAHXMB7kbFc)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Intersection Traffic Jam Detection           | Plan velocity for intersection not to enter the intersection when a vehicle is stopped ahead for a traffic jam. <br> <br> Reference implementation is in [Intersection Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_intersection_module/).                                                                                                                                                                                                                                             | - objects information <br> - Lanelet map (intersection lane)                | ![intersection-traffic-jam](image/features-intersection-traffic-jam.drawio.svg) | [Demonstration Video](https://youtu.be/negK4VbrC5o) <br> [![Demonstration Video](https://img.youtube.com/vi/negK4VbrC5o/0.jpg)](https://www.youtube.com/watch?v=negK4VbrC5o)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Traffic Light                                | Plan velocity for intersection according to a traffic light signal. <br> <br> Reference implementation is in [Traffic Light Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_traffic_light_module/).                                                                                                                                                                                                                                                                                       | - Traffic light color information                                           | ![traffic-light](image/features-traffic-light.drawio.svg)                       | [Demonstration Video](https://youtu.be/lGA53KljQrM) <br> [![Demonstration Video](https://img.youtube.com/vi/lGA53KljQrM/0.jpg)](https://www.youtube.com/watch?v=lGA53KljQrM)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Run-out Check                                | Plan velocity to decelerate for the possibility of nearby objects running out into the path. <br> <br> Reference implementation is in [Run Out Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_run_out_module/).                                                                                                                                                                                                                                                                          | - objects information                                                       | ![run-out](image/features-run-out.drawio.svg)                                   | [Demonstration Video](https://youtu.be/9IDggldT2t0) <br> [![Demonstration Video](https://img.youtube.com/vi/9IDggldT2t0/0.jpg)](https://www.youtube.com/watch?v=9IDggldT2t0)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Stop Line                                    | Plan velocity to stop at a stop line. <br> <br> Reference implementation is in [Stop Line Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_stop_line_module/).                                                                                                                                                                                                                                                                                                                             | - Lanelet map (stop line)                                                   | ![stop-line](image/features-stop-line.drawio.svg)                               | [Demonstration Video](https://youtu.be/eej9jYt-GSE) <br> [![Demonstration Video](https://img.youtube.com/vi/eej9jYt-GSE/0.jpg)](https://www.youtube.com/watch?v=eej9jYt-GSE)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Occlusion Spot Check                         | Plan velocity to decelerate for objects running out from occlusion area, for example, from behind a large vehicle. <br> <br> Reference implementation is in [Occlusion Spot Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_occlusion_spot_module/).                                                                                                                                                                                                                                      | - objects information <br> - Lanelet map (private/public lane)              | ![occlusion-spot](image/features-occlusion-spot.drawio.svg)                     | [Demonstration Video](https://youtu.be/3qs8Ivjh1fs) <br> [![Demonstration Video](https://img.youtube.com/vi/3qs8Ivjh1fs/0.jpg)](https://www.youtube.com/watch?v=3qs8Ivjh1fs)                                                                                                                                                                                                                                                                                                                                                                                                  |
| No Stop Area                                 | Plan velocity not to stop in areas where stopping is prohibited, such as in front of the fire station entrance. <br> <br> Reference implementation is in [No Stopping Area Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_no_stopping_area_module/).                                                                                                                                                                                                                                     | - Lanelet map (no stopping area)                                            | ![no-stopping-area](image/features-no-stopping-area.drawio.svg)                 |
| Merge from Private Area to Public Road       | Plan velocity for entering the public road from a private driveway to avoid a risk of collision with pedestrians or other vehicles. <br> <br> Reference implementation is in [Merge from Private Area Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_intersection_module/).                                                                                                                                                                                                              | - objects information <br> - Lanelet map (private/public lane)              | WIP                                                                             |
| Speed Bump                                   | Plan velocity to decelerate for speed bumps. <br> <br> Reference implementation is in [Speed Bump Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_speed_bump_module/).                                                                                                                                                                                                                                                                                                                    | - Lanelet map (speed bump)                                                  | ![speed-bump](image/features-speed-bump.drawio.svg)                             | [Demonstration Video](https://youtu.be/FpX3q3YaaCw) <br> [![Demonstration Video](https://img.youtube.com/vi/FpX3q3YaaCw/0.jpg)](https://www.youtube.com/watch?v=FpX3q3YaaCw)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Detection Area                               | Plan velocity to stop at the corresponding stop when an object exist in the designated detection area. <br> <br> Reference implementation is in [Detection Area Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_detection_area_module/).                                                                                                                                                                                                                                                  | - Lanelet map (detection area)                                              | ![detection-area](image/features-detection-area.drawio.svg)                     | [Demonstration Video](https://youtu.be/YzXF4U69lJs) <br> [![Demonstration Video](https://img.youtube.com/vi/YzXF4U69lJs/0.jpg)](https://www.youtube.com/watch?v=YzXF4U69lJs)                                                                                                                                                                                                                                                                                                                                                                                                  |
| No Drivable Lane                             | Plan velocity to stop before exiting the area designated by ODD (Operational Design Domain) or stop the vehicle if autonomous mode started in out of ODD lane. <br> <br> Reference implementation is in [No Drivable Lane Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_no_drivable_lane_module/).                                                                                                                                                                                      | - Lanelet map (no drivable lane)                                            | ![no-drivable-lane](image/features-no-drivable-lane.drawio.svg)                 |
| Collision Detection when deviating from lane | Plan velocity to avoid conflict with other vehicles driving in the another lane when the ego vehicle is deviating from own lane. <br> <br> Reference implementation is in [Out of Lane Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_out_of_lane_module/).                                                                                                                                                                                                                              | - objects information <br> - Lanelet map (driving lane)                     | WIP                                                                             |
| Parking                                      | Plan path and velocity for given goal in parking area. <br> <br> Reference implementation is in [Free Space Planner](https://autowarefoundation.github.io/autoware.universe/main/planning/freespace_planner/).                                                                                                                                                                                                                                                                                                                           | - objects information <br> - Lanelet map (parking area)                     | ![parking](image/features-parking.drawio.svg)                                   | [Demonstration Video](https://youtu.be/rAIYmwpNWfA) <br> [![Demonstration Video](https://img.youtube.com/vi/rAIYmwpNWfA/0.jpg)](https://www.youtube.com/watch?v=rAIYmwpNWfA)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Autonomous Emergency Braking (AEB)           | Perform an emergency stop if a collision with an object ahead is anticipated. It is noted that this function is expected as a final safety layer, and this should work even in the event of failures in the Localization or Perception system. <br> <br> Reference implementation is in [Out of Lane Module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_out_of_lane_module/).                                                                                                                | - Primitive objects                                                         | ![aeb](image/features-aeb.drawio.svg)                                           |
| Minimum Risk Maneuver (MRM)                  | Provide appropriate MRM (Minimum Risk Maneuver) instructions when a hazardous event occurs. For example, when a sensor trouble found, send an instruction for emergency braking, moderate stop, or pulling over to the shoulder, depending on the severity of the situation. <br> <br> Reference implementation is in TODO                                                                                                                                                                                                               | - TODO                                                                      | WIP                                                                             |
| Trajectory Validation                        | Check the planned trajectory is safe. If it is unsafe, take appropriate action, such as modify the trajectory, stop sending the trajectory or report to the autonomous driving system. <br> <br> Reference implementation is in [Planning Validator](https://autowarefoundation.github.io/autoware.universe/main/planning/planning_validator/).                                                                                                                                                                                          | - None                                                                      | ![trajectory-validation](image/features-trajectory-validation.drawio.svg)       |
| Running Lane Map Generation                  | Generate lane map from localization data recorded in manual driving. <br> <br> Reference implementation is in WIP                                                                                                                                                                                                                                                                                                                                                                                                                        | - None                                                                      | WIP                                                                             |
| Running Lane Optimization                    | Optimize the centerline (reference path) of the map to make it smooth considering the vehicle kinematics. <br> <br> Reference implementation is in [Static Centerline Optimizer](https://autowarefoundation.github.io/autoware.universe/main/planning/static_centerline_optimizer/).                                                                                                                                                                                                                                                     | - Lanelet map (driving lanes)                                               | WIP                                                                             |

<!-- ![supported-functions](image/planning-functions.drawio.svg) -->

### Reference Implementation

The following diagram describes the reference implementation of the Planning component. By adding new modules or extending the functionalities, various ODDs can be supported.

_Note that some implementation does not adhere to the high-level architecture design due to the difficulties of the implementation and require updating._

![reference-implementation](image/planning-diagram-tmp.drawio.svg)

For more details, please refer to the design documents in each package.

- [_mission_planner_](https://autowarefoundation.github.io/autoware.universe/main/planning/mission_planner/): calculate route from start to goal based on the map information.
- [_behavior_path_planner_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/): calculates path and drivable area based on the traffic rules.
  - [_lane_following_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/#lane-following)
  - [_lane_change_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/#lane-change)
  - [_avoidance_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/#avoidance)
  - [_pull_over_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/#pull-over)
  - [_pull_out_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/#pull-out)
  - _side_shift_
- [_behavior_velocity_planner_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_planner/): calculates max speed based on the traffic rules.
  - [_detection_area_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_detection_area_module/docs/detection-area-design/)
  - [_blind_spot_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_blind_spot_module/docs/blind-spot-design/)
  - [_cross_walk_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_crosswalk_module/docs/crosswalk-design/)
  - [_stop_line_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_stop_line_module/docs/stop-line-design/)
  - [_traffic_light_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_traffic_light_module/docs/traffic-light-design/)
  - [_intersection_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_intersection_module/docs/intersection-design/)
  - [_no_stopping_area_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_no_stopping_area_module/docs/no-stopping-area-design/)
  - [_virtual_traffic_light_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_virtual_traffic_light_module/docs/virtual-traffic-light-design/)
  - [_occlusion_spot_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_occlusion_spot_module/docs/occlusion-spot-design/)
  - [_run_out_](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_run_out_module/docs/run-out-design/)
- [_obstacle_avoidance_planner_](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_avoidance_planner/): calculate path shape under obstacle and drivable area constraints
- [_surround_obstacle_checker_](https://autowarefoundation.github.io/autoware.universe/main/planning/surround_obstacle_checker/): keeps the vehicle being stopped when there are obstacles around the ego-vehicle. It works only when the vehicle is stopped.
- [_obstacle_stop_planner_](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_stop_planner/): When there are obstacles on or near the trajectory, it calculates the maximum velocity of the trajectory points depending on the situation: stopping, slowing down, or adaptive cruise (following the car).
  - [_stop_](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_stop_planner/#obstacle-stop-planner_1)
  - [_slow_down_](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_stop_planner/#slow-down-planner)
  - [_adaptive_cruise_](https://autowarefoundation.github.io/autoware.universe/main/planning/obstacle_stop_planner/#adaptive-cruise-controller)
- [_costmap_generator_](https://autowarefoundation.github.io/autoware.universe/main/planning/costmap_generator/): generates a costmap for path generation from dynamic objects and lane information.
- [_freespace_planner_](https://autowarefoundation.github.io/autoware.universe/main/planning/freespace_planner/): calculates trajectory considering the feasibility (e.g. curvature) for the freespace scene. Algorithms are described [here](https://autowarefoundation.github.io/autoware.universe/main/planning/freespace_planning_algorithms/).
- _scenario_selector_ : chooses a trajectory according to the current scenario.
- [_external_velocity_limit_selector_](https://autowarefoundation.github.io/autoware.universe/main/planning/external_velocity_limit_selector/): takes an appropriate velocity limit from multiple candidates.
- [_motion_velocity_smoother_](https://autowarefoundation.github.io/autoware.universe/main/planning/motion_velocity_smoother/): calculates final velocity considering velocity, acceleration, and jerk constraints.

### Important information in the current implementation

An important difference compared to the high-level design is the "introduction of the scenario layer" and the "clear separation of behavior and motion." These are introduced due to current performance and implementation challenges. Whether to define these as part of the high-level design or to improve them as part of the implementation is a matter of discussion.

#### Introducing the Scenario Planning layer

There are different requirements for interfaces between driving in well-structured lanes and driving in a free-space area like a parking lot. For example, while Lane Driving can handle routes with map IDs, this is not appropriate for planning in free space. The mechanism that switches planning sub-components at the scenario level (Lane Driving, Parking, etc) enables a flexible design of the interface, however, it has a drawbacks of the reuse of modules across different scenarios.

#### Separation of Behavior and Motion

One of the classic approach to Planning involves dividing it into "Behavior", which decides the action, and "Motion", which determines the final movement. However, this separation implies a trade-off with performance, as performance tends to degrade with increasing separation of functions. For example, Behavior needs to make decisions without prior knowledge of the computations that Motion will eventually perform, which generally results in conservative decision-making. On the other hand, if behavior and motion are integrated, motion performance and decision-making become interdependent, creating challenges in terms of expandability, such as when you wish to extend only the decision-making function to follow a regional traffic rules.

To understand this background, this [previously discussed document](https://github.com/tier4/AutowareArchitectureProposal.proj/blob/main/docs/design/software_architecture/Planning/DesignRationale.md) may be useful.

### Customize features in the current implementation

While it is possible to add module-level functionalities in the current implementation, a unified interface for all functionalities is not provided. Here's a brief explanation of the methods of extending at the module level in the current implementation.

![reference-implementation-add-new-modules](image/reference-implementation-add-new-modules.drawio.svg)

#### Add new modules in behavior_velocity_planner or behavior_path_plnner

ROS nodes such as [behavior_path_planner](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/) and [behavior_velocity_planner](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_planner/) have a module interface available through plugins. By adding modules in accordance with the module interfaces defined in these ROS nodes, dynamic loading/unloading of modules becomes possible. For specific methods of adding modules, please refer to the documentation of each package.

#### Add a new ros node in the planning component

When adding modules in Motion Planning, it is necessary to create the module as a ROS Node and integrate it into the planning component. The current configuration involves adding information to the target trajectory calculated upstream, and the introduction of a ROS Node in this process allows for the expansion of functionality.

#### Add or replace with scenarios

The current implementation has introduced a scenario-level switching logic as a method for collectively switching multiple modules. This allows for the addition of new scenarios (e.g., highway driving).

By creating a scenario as a ros node and aligning the scenario_selector ros node with it, the integration is complete. The benefit of this is that you can introduce significant new functionalities without affecting the implementation of other scenarios (like Lane Driving). However, it only allows for scenario-level coordination through scenario switching and does not enable coordination at the existing planning module level.

<!--
### Important Parameters

| Package                      | Parameter                                                     | Type   | Description                                                                                                        |
| ---------------------------- | ------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------ |
| `obstacle_stop_planner`      | `stop_planner.stop_position.max_longitudinal_margin`          | double | distance between the ego and the front vehicle when stopping (when `cruise_planner_type:=obstacle_stop_planner`)   |
| `obstacle_cruise_planner`    | `common.safe_distance_margin`                                 | double | distance between the ego and the front vehicle when stopping (when `cruise_planner_type:=obstacle_cruise_planner`) |
| `behavior_path_planner`      | `avoidance.avoidance.lateral.lateral_collision_margin`        | double | minimum lateral margin to obstacle on avoidance                                                                    |
| `behavior_path_planner`      | `avoidance.avoidance.lateral.lateral_collision_safety_buffer` | double | additional lateral margin to obstacle if possible on avoidance                                                     |
| `obstacle_avoidance_planner` | `option.enable_outside_drivable_area_stop`                    | bool   | If set true, a stop point will be inserted before the path footprint is outside the drivable area.                 |

### Notation

#### [1] self-crossing road and overlapped

To support the self-crossing road and overlapped road in the opposite direction, each planning module has to meet the [specifications](https://autowarefoundation.github.io/autoware.universe/main/common/motion_utils/)

Currently, the supported modules are as follows.

- lane_following (in behavior_path_planner)
- detection_area (in behavior_velocity_planner)
- stop_line (in behavior_velocity_planner)
- virtual_traffic_light (in behavior_velocity_planner)
- obstacle_avoidance_planner
- obstacle_stop_planner
- motion_velocity_smoother

#### [2] Size of Path Points

Some functions do not support paths with only one point. Therefore, each modules should generate the path with more than two path points. -->
