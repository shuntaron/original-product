## 設計

### ステップ1

1. 業務フロー
```mermaid
sequenceDiagram
  autoNumber
  actor em as Employee
  participant ap as AP
  participant db as DB
  participant ss as SaaS
  em->>ap: 勤務予実データ取得実行
  ap->>ss: 勤務予実データ取得要求
  ss->>ap: 勤務予実データ取得結果
  Note over db: 勤務予実データ
  ap->>db: 勤務予実データ追加要求
  db->>ap: 勤務予実データ追加結果
  ap->>em: 勤務予実データ取得結果
  em->>em: 勤務予定データ確認・手動修正
  em->>ap: 勤務実績データ自動入力実行
  ap->>ss: 勤務実績データ自動入力・登録
  ss->>ss: 勤務実績データ登録
  ss->>ap: 勤務実績データ登録完了応答
  ap->>ss: 勤務予実データ取得要求
  ss->>ap: 勤務予実データ取得結果
  ap->>db: 勤務予実データ追加要求
  db->>ap: 勤務予実データ追加結果
  db->>ap: 勤務予実データ追加結果
  em->>em: 勤務予実データの差分確認
```

2. 画面遷移図
```mermaid
graph LR
  %% スタイル
  classDef default fill: #fff, stroke: #333, stroke-width: 1px;
  style top fill: #fff,stroke: #333,stroke-width: 1px;
  style timeCards fill: #fff,stroke: #333,stroke-width: 1px;

  %% サブグラフ
  subgraph top [トップ画面]
    ログイン
    %% 新規登録
  end
  
  subgraph timeCards [勤怠管理画面]
    勤務予実取得
    勤務実績手動修正
    勤務実績自動登録
  end

  subgraph timeCardsUpdate [勤務実績手動修正画面]
    更新
  end
  
  %% アロー
  ログイン-->timeCards
  勤務実績手動修正-->timeCardsUpdate
  勤務予実取得--SaaS-->勤怠管理画面へ
  更新-->勤怠管理画面へ
  勤務実績自動登録--SaaS-->勤怠管理画面へ
```

3. ワイヤーフレーム
