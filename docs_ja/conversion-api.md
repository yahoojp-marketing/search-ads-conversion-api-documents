# Conversion API
Yahoo!広告 検索広告 コンバージョン計測API

## 仕様
### サーバ
機能リリース日に公開予定

### リクエスト制限
50 requests / sec<br>
リクエスト制限を超えたリクエストは処理されず、エラーレスポンスが返却されます。

### POST /v1
#### リクエストヘッダ

| Key | Value |
| ---- | ---- |
| User-Agent | `Yahoo AppID: <AppID>` |
| Content-Type | `application/json` |

`<AppID>` にはYahoo!デベロッパーネットワークで取得したアプリケーションID(Client ID)を指定してください。<br>
詳細は https://developer.yahoo.co.jp/start/ を御覧ください。<br>
Conversion API ではアプリケーションID発行時の「ID連携利用有無」は「ID連携を利用しない」で利用可能です。<br>
**アプリケーションID(Client ID)は第三者に開示しないようにしてください。**<br>

#### リクエストボディ
必須列に`*`があるパラメータは必須です。<br>
また、以下のいずれかの条件を満たすことが必須です：
- 【条件1】yclid を指定している
- 【条件2】sa_p, sa_t, sa_ra の3項目をすべて指定している（sa_cc は任意）

| key              | DataType      |必須　 |条件付き必須　　　　　| 入力値　　　　　　　　　　　　　　　　　　| Value 例                    |
|-------------------------|--------|------|--------|----------------------------------------------------------------------|-----------------------------|
| `yclid`                   | 文字列  | -    | 条件1 | 広告をクリックしたユーザーを識別するパラメータ。広告URLの`yclid`の値。 | `YSS.1234567890.Ab12CdEfGhIJ345kLm_N_oPq`         |
| `sa_p`                    | 文字列  | -    | 条件2 | 広告をクリックしたユーザーを識別するパラメータ。広告URLの`sa_p`の値。  | `YSA`                         |
| `sa_cc`                   | 文字列  | -    | 条件2 | 広告をクリックしたユーザーを識別するパラメータ。広告URLの`sa_cc`の値。 | `1234567890`                  |
| `sa_t`                    | 文字列  | -    | 条件2 | 広告をクリックしたユーザーを識別するパラメータ。広告URLの`sa_t`の値。  | `1754368953900`               |
| `sa_ra`                   | 文字列  | -    | 条件2 | 広告をクリックしたユーザーを識別するパラメータ。広告URLの`sa_ra`の値。 | `A1`                          |
| `yahoo_conversion_id`     | 文字列  | *   | -    | コンバージョンタグ内の同名フィールドの値。      | `1234567890`                  |
| `yahoo_conversion_label`  | 文字列  | *   | -    | コンバージョンタグ内の同名フィールドの値。      | `a1B2C3dE4FgH5IJK6_lMno7`        |
| `yahoo_conversion_value`  | 整数    | -    | -     | コンバージョンタグ内の同名フィールドの値。     | `10`                          |
| `event_time`              | 整数    | *   | -     | コンバージョンの発生日時。クリック発生日時から計測期間内の10桁、または13桁のUNIX時間で入力してください。 | `1753421286937` |
| `url`                     | 文字列  | *   | -    | コンバージョンが発生したページのURL。                   | `https://example.com/page1`   |
| `referrer`                | 文字列  | -    | -    | コンバージョンが発生したページのHTTPリファラ。            | `https://ref.example.com`     |
| `user_agent`              | 文字列  | *   | -    | コンバージョンしたユーザーのブラウザのユーザーエージェント。 | `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36`             |
| `ip`                      | 文字列  | *   | -    | コンバージョンしたユーザーのIPアドレス。                 | `192.168.0.1`               |

#### リクエストサンプル
curl を用いてリクエストする場合

```
$ curl -i \
-H "User-Agent: Yahoo AppID: a1Bc23DeFg45HiJk67LmNo89PqRsTuVwXyZaBc12DeFgHiJk34LmNoPq-" \
-H "Content-Type: application/json" \
-X POST \
-d '{
    "yclid": "YSS.1234567890.Ab12CdEfGhIJ345kLm_N_oPq",
    "yahoo_conversion_id": "1234567890",
    "yahoo_conversion_label": "a1B2C3dE4FgH5IJK6_lMno7",
    "event_time": 1753421286937,
    "url": "https://example.com/page1/?yclid=YSS.1234567890.Ab12CdEfGhIJ345kLm_N_oPq&sa_p=YSA&sa_cc=1234567890&sa_t=1754368953900&sa_ra=A1",
    "ip": "192.168.0.1",
    "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36"
}' \
https://example.com
```

#### レスポンス

##### 成功
| レスポンスコード | レスポンスボディ | 補足 |
| ---- | ---- | ---- |
| 200 | (無し) | 本レスポンスの場合でも、不正な値の場合や重複判定となった場合はコンバージョンの計測対象外になります。 |

##### 失敗
| レスポンスコード | 主な原因 |
|------------------|----------|
| 400 | リクエストが不正な場合に発生します。特にリクエストボディがJSONとして解釈できない場合や入力値が不正な場合が考えられます。 |
| 401 | アプリケーションIDが正しく付与されていない場合に発生します。 |
| 403 | アプリケーションIDが不正な場合に発生します。 |
| 404 | リクエストパスが不正な場合、POST 以外のメソッドでリクエストした場合に発生します。 |
| 415 | Content-Type の指定が不正な場合に発生します。 |
| 429 | アプリケーションIDごとに設定されているアクセス回数上限に達した場合に発生します。 |
| 500 | 内部エラーが起きた場合に発生します。再度操作を実行してください。 |
| 503 | APIがメンテナンス中などの理由により利用できない状態です。 |

エラーレスポンスのボディには詳細な理由を含む場合があります。一部を除き以下の形式でレスポンスします。

```
{
  "Error" : {
    "Message" : "<error message>"
  }
}
```

## お問い合わせ
お問い合わせについてはこちらをご参照下さい。<br>
https://www.lycbiz.com/jp/contact/support/yahoo-ads/
