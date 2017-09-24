---
layout:     post
title:      "Toolbar 시작하기"
subtitle:   "Toolbar에 대해 다뤄봅니다"
date:       2017-09-23 00:00:00
author:     "Yonghoon"
header-img: "img/in-post/post-eleme-pwa/eleme-at-io.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Android
    - Toolbar
---

>아래의 나오는 코드들은 Support Library-v7과 Kotlin 언어를 사용했습니다.

## 개요

기존에는 Actionbar를 이용했지만 시간이 지날수록 더 많은 기능을 요구했다. 결국 제한적이던 Actionibar는 deprecated 되고 Toolbar가 등장했다. Toolbar는 위치 변경이 유연하며 애니메이션을 추가하는 등 커스텀하기 좋다.

Toolbar API 문서 : <https://developer.android.com/reference/android/support/v7/widget/Toolbar.html>{:target="_blank"}

<br>
## 시작

#### dependency 추가

다음의 코드가 없는 사람은 추가해야 한다. 최신 버전은 <a href="https://developer.android.com/topic/libraries/support-library/revisions.html" target="_blank">여기</a>에서 확인할 수 있다.
```
파일 : app/build.gradle

dependencies {
    compile 'com.android.support:appcompat-v7:25.4.0'
}
```

#### 테마 설정
Toolbar를 사용하려면 Actionbar를 사용하지 않는 테마를 써야한다. 다음의 코드를 추가하자.

```
파일 : app/src/main/res/values/styles.xml

<style name="AppTheme.NoTitle">
	<item name="windowActionBar">false</item>
</style>
```

만든 테마의 이름으로 적용시켜준다.
```
파일 : app/src/main/AndroidManifest.xml

<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_gachi_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme.NoTitle">
```

#### Toolbar 추가

layout에 Toolbar를 추가시켜준다.
```
파일 : app/src/main/res/layout/activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <android.support.v7.widget.Toolbar
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/toolbar"
        android:background="@color/colorPrimary"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        app:titleTextColor="#fff">
    </android.support.v7.widget.Toolbar>
</LinearLayout>
```
> **android:layout_height="?attr/actionBarSize"**<br>
> layout_height는 자유롭게 변경이 가능하다.<br>
> ?attr/actionBarSize 값을 바꾸려면 사용하는 Theme에 item을 추가한다.
> ```
><style name="AppTheme.NoTitle">
>	<item name="android:actionBarSize">49dp</item>
>	<item name="actionBarSize">49dp</item>
></style>
>```

Activity에서 toolbar를 actionbar로 설정해준다.
```
파일 : app/src/main/kotlin/com/test/MainActivity.kt

class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContentView(R.layout.activity_main)

		// actionbar
		setSupportActionBar(toolbar)
	}	
}
```

<br>
## 활용

#### 메뉴 추가
메뉴를 추가하려면 menu 파일을 만들어서 적용해야한다.
app/src/main/res/ 경로에 menu 폴더가 없다면 폴더를 만들고 아래의 menu.xml 파일을 만든다.
```
파일 : app/src/main/res/menu/menu.xml

<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:app="http://schemas.android.com/apk/res-auto">
	<item
		android:id="@+id/menu_add_person"
		android:title="@string/add_person"

		app:showAsAction="always"

		<!-- if use icon -->
		android:icon="@drawable/ic_add_person" />
</menu>
```

> **app:showAsAction**
> - always : 항상 표시
> - never : 더보기 버튼 안에 표시
> - ifRoom : actionbar에 공간이 있을 때 표시
> - withText : icon과 text 둘다 표시

생성한 menu 파일을 Activity에 적용한다.
```
파일 : app/src/main/kotlin/com/test/MainActivity.kt

class MainActivity : AppCompatActivity() {
	override fun onCreateOptionsMenu(menu: Menu?): Boolean {
		menuInflater.inflate(R.menu.menu, menu)
		return super.onCreateOptionsMenu(menu)
	}
}
```

#### 메뉴 이벤트

activity에 등록된 menu들의 이벤트를 등록할 수 있다.
```
파일 : app/src/main/kotlin/com/test/MainActivity.kt

class MainActivity : AppCompatActivity() {
	override fun onOptionsItemSelected(item: MenuItem?): Boolean {
		when(item?.itemId) {
			R.id.menu_add_person -> {
				// do something
			}

			android.R.id.home -> {
				// do something
			}
		}

		return super.onOptionsItemSelected(item)
	}
}
```

> **android.R.id.home**<br>
> Toolbar 좌측에 있는 버튼을 가리킴 ([좌측 버튼 만드는 방법](#좌측에-버튼-텍스트-추가) 참조)


#### 좌측에 버튼, 텍스트 추가
actionbar의 기능을 사용하여 button, text를 추가할 수 있다.

**텍스트(Title) 추가**
<br>
Title이 보이도록 설정하고 텍스트를 입력하면 된다.
```
파일 : app/src/main/kotlin/com/test/MainActivity.kt

class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContentView(R.layout.activity_main)

		// actionbar
		setSupportActionBar(toolbar)
		supportActionBar?.setDisplayShowTitleEnabled(true)
		title = "타이틀"
	}	
}
```

**버튼 추가**
<br>
Navigation button 이나 뒤로가기 등의 버튼을 추가하는 방법이다.
사용할 아이콘을 등록하고 보이도록 설정한다.
```
파일 : app/src/main/kotlin/com/test/MainActivity.kt

class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContentView(R.layout.activity_main)

		// actionbar
		setSupportActionBar(toolbar)
		supportActionBar?.setHomeAsUpIndicator(R.drawable.ic_back)
		supportActionBar?.setDisplayHomeAsUpEnabled(true)
	}	
}
```
> icon을 vector 파일로 사용할 때는 코드에서 색을 바꿀 수 있다. ([아이콘 색 변경](#아이콘-색-변경) 참조)


#### Toolbar Custom
layout에서 Toolbar안에 LinearLayout, Buttom 등의 위젯을 넣어서 원하는 형태의 Toolbar를 사용할 수 있다.
```
<android.support.v7.widget.Toolbar
	android:id="@+id/toolbar"
	android:layout_width="match_parent"
	android:layout_height="?attr/actionBarSize">
	<TextView
		android:id="@+id/tv_toolbar"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:layout_gravity="center"
		android:clickable="false"
		android:focusable="false"
		android:longClickable="false" />
</android.support.v7.widget.Toolbar>
```


#### 아이콘 색 변경

좌측의 아이콘 색은 Tint로 변경할 수 있다.
```
val icon = AppCompatResources.getDrawable(this, R.drawable.ic_back)!!
DrawableCompat.setTint(icon, ContextCompat.getColor(this, R.color.colorWhite))

// actionbar
setSupportActionBar(toolbar)
supportActionBar?.run {
	setHomeAsUpIndicator(icon)
	setDisplayShowTitleEnabled(false)
	setDisplayHomeAsUpEnabled(true)
}
```

메뉴의 아이콘 색을 변경하려면 Theme에 아래와 같이 입력하면 된다.
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
	<item name="colorPrimary">@color/colorPrimary</item>
	<item name="colorPrimaryDark">@color/colorPrimaryDark</item>
	<item name="colorAccent">@color/colorAccent</item>

	<!-- Customize color of menu icon in toolbar. -->
	<item name="android:textColorSecondary">@color/colorWhite</item>
</style>
```