# Weather

基于 [高德开放平台](https://lbs.amap.com/dev/id/newuser) 的 PHP 天气信息组件。

## 安装

```bash
$ composer require ydy7512/weather -vvv
```

## 配置

在使用本拓展之前，你需要去 [高德开放平台](https://lbs.amap.com/dev/id/newuser) 注册账号，然后创建应用，获取应用的 API Key。

## 使用

```php
use Ydy7512\Weather\Weather;

$key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx';

$weather = new Weather($key);
```

### 获取实时天气

```php
$response = $weather->getWeather('厦门');
```
示例：
```json
{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "lives": [
        {
            "province": "福建",
            "city": "厦门市",
            "adcode": "350200",
            "weather": "小雨",
            "temperature": "20",
            "winddirection": "北",
            "windpower": "7",
            "humidity": "60",
            "reporttime": "2018-11-01 11:00:00"
        }
    ]
}
```

### 获取近期天气预报

```php
$response = $weather->getWeather('厦门', 'all');
```
示例：
```json
{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "forecasts": [
        {
        "city": "厦门市",
        "adcode": "350200",
        "province": "福建",
        "reporttime": "2018-11-01 08:00:00",
        "casts": [
            {
                "date": "2018-11-01",
                "week": "4",
                "dayweather": "小雨",
                "nightweather": "中雨",
                "daytemp": "21",
                "nighttemp": "16",
                "daywind": "东北",
                "nightwind": "东北",
                "daypower": "6",
                "nightpower": "6"
            },
            {
                "date": "2018-11-02",
                "week": "5",
                "dayweather": "小雨",
                "nightweather": "小雨",
                "daytemp": "20",
                "nighttemp": "17",
                "daywind": "东北",
                "nightwind": "北",
                "daypower": "5",
                "nightpower": "4"
            },
            {
                "date": "2018-11-03",
                "week": "6",
                "dayweather": "阴",
                "nightweather": "多云",
                "daytemp": "24",
                "nighttemp": "20",
                "daywind": "北",
                "nightwind": "西北",
                "daypower": "≤3",
                "nightpower": "≤3"
            },
            {
                "date": "2018-11-04",
                "week": "7",
                "dayweather": "多云",
                "nightweather": "多云",
                "daytemp": "25",
                "nighttemp": "21",
                "daywind": "东北",
                "nightwind": "东北",
                "daypower": "4",
                "nightpower": "4"
            }
            ]
        }
    ]
}
```

### 获取 XML 格式返回值

第三个参数为返回值类型，可选 `json` 与 `xml` ，默认 `json`：
```php
$response = $weather->getWeather('厦门', 'base', 'xml');
```
示例：
```xml
<response>
    <status>1</status>
    <count>1</count>
    <info>OK</info>
    <infocode>10000</infocode>
    <lives type="list">
        <live>
            <province>福建</province>
            <city>厦门市</city>
            <adcode>350200</adcode>
            <weather>小雨</weather>
            <temperature>19</temperature>
            <winddirection>东北</winddirection>
            <windpower>7</windpower>
            <humidity>61</humidity>
            <reporttime>2018-11-01 11:00:00</reporttime>
        </live>
    </lives>
</response>
```

### 参数说明

```text
array | string getWeather(string $city, string $type = 'base', string $format = 'json')
```
* `$city` - 城市名，比如："厦门"；
* `$type` - 返回内容类型：`base`：返回实况天气 / `all`：返回预报天气；
* `$format` - 输出的数据格式，默认为 json 格式，当 output 设置为" `xml` "时，输出的为XML格式的数据。

### 在 Laravel 中使用

在 Laravel 中使用也是用样的安装方式，配置写在 `config/services.php` 中：
```php
.
.
.
'weather' => [
    'key' => env('WEATHER_API_KEY'),
]
```
然后在 `.env` 中配置 `WEATHER_API_KEY` ：
```text
WEATHER_API_KEY=xxxxxxxxxxxxxxxxxxxxx
```
可以用两种方式来获取 `Ydy7512\Weather\Weather` 实例：

#### 方法参数注入

```php
.
.
.
use Ydy7512\Weather\Weather;
.
.
.
public function edit(Weather $weather)
{
    $response = $weather->getWeather('厦门');
}
.
.
.
```

#### 服务名访问
```php
.
.
.
public function edit()
{
    $response = app('weather')->getWeather('厦门');
}
```

## 参考

* [高德开放平台天气接口](https://lbs.amap.com/api/webservice/guide/api/weatherinfo/)

## License

MIT