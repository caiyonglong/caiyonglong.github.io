---
layout: post
tags: 
 - api
categories: 接口
title: 接入Google、FaceBook第三方登录
date: 2019-01-16 23:16:43
---

# 通过Firebase接入Google、FaceBook等第三方登录

## 准备
- 科学上网
- google 账号，facebook账号

## 接入指南
主要分为三部分。
- Google登录
- Facebook登录
- Firebase集成接入google,facebook登录

### Google登录
推荐只接入Google登录，不接入Firebase

[接入指南](https://developers.google.com/identity/sign-in/android/start?authuser=0)

主要步骤
1、新建应用，或者已有的选择应用
2、创建应用的凭借，即token

核心代码

```
// Configure sign-in to request the user's ID, email address, and basic
// profile. ID and basic profile are included in DEFAULT_SIGN_IN.
GoogleSignInOptions gso = new GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
        .requestEmail()
        .build();
```

```
// Build a GoogleSignInClient with the options specified by gso.
mGoogleSignInClient = GoogleSignIn.getClient(this, gso);

```

```
private void signIn() {
    Intent signInIntent = mGoogleSignInClient.getSignInIntent();
    startActivityForResult(signInIntent, RC_SIGN_IN);
}
```

### Facebook登录
推荐只接入Facebook登录，不接入Firebase

[Facebook应用管理](https://developers.facebook.com/apps/)

主要流程
- 1、首先进入facebook应用开发面板，新建应用，并添加Facebook 登录
- 2、跟着步骤一步步完成所有配置（注意点：生成秘钥序列）
- 3、然后一步步点击完成
- 4、测试OK



### Firebase集成接入google,facebook登录
如果项目中已经集成了Firebase，则推荐使用这种方式

[Firebase项目管理界面](https://console.firebase.google.com) 创建项目


- 1、进入项目设置 开发->Authentication->登录方法。启用登录提供方中的Google，facebook。
- 2、关联google登录，只需要更新项目设置，添加应用，然后根据要求添加sha1指纹，填写包名，下载google-service.json文件引入到项目中。
- 3、启动facebook登录，按facebook登录步骤，启动facebook登录，然后配置应用token。

