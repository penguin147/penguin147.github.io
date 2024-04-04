---
title:  "[Android] Unity Local Notification 구현하기"
excerpt: "Unity Local Notification을 구현해 보자"

categories:
  - Unity
tags:
  - [Unity, Android]

date: 2020-04-04
last_modified_at: 2020-04-04
---

# 개발 환경
- Unity 2021.3.8f1
- Mobile Notification 2.0.2

# 환경 구성

## 패키지 다운로드
![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/5cd8ec9f-a5c5-4f64-baa7-198fda7b40f3)
Window -> Package Manager -> Unity Registry에서 Mobile Notifications를 다운로드 받습니다.

<br>

## 이미지 설정
![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/07cf48fe-0219-428a-bf22-f0838c064351)
Edit -> Project Settings -> Mobile Notifications에서 Local Notification에서 사용할 Large Image와  Small Image를 설정해 줍니다.

<br>

![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/48dab152-9b0c-42c7-be64-80e8203851c7)

이미지를 그냥 넣으면 빨강 느낌표가 뜹니다. 이미지 설정에서 Read/Write에 체크를 해줍니다.

<br>

![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/b0a16969-7377-4e5e-94bf-d0e517fcf8b4)
다음과 같이 Small Image와 Large Image를 등록해 줍니다.

<br>

# 구현
## 코드
```c#
using System;
using UnityEngine;
using Unity.Notifications.Android;

public class NotificationManager : MonoBehaviour
{

    private void Awake()
    {
        AndroidNotificationCenter.CancelAllNotifications();
        AlarmChannelRegist();
    }

    public void SendNotification(string title, string message, DateTime fireTime)
    {
        try
        {
            AndroidNotification notification = new AndroidNotification();
            notification.ShouldAutoCancel = true;
            notification.SmallIcon = "icon_0";
            notification.LargeIcon = "icon_1";
            notification.Title = title;
            notification.Text = message;
            notification.FireTime = fireTime;

            AndroidNotificationCenter.SendNotification(notification, "test");
        }catch(Exception e)
        {
            Debug.LogError(e.ToString());
        }
    }

    public void AlarmChannelRegist()
    {
        var test_channel = new AndroidNotificationChannel()
        {
            Id = "test",
            Name = "testChannel",
            Importance = Importance.Default,
            Description = "test notification",
        };

        AndroidNotificationCenter.RegisterNotificationChannel(test_channel);
    }
}

```

<br>

그리고 사용할 때는
```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ButtonEvent : MonoBehaviour
{
    public NotificationManager notificationManager;
    
    public void buttonClicked()
    {
        notificationManager.SendNotification("test", "test notification", DateTime.Now.AddSeconds(10));
    }
    
}
```
다음과 같이 호출해 사용합니다.

<br>

## 결과
![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/be8a7e2c-79b1-46fd-adc8-c115bf4328f5)

제 경우에는 버튼이 클릭되었을 경우, 10초 뒤에 알림이 나오도록 하였습니다.

<br>

![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/afa52b35-0be8-4beb-ad4e-eebfda4f58e4)

다음과 같이 알림이 나오는 것을 확인할 수 있습니다.

<br>

## Small icon 이미지로 설정
```c#
using System;
using UnityEngine;
using Unity.Notifications.Android;

public class NotificationManager : MonoBehaviour
{

    private void Awake()
    {
        AndroidNotificationCenter.CancelAllNotifications();
        AlarmChannelRegist();
    }

    public void SendNotification(string title, string message, DateTime fireTime)
    {
        try
        {
            AndroidNotification notification = new AndroidNotification();
            notification.ShouldAutoCancel = true;
            //notification.SmallIcon = "icon_0";
            notification.LargeIcon = "icon_1";
            notification.Title = title;
            notification.Text = message;
            notification.FireTime = fireTime;

            AndroidNotificationCenter.SendNotification(notification, "test");
        }catch(Exception e)
        {
            Debug.LogError(e.ToString());
        }
    }

    public void AlarmChannelRegist()
    {
        var test_channel = new AndroidNotificationChannel()
        {
            Id = "test",
            Name = "testChannel",
            Importance = Importance.Default,
            Description = "test notification",
        };

        AndroidNotificationCenter.RegisterNotificationChannel(test_channel);
    }
}

```
Small icon은 기본적으로 흑백의 이미지만 사용 가능합니다. 권장되는 사항은 아니지만 small icon을 지정해 주는 부분을 주석 처리하면
![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/f82fb9fb-57fe-47b2-a2d3-ea6f4ecf4633)
에서 설정해둔 Default Icon이 small icon으로 표시되는것을 확인했습니다.

<br>

![image](https://github.com/penguin147/penguin147.github.io/assets/50098025/87f5374a-8be0-417b-a1d5-c99f86edab48)

현재 개발중인 게임에서는 게임을 형상화한 아이콘 보다는 게임의 색이 표현되는 이미지가 나왔으면 좋을것 같아 이렇게 처리하였습니다.
