# vod-source-clean

给 **cms-player (Apple TV)** 用的影视点播源，全部实测过：接口返回非空、取最新一部片子、抓到真实 m3u8 播放列表 (#EXTM3U) 才算通过。

## 直接填进 App（按键盘手酸程度排序）

**最短，Apple TV 上敲这个：**

```
https://tinyurl.com/247pz3t5
```

```
https://clck.ru/3Wp64y
```

较短直链（不用短链服务时）：

```
https://cdn.jsdelivr.net/gh/daletyler1737/xlz/1.json
```

原始直链：

```
https://cdn.jsdelivr.net/gh/daletyler1737/vod-source-clean/main/cmsplayer.json
https://gh-proxy.com/raw.githubusercontent.com/daletyler1737/vod-source-clean/main/cmsplayer.json
```

（短链是指向上面的 jsdelivr 直链，源一失效短链跟着废；换源时记得重建短链）

## 文件

| 文件 | 内容 |
| --- | --- |
| `cmsplayer.json` | 12 个主用源，纯净（成人占比 ≤15%），按可用性排序 |
| `cmsplayer-no-adult.json` | **11 个完全无成人内容的源**（3 页共 60 条标题逐个查过，0 命中），推荐用这个 |
| `cmsplayer-adult.json` | 12 个成人源，单独一个文件，不想看就别填 |
| `adult.json` | 旧版成人源（TVBox 格式） |
| `final.m3u` / `select.m3u` / `region.m3u` | 直播源，给支持 M3U 的播放器（VLC/IPTV 客户端） |

## 格式（cms-player 铁律）

必须是**裸数组**，每项**只允许** `name` / `api` / `note` 三个键：

```json
[
 {"name": "量子云", "api": "https://cj.lziapi.com/api.php/provide/vod/", "note": "已验证可播"}
]
```

多塞 `type` / `key` / `searchable` 之类的键，tvOS 播放器会直接报"格式不正确"。

## 怎么验证的

- DNS 可解析 → `GET <api>?ac=videolist` 返回 200 且 `list` 非空
- 取 `vod_time` 最新、且非成人标题的一部 → 取 `vod_play_url` 末集地址 → 抓到 `#EXTM3U` 才算可播
- 同一域名只留最快的一条，避免同一后端重复

采集站换域名很勤，源失效是常态。失效了就重跑一遍探测，不要手动一个个试。
