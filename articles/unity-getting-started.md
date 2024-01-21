---
title: "AWS S3に保存した動画をUnityで再生するメモ"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "s3", "aws", "csharp", "vr"]
published: true
---

## はじめに

AWS S3 に保存した動画を、Unity で再生する手順をメモします。

## 完成イメージ

https://twitter.com/mitsuoka0423/status/1525853893401247744?s=20&t=Qmi9CXcFaBecG0HQj4c8EQ?conversation=none

## Unityプロジェクトを作成する

Unity Hub を利用して、新規プロジェクトを作成する。

- `新しいプロジェクト > 3D`を選択
- 任意の`エディターバージョン`を選択（今回は`2020.3.26f1`を利用）
- `プロジェクト名`を入力
- `保存場所`を選択
- `プロジェクトを作成`をクリック

[![Image from Gyazo](https://i.gyazo.com/7f01c2d32a14eff925b399f0899b10b6.png)](https://gyazo.com/7f01c2d32a14eff925b399f0899b10b6)

作成が終わったら、プロジェクトを開きます。

## 動画を表示する用のPlaneオブジェクトを追加する

`GameObject > 3D Object > Plane`を選択する。

> Plane：平面、水平面、(結晶体の)面

[![Image from Gyazo](https://i.gyazo.com/d26cfa4f2096983c72462cb8fd42d798.png)](https://gyazo.com/d26cfa4f2096983c72462cb8fd42d798)

追加された`Plane`は横に寝ているため、立てる。

[![Image from Gyazo](https://i.gyazo.com/204c4fedaad0fd121eb4f8330be2cf0b.png)](https://gyazo.com/204c4fedaad0fd121eb4f8330be2cf0b)

[![Image from Gyazo](https://i.gyazo.com/0ad4d3b4789164f0f639fb5336ffb34c.png)](https://gyazo.com/0ad4d3b4789164f0f639fb5336ffb34c)

## PlaneオブジェクトにVideo Playerを追加する

[![Image from Gyazo](https://i.gyazo.com/585d4b12dd58b69c9d9d82245cbbb964.png)](https://gyazo.com/585d4b12dd58b69c9d9d82245cbbb964)

Hierarchy に Video Player が追加される。

[![Image from Gyazo](https://i.gyazo.com/ed01ac16459c1302185602d0711393c3.png)](https://gyazo.com/ed01ac16459c1302185602d0711393c3)

## C#スクリプトを追加する

`Assets > 右クリック > create > C# Script`

[![Image from Gyazo](https://i.gyazo.com/56cdd6e28a4b46e5aa26cc5606e4168b.png)](https://gyazo.com/56cdd6e28a4b46e5aa26cc5606e4168b)

コードの中身はこちら。
`video.url`の値は、再生する動画の URL に置き換える。

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Video;

public class VideoDownloader : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        VideoPlayer video = gameObject.GetComponent<VideoPlayer>();
        video.playOnAwake = false;
        video.waitForFirstFrame = true;
        video.source = VideoSource.Url;
        video.url = "http://unity-video-s3-trial-20220509.s3-website-ap-northeast-1.amazonaws.com/video.mp4";

        video.prepareCompleted += VideoPlayerOnPrepareCompleted;
        video.Prepare();
    }

    private void VideoPlayerOnPrepareCompleted(VideoPlayer source)
    {
        source.Play();
    }
}
```

## 動作確認

動いた !

https://twitter.com/mitsuoka0423/status/1525853893401247744?s=20&t=Qmi9CXcFaBecG0HQj4c8EQ?conversation=none