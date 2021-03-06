### とりあえず負荷分散
- LVS,ELBを使って複数のサーバに分散する
- サーバは仮想、コンテナで複数たてる
- 負荷分散されました
- 本当に?
↑図面用意

### 負荷分散について
- 負荷分散を設計してください、説明してくださいとなったときに説明できるようになりたい
- 何も考えずにサーバ台数を増やすことで解決する世界は理想だけど現実はそうとは限らない

### 負荷分散を使う前に考えること
負荷=処理をする
本当に負荷分散する必要があるのか
- 処理を減らす
  - アルゴリズムやクエリを改善する
- 処理速度を速くする
  - スケールアップ
  - CPUのコア数を増やすとかは処理の分散と捉えてもいいかも知れない
- 処理を分散する←コレ
  - スケールアウト

### 負荷分散で考えること
- なんの負荷(処理)を分散したいのか
- 本当に負荷は分散されるのか
- 本当に負荷は分散されるのか

### なんの負荷を分散したいのか
ボトルネック調査の話
- CPU
- ディスクIO
- ネットワーク
  - IOともいえるけど別に考えてもいいと思っている
- ボトルネックは移動するうえに完璧になくなることはない
  - 一回で勝負がつくわけではない
  - 次にどこにボトルネックが移動するのか考える
  - 更にその次は..とやるのはつらいので1~2回先ぐらいまでにしておく?

### 負荷は分散されているのか
- CPUが1コアに張り付く
  - NICのinterupt関連
  - epoll,kqueueなどのevent系使っているミドルウェア
```
top - 13:41:45 up 88 days, 15:26,  1 user,  load average: 0.79, 0.78, 0.77
Tasks: 117 total,   2 running, 115 sleeping,   0 stopped,   0 zombie
%Cpu0  : 95.9 us,  0.0 sy,  0.0 ni,  3.1 id,  0.0 wa,  0.0 hi,  1.0 si,  0.0 st
%Cpu1  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu2  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu3  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu4  :  0.0 us,  1.0 sy,  0.0 ni, 99.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu5  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu6  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu7  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   2047840 total,   560220 used,  1487620 free,    85884 buffers
KiB Swap:  2097148 total,        0 used,  2097148 free.   330612 cached Mem
```
- MySQLの更新系クエリをmasterに、参照系クエリをslaveに
  - readは分散されるけど、writeは分散されない
  - slaveにもレプリケーションで更新されるのでmaster,slaveのwriteのIOPSは変わらないことも

- その他にも...
  - シャーディングしたけど特定の1シャードのみ負荷が上がっている
  - 仮想マシンを分けたけど同じ仮想ブリッジにつながって同じNICで処理していた
  - 違う物理マシンに仮想マシンを分散したけど同じスイッチにつながっていてUPリンクの帯域が圧迫していた

### どんな負荷分散技術を使うか

### 負荷分散技術に必要な要素は何か

### 俺の考えた最強の負荷分散技術をつくる

