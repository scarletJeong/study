# Android Manifest와 Splash Activity 로직 이해

## 소개

이 문서에서는 `manifest` 파일의 주요 구성 요소와 Splash Activity의 구현 방식을 정리함.

---

## 주요 학습 내용

### 1. Manifest 파일의 역할

#### 정의
- `AndroidManifest.xml` 파일은 안드로이드 애플리케이션의 구조를 정의하는 설정 파일임.
- 애플리케이션 컴포넌트(Activity, Service, BroadcastReceiver 등), 사용자 권한, 하드웨어 및 소프트웨어 요구 사항 등을 지정함.

#### 주요 구성 요소
1. **`<uses-permission>`**
   - 애플리케이션이 요청하는 사용자 권한을 선언함.
   - 예:
     ```xml
     <uses-permission android:name="android.permission.INTERNET" />
     <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
     ```
   - `INTERNET`: 인터넷 액세스 권한.
   - `ACCESS_FINE_LOCATION`: GPS를 통해 정밀한 위치를 가져오는 권한.

2. **`<application>`**
   - 애플리케이션의 기본 속성과 구성 요소를 정의함.
   - 예:
     ```xml
     <application
         android:icon="@mipmap/ic_launcher"
         android:label="@string/app_name"
         android:theme="@style/AppTheme">
         <activity android:name=".SplashActivity">
             <intent-filter>
                 <action android:name="android.intent.action.MAIN" />
                 <category android:name="android.intent.category.LAUNCHER" />
             </intent-filter>
         </activity>
         <activity android:name=".MainActivity" />
     </application>
     ```
   - `icon`: 애플리케이션 아이콘.
   - `label`: 애플리케이션 이름.
   - `theme`: 애플리케이션에서 사용할 기본 테마.
   - `<activity>`: 각 화면을 정의하며, `SplashActivity`가 앱의 시작점으로 설정됨.

#### 역할 요약
- 앱의 기본 권한과 설정을 관리하며, 컴포넌트 간의 관계와 실행 방식을 정의함.
- 런타임에 필요한 리소스와 동작을 제어함.

---

### 2. Splash Activity 로직

#### 정의
- 애플리케이션 실행 시 첫 화면으로 표시되는 Activity임.
- 로딩 화면 또는 초기화 작업을 위한 UI를 제공하며, 사용자가 메인 화면으로 이동하기 전의 작업을 수행함.

#### 주요 기능
1. **UI 표시**:
   - 단순한 로딩 화면이나 브랜드 로고를 사용자에게 표시함.

2. **초기화 작업 수행**:
   - 네트워크 연결 설정, 데이터베이스 초기화, 캐시 로딩 등 백그라운드 작업을 처리함.

3. **MainActivity로 전환**:
   - 초기화 작업이 완료되면 MainActivity로 이동함.

#### 구현 예시
```java
import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import androidx.appcompat.app.AppCompatActivity;

public class SplashActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_splash);

        // 초기화 작업 수행
        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                // MainActivity로 전환
                Intent intent = new Intent(SplashActivity.this, MainActivity.class);
                startActivity(intent);
                finish();
            }
        }, 3000); // 3초 후 실행
    }
}
```
- `Handler`를 사용하여 3초 동안 Splash 화면을 유지한 후 MainActivity로 전환함.
- `finish()`를 호출하여 SplashActivity를 종료하고 메모리 낭비를 방지함.

---

## 깨달은 것

1. **Manifest 파일의 역할**:
   - 앱의 권한, 컴포넌트, 초기 설정 등을 관리하며, 애플리케이션의 기본 구조를 정의함.

2. **Splash Activity의 역할**:
   - 초기 로딩 화면에서 필요한 작업을 수행하며, 사용자가 앱을 시작할 준비를 돕는 중요한 구성 요소임.
