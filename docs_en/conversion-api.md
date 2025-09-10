# Conversion API  
Yahoo! JAPAN Ads Search Ads Conversion API

In case there are any discrepancies between the Japanese and English references, the Japanese version shall be considered correct.

## Specifications

### Server  
https://search-ads-cv.yahooapis.jp

### Request Limit  
50 requests / sec  

Requests exceeding the threshold will be ignored and will result in error responses.

### POST /v1

#### Request Header

| Key         | Value                          |
|-------------|--------------------------------|
| `User-Agent`  | `Yahoo AppID: <AppID>`         |
| `Content-Type`| `application/json`             |

For `<AppID>`, use Application ID　(Client ID) issued on Yahoo! Developer Network.<br>
Refer to https://developer.yahoo.co.jp/start/ for further details.<br>
Conversion API is available even for users who have selected "ID連携を利用しない (or No ID Linkage)" for "ID連携利用有無 (or ID Linkage options)" when issuing Application ID on Yahoo! Developer Network.<br>
**DO NOT share your Application ID (Client ID) with others.**

#### Request Body

Request Body must contain parameters with `*` symbols.<br>
Request Body must contain one of the following.

- **Condition 1**: `yclid` is specified  
- **Condition 2**: All of `sa_p`, `sa_t`, and `sa_ra` are specified (`sa_cc` is optional)

| Key                    | DataType | Required | Conditionally Required | input　　　　　　　　　　　　                                                                 | Value (example)                          |
|------------------------|-----------|----------|------------------------|-----------------------------------------------------------------------------|----------------------------------------|
| `yclid`                | string    | -        | Condition 1            | Parameter to identify users who have clicked ads. Value of `yclid` in the ad URL. | `YSS.1234567890.Ab12CdEfGhIJ345kLm_N_oPq` |
| `sa_p`                 | string    | -        | Condition 2            | Parameter to identify users who have clicked ads. Value of `sa_p` in the ad URL.  | `YSA`                                  |
| `sa_cc`                | string    | -        | Condition 2            | Parameter to identify users who have clicked ads. Value of `sa_cc` in the ad URL. | `1234567890`                           |
| `sa_t`                 | string    | -        | Condition 2            | Parameter to identify users who have clicked ads. Value of `sa_t` in the ad URL.  | `1754368953900`                        |
| `sa_ra`                | string    | -        | Condition 2            | Parameter to identify users who have clicked ads. Value of `sa_ra` in the ad URL. | `A1`                                   |
| `yahoo_conversion_id`  | string    | *        | -                      | Value from the corresponding field in the conversion tag.                  | `1234567890`                           |
| `yahoo_conversion_label` | string  | *        | -                      | Value from the corresponding field in the conversion tag.                  | `a1B2C3dE4FgH5IJK6_lMno7`              |
| `yahoo_conversion_value` | number  | -        | -                      | Value from the corresponding field in the conversion tag.                  | `10`                                   |
| `event_time`           | number   | *        | -                      | Date and time conversions occurred. Must be a 10- or 13-digit UNIX timestamp within the measurement period from the click. | `1753421286937`          |
| `url`                  | string    | *        | -                      | The URL where the conversion occurred.                                     | `https://example.com/page1`           |
| `referrer`             | string    | -        | -                      | The referrer where the conversion occurred.                                | `https://ref.example.com`             |
| `user_agent`           | string    | *        | -                      | The user agent of the user who triggered the conversion.                   | `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36` |
| `ip`                   | string    | *        | -                      | The IP address of the user who triggered the conversion.                   | `192.168.0.1`                          |

#### Sample Request

API Request for curl

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
https://search-ads-cv.yahooapis.jp/v1/
```

#### Response

##### Success
| Response Code | Response Body | note |
|------------------|----------------|-------|
| 200              | N/A        | Even with 200 response code, conversions with invalid inputs or duplicated conversions will be excluded from measurement. |

##### Failure
| Response Code | Common causes |
|------------------|------------|
| 400 | Occurs when the request is invalid. Common causes include when the body can not be interpreted as JSON or when inputs are invalid. |
| 401 | Occurs when Application ID is not properly set. |
| 403 | Occurs when Application ID is invalid. |
| 404 | Occurs when the request path is invalid or the request method is not POST. |
| 415 | Occurs when the input for Content-Type is invalid. |
| 429 | Occurs when the number of request reaches its designated limit per Application ID. |
| 500 | Occurs when internal errors happen. Try calling the api again. |
| 503 | Occurs when the API is not available due to maintenance and other reasons. |

Some error responses may contain further details. Most will be returned in the following format.

```
{
  "Error" : {
    "Message" : "<error message>"
  }
}
```

## Contact

Please refer to the following.
https://global-marketing.yahoo-net.jp/contact
