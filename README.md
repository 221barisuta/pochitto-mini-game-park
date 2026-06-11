# ぽちっとミニゲームパーク v2

CanvasとWebAudioで動く、スマホ縦持ち向けのオリジナルミニゲーム集です。

- おちゃそそぎ名人
- くるくるレーン
- スポットライト・ダンス
- デイリーチャレンジ、ベスト保存、コンボ、フィーバー
- 結果画像生成とWeb Share
- PWA、オフライン起動

公開URL: https://221barisuta.github.io/pochitto-mini-game-park/

## ローカル確認

通常プレイだけなら `index.html` を直接開けます。Service Worker、PWA、Web Shareを確認するときはHTTPサーバーを使います。

```bash
python3 -m http.server 4173
```

## 設定の差し替え

`index.html` の `APP_CONFIG` を編集します。

```js
const APP_CONFIG = {
  analyticsProvider: "none", // none | ga4 | plausible
  ga4MeasurementId: "",
  plausibleDomain: "",
  rankingEndpoint: "",
  canonicalUrl: "https://221barisuta.github.io/pochitto-mini-game-park/"
};
```

- GA4: `analyticsProvider`を`ga4`にして`ga4MeasurementId`を設定
- Plausible: `analyticsProvider`を`plausible`にして`plausibleDomain`を設定
- OGPドメイン: `canonicalUrl`とHTML内の`og:url`、`og:image`、canonicalを変更
- ランキング: `rankingEndpoint`へPOSTを受けるAPIを設定

ランキングAPIには次のJSONを送ります。未設定・送信失敗時もゲーム結果には影響しません。

```json
{
  "version": 2,
  "gameId": "tea",
  "mode": "daily",
  "dailyKey": "2026-06-11",
  "seed": 123456,
  "score": 1250,
  "maxCombo": 12,
  "judgments": {
    "perfect": 3,
    "great": 4,
    "good": 2,
    "miss": 1
  },
  "durationMs": 25000
}
```

## ゲーム追加

`index.html` の登録ゾーンへ `registerGame({...})` を追加します。入力、ゲームループ、スコア、音声、保存はコアが管理するため、ゲーム側でDOMイベントや独自RAFを登録しないでください。
