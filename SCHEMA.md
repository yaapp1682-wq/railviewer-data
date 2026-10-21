# railways.db スキーマ

my鉄道マップ が同梱する鉄道データベースのスキーマです。
行数は 2026-10-13 時点のビルド（リリース v1.0.7・アプリ 1.1.1 同梱）のもので、リリースにより変動します。

## lines（路線 / 652行）

名寄せ・統合後の「1 本の路線」。

| 列 | 型 | 内容 |
| --- | --- | --- |
| line_id | INTEGER PK | 路線ID |
| display_name | TEXT | 表示名（例: 山手線） |
| normalized_name | TEXT | 検索用正規化名 |
| yomi | TEXT | ふりがな（OSM name:ja-Hira 由来。無い場合あり） |
| rail_type | TEXT | 種別: shinkansen / jr / subway / private / monorail / tram / light_rail / funicular |
| operator | TEXT | 事業者名 |
| network | TEXT | 路線網（OSM network タグ） |
| line_code | TEXT | 路線記号（JY 等） |
| colour | TEXT | 路線色（#RRGGBB。OSM colour タグ由来） |
| priority | INTEGER | 表示優先度（本アプリ用の独自指標） |
| wikipedia / wikidata | TEXT | 参照タグ（無い場合あり） |
| way_count | INTEGER | 構成 way 数 |
| length_m | REAL | 総延長（m） |
| min_lat / max_lat / min_lon / max_lon | REAL | バウンディングボックス |
| topology | TEXT | 位相分類: linear / loop / branch / split |
| section_count | INTEGER | 区間数（split の場合 2 以上） |
| name_en | TEXT | 英語名（例: Yamanote Line。無い場合あり） |
| name_fold / name_en_fold | TEXT | 検索用に表記をそろえた名前（例: 山手線 / yamanoteline） |

## line_segments（描画セグメント / 121,628行）

OSM way 単位のジオメトリ。地図描画に使用。

| 列 | 型 | 内容 |
| --- | --- | --- |
| segment_id | INTEGER PK | セグメントID |
| line_id | INTEGER | 所属路線 |
| osm_way_id | INTEGER | 元の OSM way ID |
| railway | TEXT | OSM railway タグ（rail / subway / disused 等） |
| service | TEXT | OSM service タグ（siding / yard 等。本線は NULL） |
| tunnel / bridge | INTEGER | トンネル / 橋フラグ |
| length_m | REAL | 長さ（m） |
| point_count | INTEGER | 頂点数 |
| geometry_blob | BLOB | 頂点列。little-endian float32 の (lat, lon) 連続（`<Nf`） |
| min_lat / max_lat / min_lon / max_lon | REAL | バウンディングボックス |

## stations（駅 / 9,148行）

| 列 | 型 | 内容 |
| --- | --- | --- |
| station_id | INTEGER PK | 駅ID |
| display_name | TEXT | 表示名 |
| normalized_name | TEXT | 検索用正規化名 |
| yomi | TEXT | ふりがな（無い場合あり） |
| lat / lon | REAL | 座標 |
| station_kind | TEXT | station / halt |
| operator | TEXT | 事業者名 |
| is_interchange | INTEGER | 乗換駅フラグ（複数路線接続） |
| line_count | INTEGER | 接続路線数 |
| wikipedia / wikidata | TEXT | 参照タグ（無い場合あり） |
| name_en | TEXT | 英語名（例: Shinjuku。OSM name:en を基に整えたもの。無い場合あり） |
| name_fold / name_en_fold | TEXT | 検索用に表記をそろえた名前（例: 渋谷 / shibuya） |
| name_lc / yomi_lc | TEXT | 現地の言葉の名前・読み（海外の路線データ用。日本では空） |

## line_stations（路線⇔停車駅 / 11,534行）

路線ごとの停車駅リスト（順序付き）。

| 列 | 型 | 内容 |
| --- | --- | --- |
| line_id | INTEGER | 路線ID |
| station_id | INTEGER | 駅ID |
| stop_order | INTEGER | 区間内の停車順 |
| lat / lon | REAL | 駅座標（非正規化コピー） |
| chainage_m | REAL | 路線起点からのキロ程（m） |
| section | INTEGER | 区間番号（分断路線は複数区間） |

## routes / route_stops（経路 / 829行・12,118行）

OSM のルートリレーション由来の停車順。line_stations と併用。

