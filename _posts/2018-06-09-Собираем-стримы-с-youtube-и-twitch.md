---
  layout: post
  title: Собираем стримы с youtube и twitch
  tags: api, wordpress, youtube, twitch
  categories: development
---

На фрилансе нашел заказ на сборку апи с youtube и twitch. Для выполнения буду использовать Api ресурсов.

## Twitch

Api твича предельно ясно. На странице [документации](https://github.com/justintv/Twitch-API/wiki/Streams-Resource) можно увидеть метод: ```GET /streams/:channel/```. Возвращает что то вроде этого json, если канал онлайн и идет стрим.

Для быстрой работы с Api я использовал composer package `nicklaw5/twitch-api-php`.

```
{
  "stream": {
    "game": "StarCraft II: Wings of Liberty",
    "name": "live_user_incontroltv",
    "created_at": "Tue Jul 24 13:22:26 2012",
    "viewers": 3505,
    "updated_at": "Tue Jul 24 15:33:39 2012",
    "channel_id": 20248706,
    "preview": "http://static-cdn.jtvnw.net/previews/live_user_incontroltv-630x473.jpg",
    "_links": {
      "self": "https://api.twitch.tv/kraken/streams/live_user_incontroltv"
      "channel": "https://api.twitch.tv/kraken/channels/live_user_incontroltv"
    },
    "_id": 138658440,
    "delay_length": 0,
    "broadcaster": "fme",
    "geo": "US",
    "channel": {
      "name": "incontroltv",
      "game": "StarCraft II: Wings of Liberty",
      "created_at": "2011-02-07T06:24:25Z",
      "teams": [
        {
          "name": "eg",
          "created_at": "2011-10-11T23:59:43Z",
          "background": "http://static-cdn.jtvnw.net/jtv_user_pictures/team-eg-background_image-089a407eb59fe3b2.png",
          "updated_at": "2012-01-15T19:43:40Z",
          "banner": "http://static-cdn.jtvnw.net/jtv_user_pictures/team-eg-banner_image-8089b058e6ffe4cd-640x125.png",
          "logo": "http://static-cdn.jtvnw.net/jtv_user_pictures/team-eg-team_logo_image-53eaf029dad7d5c9-300x300.png",
          "_links": {
            "self": "https://api.twitch.tv/kraken/teams/eg"
          },
          "_id": 2,
          "info": "Team Info\n",
          "display_name": "Evil Geniuses"
        }
      ],
      "banner": "http://static-cdn.jtvnw.net/jtv_user_pictures/incontroltv-channel_header_image-612847bc6f091b50-640x125.jpeg",
      "updated_at": "2012-07-24T20:22:26Z",
      "_id": 20248706,
      "_links": {
        "stream_key": "https://api.twitch.tv/kraken/channels/incontroltv/stream_key",
        "self": "https://api.twitch.tv/kraken/channels/incontroltv",
        "chat": "https://api.twitch.tv/kraken/chat/incontroltv",
        "commercial": "https://api.twitch.tv/kraken/channels/incontroltv/commercial",
        "features": "https://api.twitch.tv/kraken/channels/incontroltv/features"
      },
      "logo": "http://static-cdn.jtvnw.net/jtv_user_pictures/incontroltv-profile_image--300x300.",
      "mature": null,
      "display_name": "iNcontroLTV",
      "status": "EGiNcontroL: music, commentary and all the stuff that makes rainbows and children possible"
    },
    "status": "EGiNcontroL: music, commentary and all the stuff that makes rainbows and children possible"
  },
  "_links": {
    "self": "https://api.twitch.tv/kraken/streams/live_user_incontroltv"
  }
}
```

## Youtube

Здесь все так же просто. Запрос типа `https://www.googleapis.com/youtube/v3/search?part=snippet&channelId={YOUR_CHANNEL_ID}&eventType=live&type=video&key={YOUR_API_KEY}` решает все.

## Проблемы и их решения

### Twitch

1. Твич апи работает только с ID каналов, которые нужно предварительно получить методом ` $this->twitch_api->getLiveStreams($channel = "$twitch_channel_id")`;

### Youtube

1. Запрос для получения стримов с канала должен выглядеть примерно так: `GET https://www.googleapis.com/youtube/v3/search?part=snippet&channelId=UClIIopOeuwL8KEK0wnFcodw&eventType=live&maxResults=25&regionCode=US&type=video&key={YOUR_API_KEY}`
2. Нужно как то обойти авторизацию через OAuth. Она требуе обязательной аутенфикации клиента.
