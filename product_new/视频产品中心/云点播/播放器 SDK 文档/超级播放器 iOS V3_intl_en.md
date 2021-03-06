## Overview

The superplayer SDK for iOS is a player component used to play back videos in VOD. It can implement powerful playback functionality similar to Tencent Video with just a few lines of code.

* Basic features: landscape/portrait mode switch, definition selection, gestures, small window playback, etc.
* Advanced features: video buffering, software/hardware decoding switch, adjustable-speed playback, video thumbnails, DRM-encrypted playback, etc.

The superplayer SDK supports more formats and has better compatibility and functionality than system-default players. In addition, it features instant playback on splash screen and low latency.

## SDK Download

The VOD superplayer SDK for iOS can be downloaded [here](https://github.com/tencentyun/SuperPlayer_iOS).

## Quick Integration

### CocoaPods integration

Add the following code to your Podfile:
```
pod 'SuperPlayer'
```

Enter `pod install` or `pod update` on the command line to perform the installation.

### Preparing videos

Log in to the [VOD Console](https://console.cloud.tencent.com/vod/overview), click **Media Assets** on the left sidebar, and you will see the uploaded video and its corresponding ID (i.e., `FileId`) in the video list in the **Uploaded** column. If you don't have a video, please click **Upload Video** to upload one.
![](https://main.qcloudimg.com/raw/5aa5675fb0b702b447e422328f54cb72.png)

You can initiate an [adaptive bitrate streaming](https://intl.cloud.tencent.com/document/product/266/33942) task for the uploaded video through [ProcessMedia](https://intl.cloud.tencent.com/document/product/266/34125):
You are recommended to enter 10 for `MediaProcessTask.AdaptiveDynamicStreamingTaskSet.Definition` in the API parameter, indicating transcoding to adaptive bitstream in HLS format.

### Starting playback

The main class of the player is `SuperPlayerView`, and videos can be played back after it is created.
```objective-c
// Import the header file
#import <SuperPlayer/SuperPlayer.h>
_playerView = [[SuperPlayerView alloc] init];
// Set the delegate to accept events
_playerView.delegate = self;
// Set the parent View; `_playerView` will be automatically added under `holderView`
_playerView.fatherView = self.holderView;
SuperPlayerModel *playerModel = [[SuperPlayerModel alloc] init];
SuperPlayerVideoId *video = [[SuperPlayerVideoId alloc] init];
// Set the playback information
video.appId = 1256993030;  //AppId
video.fileId = @"7447398157015849771";  // Video `FileId`
video.playDefinition = @"10";   // Playback template ID
video.version = FileIdV3;
playerModel.videoId = video;
// Start playback
[_playerView playWithModel:self.playerModel];
```

In the code, `appId` is your AppId, `fileId` is the ID of the video you want to play back, `playDefinition` is the ID of the playback template used for playback, and `version` is fixed to `SuperPlayerVideoId.FILE_ID_V3`.

Run the code and you can see that the video is played back on the phone and most of the features in the UI are available.


## Thumbnails and Timestamps

When videos are played back, the "thumbnails" and "timestamps" on the progress bar can help viewers find the points of interest easily. Thumbnails are implemented through [image sprites](https://intl.cloud.tencent.com/document/product/266/34125), while timestamps by modifying timestamp information in media assets.

After image sprites are generated and timestamps are added, new elements will be displayed in the player UI.


## Small Window Playback

A small window is a player that floats over the main window within the application. Small window playback is very simple. You just need to call the following code in the appropriate position:

```objective-c
[SuperPlayerWindow sharedInstance].superPlayer = _playerView; // Set the player for small window playback
[SuperPlayerWindow sharedInstance].backController = self;  // Set the returned view controller
[[SuperPlayerWindow sharedInstance] show]; // Floating display
```


## Exiting Playback

If the player is no longer needed, please call `resetPlayer` to clear the internal state of the player and free up the memory.
```objective-c
[_playerView resetPlayer];
```

## More Features

To try out the complete features, scan the QR code below to download the Tencent Video Cloud toolkit or run the project demo directly.
<img src="https://main.qcloudimg.com/raw/b670e99ddb3f0d828798520e19f40fa7.png" width="150">
