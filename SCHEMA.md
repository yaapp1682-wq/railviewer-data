# railways.db スキーマ

my鉄道マップ が同梱する鉄道データベースのスキーマです。
行数は 2026-07-02 時点のビルドのもので、リリースにより変動します。

## lines（路線 / 650行）

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

## line_segments（描画セグメント / 126,053行）

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

## stations（駅 / 8,995行）

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

## line_stations（路線⇔停車駅 / 11,887行）

路線ごとの停車駅リスト（順序付き）。

| 列 | 型 | 内容 |
| --- | --- | --- |
| line_id | INTEGER | 路線ID |
| station_id | INTEGER | 駅ID |
| stop_order | INTEGER | 区間内の停車順 |
| lat / lon | REAL | 駅座標（非正規化コピー） |
| chainage_m | REAL | 路線起点からのキロ程（m） |
| section | INTEGER | 区間番号（分断路線は複数区間） |

## routes / route_stops（OSM ルート関係 / 641行・10,867行）

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

## line_adjacency（駅隣接グラフ / 11,675行）

線路ジオメトリから構築した駅の隣接関係。路線詳細の
分岐・環状・分断表示（位相表示）に使用。

| 列 | 内容 |
| --- | --- |
| line_id | 路線ID |
| a_station_id / b_station_id | 隣接する駅のペア |
| dist_m | 駅間の線路沿い距離（m・近似） |

## line_codes（路線記号 / 328行）

| 列 | 内容 |
| --- | --- |
| line_id / code / code_label / network | 路線ID・記号（JY 等）・ラベル・路線網 |

## line_aliases（別名 / 0行）

検索用の別名（現ビルドでは未使用）。

## db_meta

ビルド情報（生成日時・元データ等）の key-value。

## R\*Tree インデックス

- `lines_rtree`（rowid = line_id）
- `segments_rtree`（rowid = segment_id）
- `routes_rtree`（rowid = route_id）

いずれも min_lat / max_lat / min_lon / max_lon による空間検索用です。
