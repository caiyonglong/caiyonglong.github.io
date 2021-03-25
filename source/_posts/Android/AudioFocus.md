---
title: 音频焦点管理
categories: Android
tags:
  - 源码分析
  - AudioFocus
abbrlink: 68e283a1
date: 2018-04-24 10:04:02
---

#前言
随着Android版本的升级，以前用的一些api都提过时，项目中使用
AudioFocusRequest 顾名思义是一个音频焦点请求类。


一个封装音频焦点请求信息的类,AudioFocusRequest通过Builder实例化，有两个方法requestAudioFocus和abandonAudioFocusRequest。
    
#什么是焦点请求？

音频焦点是API 8中引入的一个概念。它主要作用于用户在同一个时刻只能关注单个音频流，例如，不能在同一时间听音乐或广播。在某些情况下，可以同时播放多个音频流，但只有一个用户会真正听到（关注），而其他的是背景音。例如在驾驶时正在说话，低音量播放音乐。（又称为低音）。
 
当应用程序请求音频焦点时，它表示它有意“拥有”音频焦点播放音频。我们来回顾一下不同类型的焦点请求，请求后的返回值，以及对损失的回应。
      
注意：在授予焦点之前，应用程序不应播放任何内容。

### 不同类型的焦点请求
-  AUDIOFOCUS_GAIN：用于指示音频焦点的增益或未知持续时间的音频焦点请求。
-  AUDIOFOCUS_GAIN_TRANSIENT：用于指示临时增益或音频焦点请求，预计持续时间短
-  AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK：用于指示音频焦点的临时请求，预计持续时间较短时间量，以及其他音频应用程序可以继续播放的位置降低产出水平后（也称为“回避”）。临时改变的例子是回放音乐播放的行车路线在后台是可以接受的。
      
- AUDIOFOCUS_GAIN_TRANSIENT_EXCLUSIVE：用于指示音频焦点的临时请求，预计持续时间较短，没有其他应用程序或系统组件应该播放的时间量、任何东西。 独占和瞬态音频焦点请求的例子是语音、备忘记录和语音识别，在此期间系统不应该播放任何内容通知，媒体播放应暂停。

- AUDIOFOCUS_LOSS：用于指示未知持续时间的音频焦点丢失。
- AUDIOFOCUS_LOSS_TRANSIENT：用于指示音频焦点的瞬时丢失。
- AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK：用于指示音频焦点的失败者可能发生的音频焦点的瞬时丢失，如果它想继续播放（也称为“躲避”），则降低其输出音量，新的焦点所有者不需要其他人保持沉默。

```
 /**
     * Used to indicate no audio focus has been gained or lost, or requested.
     */
    public static final int AUDIOFOCUS_NONE = 0;

    /**
     * Used to indicate a gain of audio focus, or a request of audio focus, of unknown duration.
     * @see OnAudioFocusChangeListener#onAudioFocusChange(int)
     * @see #requestAudioFocus(OnAudioFocusChangeListener, int, int)
     */
    public static final int AUDIOFOCUS_GAIN = 1;
    /**
     * Used to indicate a temporary gain or request of audio focus, anticipated to last a short
     * amount of time. Examples of temporary changes are the playback of driving directions, or an
     * event notification.
     * @see OnAudioFocusChangeListener#onAudioFocusChange(int)
     * @see #requestAudioFocus(OnAudioFocusChangeListener, int, int)
     */
    public static final int AUDIOFOCUS_GAIN_TRANSIENT = 2;
    /**
     * Used to indicate a temporary request of audio focus, anticipated to last a short
     * amount of time, and where it is acceptable for other audio applications to keep playing
     * after having lowered their output level (also referred to as "ducking").
     * Examples of temporary changes are the playback of driving directions where playback of music
     * in the background is acceptable.
     * @see OnAudioFocusChangeListener#onAudioFocusChange(int)
     * @see #requestAudioFocus(OnAudioFocusChangeListener, int, int)
     */
    public static final int AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK = 3;
    /**
     * Used to indicate a temporary request of audio focus, anticipated to last a short
     * amount of time, during which no other applications, or system components, should play
     * anything. Examples of exclusive and transient audio focus requests are voice
     * memo recording and speech recognition, during which the system shouldn't play any
     * notifications, and media playback should have paused.
     * @see #requestAudioFocus(OnAudioFocusChangeListener, int, int)
     */
    public static final int AUDIOFOCUS_GAIN_TRANSIENT_EXCLUSIVE = 4;
    /**
     * Used to indicate a loss of audio focus of unknown duration.
     * @see OnAudioFocusChangeListener#onAudioFocusChange(int)
     */
    public static final int AUDIOFOCUS_LOSS = -1 * AUDIOFOCUS_GAIN;
    /**
     * Used to indicate a transient loss of audio focus.
     * @see OnAudioFocusChangeListener#onAudioFocusChange(int)
     */
    public static final int AUDIOFOCUS_LOSS_TRANSIENT = -1 * AUDIOFOCUS_GAIN_TRANSIENT;
    /**
     * Used to indicate a transient loss of audio focus where the loser of the audio focus can
     * lower its output volume if it wants to continue playing (also referred to as "ducking"), as
     * the new focus owner doesn't require others to be silent.
     * @see OnAudioFocusChangeListener#onAudioFocusChange(int)
     */
    public static final int AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK =
            -1 * AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK;
```