| routes 列 | 内容 |
| --- | --- |
| route_id / line_id | ルートID / 所属路線 |
| name / from_station / to_station | ルート名・起終点 |
| length_m / is_loop / point_count | 長さ・環状フラグ・頂点数 |
| geometry_blob | 頂点列（line_segments と同形式） |
| min_lat 〜 max_lon | バウンディングボックス |

| route_stops 列 | 内容 |
| --- | --- |
| route_id / seq / station_id / chainage_m | ルートID・順序・駅ID・キロ程 |

## line_adjacency（駅隣接グラフ / 10,893行）

線路ジオメトリから構築した駅の隣接関係。路線詳細の
分岐・環状・分断表示（位相表示）に使用。

| 列 | 内容 |
| --- | --- |
| line_id | 路線ID |
| a_station_id / b_station_id | 隣接する駅のペア |
| dist_m | 駅間の線路沿い距離（m・近似） |

## line_codes（路線記号 / 325行）

| 列 | 内容 |
| --- | --- |
| line_id / code / code_label / network | 路線ID・記号（JY 等）・ラベル・路線網 |

## line_aliases（別名 / 0行）

検索用の別名（現ビルドでは未使用）。

## operator_names（事業者の英語名 / 291行）

lines.operator / stations.operator の書き方ごとの会社名と英語名。

| 列 | 内容 |
| --- | --- |
| operator | DB の operator の値（PK） |
| company_ja | 会社名（複数社は ; 区切り） |
| en | 英語の正式名（例: East Japan Railway Company） |
| short_en | 短い英語名（例: JR East） |

## train_paths（電車の走る線 / 786行）

アプリの電車アニメーション用に、経路ごとに線をつないだもの（2026-10 追加）。

| 列 | 型 | 内容 |
| --- | --- | --- |
| line_id / path_index | INTEGER PK | 路線と、その路線の何本目の線か |
| route_id | INTEGER | 元にした経路（無い場合は 0） |
| is_loop | INTEGER | 環状フラグ |
| auto_trains | INTEGER | 架空ダイヤの電車を走らせるか |
| points / cum | BLOB | 線の頂点と、各頂点までの距離（アプリ用に圧縮した形式） |
| chain / station_ids | BLOB | 停車駅のキロ程（m・little-endian float64）と駅ID（little-endian int64） |

## train_lanes（電車の走る線路 / 785行）

電車が上り・下りでそれぞれの線路を走るように、train_paths の経路ごとに、線路が経路の線から横にどれだけずれているかを持つもの（2026-10 追加）。

| 列 | 型 | 内容 |
| --- | --- | --- |
| line_id / path_index | INTEGER PK | train_paths と同じ |
| start / step / len | REAL | 経路のキロ程（m）の始め・点の間隔・長さ |
| fwd / bwd | BLOB | キロ程が増える向き・減る向きに走る電車の線路の横のずれ（m。アプリ用に圧縮した形式） |
| lfwd / lbwd | BLOB | 各駅に止まる電車用の線路（本線と違う所だけ。無い場合あり） |

## train_single（単線の区間 / 490行）

train_paths の経路ごとの、単線の区間と行き違いできる駅（2026-10 追加）。

| 列 | 型 | 内容 |
| --- | --- | --- |
| line_id / path_index | INTEGER PK | train_paths と同じ |
| len | REAL | 経路の長さ（m） |
| runs | BLOB | 単線の区間（始め・終わりのキロ程 m の組。little-endian float64） |
| pass_ids | BLOB | 単線の区間の中で行き違いできる駅ID（little-endian int64） |

## line_overview / line_overview2（縮小表示用の線 / 30,887行・30,887行）

地図を広く表示したとき用に、線の点を間引いたもの（2026-10 追加）。
列は ov_id（PK）・line_id・geometry_blob（line_segments と同じ形式）・min_lat / max_lat / min_lon / max_lon。
line_overview がより粗く、line_overview2 はその中間の縮尺用です。

## station_grams（駅名検索用 / 103,615行）

駅名（表示名・読み・英語名など）の 2 文字ずつの組と駅ID（gram, station_id）。駅名検索の索引です（2026-10 追加）。

## db_meta

ビルド情報（生成日時・元データ等）の key-value。

## R\*Tree インデックス

- `lines_rtree`（rowid = line_id）
- `segments_rtree`（rowid = segment_id）
- `routes_rtree`（rowid = route_id）
- `overview_rtree` / `overview2_rtree`（rowid = ov_id）

いずれも min_lat / max_lat / min_lon / max_lon による空間検索用です。
