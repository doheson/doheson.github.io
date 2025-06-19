---
title: "Android15(API level 35) 이상 대비 Edge to Edge 패딩 처리"
date: 2025-06-17T12:05:00+09:00
draft: false
tags: ["Android", "Edge to Edge"]
categories: ["Android"]
---
Android 15(API 레벨 35) 이상을 타겟팅하는 앱은 기본적으로 전체 디스플레이 영역을 활용한다.\
이 과정에서 안드로이드의 네비게이션바와 앱의 컨텐츠가 겹치는 현상이 발생하여 이슈가 발생했다.\
기존에는 windowOptOutEdgeToEdgeEnforcement 값을 true로 설정하여 EdgeToEdge 현상을 비활성화 했지만,\
Android 16부터는 해당 기능을 지원하지 않아 다른 방법을 찾아보던 중, 패딩을 넣는 방식으로 적용했다.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    RNBootSplash.init(this, R.style.BootTheme) // ⬅️ initialize the splash screen
    super.onCreate(null) // super.onCreate(null) with react-native-screens

    // API35 이상부터 자동으로 적용되는 EdgeToEdge(네비게이션 바에 겹치는 현상 발생) 기능 방지
    // Edge-to-edge 활성화 (시스템 바 영역까지 콘텐츠 확장)
    WindowCompat.setDecorFitsSystemWindows(window, false)

    // rootView 에 패딩 적용
    val rootView = findViewById<View>(android.R.id.content)
    ViewCompat.setOnApplyWindowInsetsListener(rootView) { view, insets ->
        val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
        view.updatePadding(
            top = systemBars.top, // 상태 바 높이만큼 상단 패딩 추가
            bottom = systemBars.bottom // 네비게이션 바 높이만큼 하단 패딩 추가
        )
        insets
    }
}
```

<span style="font-size:20px; color:white;">출처</span>

https://developer.android.com/about/versions/16/behavior-changes-16?hl=ko#edge-to-edge