### 例子

初始化audio focus request
```
      mAudioManager = (AudioManager) Context.getSystemService(Context.AUDIO_SERVICE);
      mPlaybackAttributes = new AudioAttributes.Builder()
              .setUsage(AudioAttributes.USAGE_MEDIA)
              .setContentType(AudioAttributes.CONTENT_TYPE_SPEECH)
              .build();
      mFocusRequest = new AudioFocusRequest.Builder(AudioManager.AUDIOFOCUS_GAIN)
              .setAudioAttributes(mPlaybackAttributes)
              .setAcceptsDelayedFocusGain(true)
              .setWillPauseWhenDucked(true)
              .setOnAudioFocusChangeListener(this, mMyHandler)
              .build();
      mMediaPlayer = new MediaPlayer();
      mMediaPlayer.setAudioAttributes(mPlaybackAttributes);
      final Object mFocusLock = new Object();
     
      boolean mPlaybackDelayed = false;

      // requesting audio focus
      int res = mAudioManager.requestAudioFocus(mFocusRequest);
      synchronized (mFocusLock) {
          if (res == AUDIOFOCUS_REQUEST_FAILED) {
              mPlaybackDelayed = false;
          } else if (res == AUDIOFOCUS_REQUEST_GRANTED) {
              mPlaybackDelayed = false;
              playbackNow();
          } else if (res == AUDIOFOCUS_REQUEST_DELAYED) {
             mPlaybackDelayed = true;
          }
      }
```
实现音频焦点监听，OnAudioFocusChangeListener
```
      &#64;Override
      public void onAudioFocusChange(int focusChange) {
          switch (focusChange) {
              case AudioManager.AUDIOFOCUS_GAIN:
                  if (mPlaybackDelayed || mResumeOnFocusGain) {
                      synchronized (mFocusLock) {
                          mPlaybackDelayed = false;
                          mResumeOnFocusGain = false;
                      }
                      playbackNow();
                  }
                  break;
              case AudioManager.AUDIOFOCUS_LOSS:
                  synchronized (mFocusLock) {
                      // 长久失去音频焦点，停止播放
                      mResumeOnFocusGain = false;
                      mPlaybackDelayed = false;
                  }
                  pausePlayback();
                  break;
              case AudioManager.AUDIOFOCUS_LOSS_TRANSIENT:
              case AudioManager.AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK:
                  // 我们以同样的方式处理所有短暂的损失，因为我们从不聆听有声书
                  synchronized (mFocusLock) {
                      // we should only resume if playback was interrupted
                      mResumeOnFocusGain = mMediaPlayer.isPlaying();
                      mPlaybackDelayed = false;
                  }
                  pausePlayback();
                  break;
          }
      }
```
