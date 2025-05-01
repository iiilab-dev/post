# iiiLab视频图片解析接口文档

此接口为通用视频图片解析接口，支持解析1000+境内外网站视频、图片、音频

> API基于REST架构设计。API具有结构清晰的面向资源的URL，接收JSON格式的请求体，返回JSON格式的响应，并使用标准的HTTP响应状态码。

### 请求参数

**接口地址：** `https://service.iiilab.com/openapi/extract`

**请求方式：** `POST`

**Content-Type：** `application/json`

**请求头(Header)**

请求头|请求头说明|值举例
:---|:---|:---
x-client-id|客户ID|iiiLab分配给您的客户ID|996981887a27d721
x-client-secret|客户秘钥|iiiLab分配给您的客户秘钥|c4ca4238a0b923820dcc509a6f75849b


**请求参数(Body)**

参数|参数说明|是否可空|值举例
:---|:---|:---|:---
url|要解析的帖子页面地址|不可空|`https://weibo.com/detail/4830591038789274`

### 🟢成功返回数据 

> HTTP状态码为200

```
{
    "text": "碉堡了😳 8K HDR IMAX 杜比5.1环绕声",
    "medias": [
        {
            "media_type": "video",
            "resource_url": "https://example.com/xyz/c4ca4238a0b923820dcc.mp4",
            "preview_url": "https://example.com/xyz/frame/id/c4ca4238a0b923820dcc?w=540&xcdelogo=0"
        },
        {
            "media_type": "image",
            "resource_url": "https://example.com/v/c4ca4238a0b923820dcc.jpg"
        },
        {
            "media_type": "audio",
            "resource_url": "https://example.com/c4ca4238a0b923820dcc.m4a",
            "preview_url": "https://example.com/c4ca4238a0b923820dcc.jpg",
            "headers": {
              "Referer": "https://www.sample.net/",
              "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
            }
        }
    ]
}
```

<details>
<summary>💡 部分网站的视频会包含多个清晰度版本，比如YouTube、FB等 [点击展开查看] 👈</summary>
  
```
{
  "text": "碉堡了😳 8K HDR IMAX 杜比5.1环绕声",
  "medias": [
    {
      "media_type": "video",
      "resource_url": "https://example.com/video/abc123.mp4",
      "preview_url": "https://example.com/images/xyz789.webp",
      "formats": [
        {
          "quality": 2160,
          "video_url": "https://example.com/video/4k/def456.webm",
          "video_ext": "webm",
          "video_size": 427472553,
          "audio_url": "https://example.com/audio/hij789.m4a",
          "audio_ext": "m4a",
          "audio_size": 9278232,
          "separate": 1,
          "quality_note": "4K"
        },
        {
          "quality": 1440,
          "video_url": "https://example.com/video/2k/klm012.webm",
          "video_ext": "webm",
          "video_size": 170247698,
          "audio_url": "https://example.com/audio/nop345.m4a",
          "audio_ext": "m4a",
          "audio_size": 9278232,
          "separate": 1,
          "quality_note": "2K"
        },
        {
          "quality": 1080,
          "video_url": "https://example.com/video/1080p/qrs678.mp4",
          "video_ext": "mp4",
          "video_size": 42534942,
          "audio_url": "https://example.com/audio/tuv901.m4a",
          "audio_ext": "m4a",
          "audio_size": 9278232,
          "separate": 1,
          "quality_note": "1080P"
        },
        {
          "quality": 720,
          "video_url": "https://example.com/video/720p/wxy234.mp4",
          "video_ext": "mp4",
          "video_size": 15488136,
          "audio_url": "https://example.com/audio/zab567.m4a",
          "audio_ext": "m4a",
          "audio_size": 9278232,
          "separate": 1,
          "quality_note": "720P"
        },
        {
          "quality": 480,
          "video_url": "https://example.com/video/480p/cde890.mp4",
          "video_ext": "mp4",
          "video_size": 8985464,
          "audio_url": "https://example.com/audio/fgh123.m4a",
          "audio_ext": "m4a",
          "audio_size": 9278232,
          "separate": 1,
          "quality_note": "480P"
        },
        {
          "quality": 360,
          "video_url": "https://example.com/video/360p/ijk456.mp4",
          "video_ext": "mp4",
          "video_size": 11133410,
          "audio_url": null,
          "audio_ext": null,
          "audio_size": null,
          "separate": 0,
          "quality_note": "360P"
        },
        {
          "quality": 240,
          "video_url": "https://example.com/video/240p/lmn789.mp4",
          "video_ext": "mp4",
          "video_size": 2486863,
          "audio_url": "https://example.com/audio/opq012.m4a",
          "audio_ext": "m4a",
          "audio_size": 9278232,
          "separate": 1,
          "quality_note": "240P"
        },
        {
          "quality": 144,
          "video_url": "https://example.com/video/144p/rst345.mp4",
          "video_ext": "mp4",
          "video_size": 1234145,
          "audio_url": "https://example.com/audio/uvw678.m4a",
          "audio_ext": "m4a",
          "audio_size": 9278232,
          "separate": 1,
          "quality_note": "144P"
        }
      ]
    }
  ]
}
```
</details>

**返回字段说明**

| 字段 | 说明 | 是否一定有 |
|--------|------|------------|
| medias | 一个链接里可能包含1个或多个media | ✨一定有 |
| medias -> media_type | 可能是video、image、audio | ✨一定有 |
| medias -> resource_url | 视频地址(video)、图片地址(image)、音频地址(audio) | ✨一定有 |
| medias -> preview_url | 视频封面(video)、音频封面(audio) | 💭可能有 |
| medias -> formats | 视频多清晰度列表 | 💭可能有 |
| medias -> headers | 下载resource_url时需要添加的请求头信息 | 💭可能有 |


### 🔴失败返回示例

> HTTP状态码非200，比如400、422、401、402、500等

```
{
    "message": "链接格式错误"
}
```

### 响应HTTP状态码说明

HTTP状态码|说明|返回内容示例
:---|:---|:---
200|成功|参考上述成功返回数据示例
400|业务失败|解析失败，请检查帖子链接是否包含视频图片
422|参数错误|链接格式错误
401|鉴权失败|clientId和clientSecret不匹配
402|调用次数已用完|接口调用额度已用完，请及时充值
500|未知错误|该错误一般不会遇到，如果遇到，请联系iiiLab技术支持
