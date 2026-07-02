# railviewer-data

**my鉄道マップ**（iOS）が同梱する鉄道データベース（railways.db）の公開リポジトリです。

このデータベースは [OpenStreetMap](https://www.openstreetmap.org/) のデータを
加工して作成した派生データベースであり、
[Open Database License (ODbL) 1.0](https://opendatacommons.org/licenses/odbl/1-0/) に基づき公開しています。

> © OpenStreetMap contributors
>
> Contains information from OpenStreetMap, which is made available
> under the Open Database License (ODbL).

- アプリ: my鉄道マップ（App Store リンクは公開後に掲載します）
- ライセンス全文: [LICENSE](LICENSE)
- スキーマ詳細: [SCHEMA.md](SCHEMA.md)
- アプリの[プライバシーポリシー](PRIVACY.md)・[サポート](SUPPORT.md)

## データのダウンロード

DB ファイル本体（railways.db）は **[Releases](../../releases) からダウンロード**してください。

| ファイル | サイズ | 内容 |
| --- | --- | --- |
| railways.db | 約 32 MB | 全国の鉄道 650 路線 / 駅 8,995 / 描画セグメント 126,053 本 |

**元データ**: [Geofabrik 日本抽出](https://download.geofabrik.de/asia/japan.html) `japan-260513.osm.pbf`（2026年5月13日時点）を加工しています。

## データの内容

OpenStreetMap の鉄道データ（日本全国）を、本アプリ向けに
「**1 本の路線**」単位へ名寄せ・統合した SQLite データベースです。

| テーブル | 行数 | 内容 |
| --- | --- | --- |
| lines | 650 | 路線（名寄せ・統合後の論理単位）。名称・ふりがな・種別・路線色・事業者・位相分類等 |
| line_segments | 126,053 | 描画用セグメント（OSM way 単位のジオメトリ） |
| stations | 8,995 | 駅（名称・座標・種別・事業者・乗換フラグ等） |
| line_stations | 11,887 | 路線⇔停車駅の対応（停車順・区間・キロ程付き） |
| routes / route_stops | 644 / 11,102 | OSM ルート関係由来の停車順 |
| line_adjacency | 11,673 | 駅の隣接グラフ（路線詳細の分岐・環状表示に使用） |
| line_codes | 328 | 路線記号（JY・M 等） |
| lines_rtree / segments_rtree / routes_rtree | — | R\*Tree 空間インデックス |

各テーブル・各列の定義は [SCHEMA.md](SCHEMA.md) を参照してください。

## 加工内容の概要

OSM 生データ（日本抽出）からの独自パイプラインによる主な加工:

1. **路線の名寄せ・統合** — 鉄道ルートリレーション（train/subway/tram 等）による
   グループ化を第一とし、事業者名の表記ゆれ・「本線↔線」等の名称ゆれによる
   重複路線を停車駅集合の重なりで統合
2. **駅の整備** — ホーム細分ノード（「○番のりば」等）の除去、
   表記ゆれ重複駅（末尾「駅」・ヶ↔ケ等）の統合、
   ルート関係が不完全な路線への駅の近接補完（nearest-line-wins 方式で
   並走他線の駅の混入を防止）
3. **位相グラフ生成** — 線路ジオメトリから駅の隣接グラフ（line_adjacency）を構築し、
   各路線を linear / loop / branch / split に分類。区間番号・停車順・キロ程を付与
4. **クレンジング** — 建設中・廃線・構造物リレーションの除外、
   運休線（線路が disused タグ）の救済、誤所属駅
   （私鉄ブランド駅が JR 線に混入等）の自動除去
5. **属性整備** — ふりがな、路線色（OSM colour タグ）、路線記号、事業者、
   種別分類（新幹線 / JR / 地下鉄 / 私鉄 / モノレール / 路面電車 /
   ライトレール / ケーブルカー）等の付与

## データの限界

- **元データは OpenStreetMap** です。OSM 上の入力漏れ・誤り（駅の欠落、
  名称の表記ゆれ、線路ジオメトリの欠損等）はそのまま反映されます
- 名寄せ・統合はヒューリスティックであり、本来別の路線が 1 本に統合される、
  または同一路線が分裂して収録される場合があります
- 一部路線には線路ジオメトリの欠損による分断が残っています
- 運行系統（湘南新宿ライン等）と線路名義の路線が併存する箇所があります
- priority 等のスコアは本アプリの表示用に設計した独自指標です
- 時刻表・運行情報は含まれません（アプリ内の電車アニメーションは
  実ダイヤではなく、地域・路線特性から推定した架空のダイヤです）
- ナビゲーション・測量等、正確性が要求される用途には適しません

## 更新方針

- OSM データの更新に追従した定期的な再ビルドは予定していますが、
  頻度は保証しません。各リリースに元データの取得時期を記載します
- スキーマは後方互換（列の追加のみ）を基本としますが、
  破壊的変更がある場合はリリースノートに明記します
- 誤りに気づかれた場合は Issue でご報告ください。本データベースには
  加工処理に起因する誤り（名寄せ・統合の誤判定等）が含まれる場合があります。
  元データ（OpenStreetMap）の情報不足による欠落は、OSM 本体の更新により
  将来のビルドで改善されることがあります

## ライセンス

このデータベースは [Open Database License (ODbL) 1.0](LICENSE) で提供します。

- 出典: © OpenStreetMap contributors
  (<https://www.openstreetmap.org/copyright>)
- 本データベースを利用・再配布する場合は ODbL の条件
  （出典表示・同ライセンスでの共有）に従ってください
