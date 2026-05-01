# morphe-setlog-patches

[Morphe](https://github.com/MorpheApp) 패처를 사용한 **Setlog** (`com.newchat.setlog`) v1.0 전용 패치 모음입니다.

## 패치 목록

| 패치 이름 | 기본값 | 설명 |
|---|---|---|
| 고스트 모드 (Ghost Mode) | ✅ | 메시지·컬렉션을 읽어도 읽음 표시가 상대방에게 전송되지 않습니다 |
| 컬렉션 스텔스 뷰 (Stealth View) | ✅ | 컬렉션을 열어도 조회 기록이 서버에 남지 않습니다 |
| 타이핑 숨김 (Hide Typing) | ✅ | 입력 중 표시가 상대방에게 전송되지 않습니다 |
| 추적 제거 (Remove Tracking) | ✅ | Firebase 분석 이벤트 전송을 차단합니다 |
| 라이센스 우회 (License Bypass) | ✅ | PairIP 라이센스 검사를 항상 통과시킵니다 |
| 스크린샷 허용 (Allow Screenshot) | ✅ | 앱 내 스크린샷 촬영이 가능해집니다 |
| 삭제된 메시지 유지 (Keep Deleted Messages) | ✅ | 상대방이 삭제한 메시지가 화면에서 사라지지 않습니다 |
| Morphe 설정 진입점 | ✅ | 앱 내 Morphe 설정 버튼을 주입하고 Guard 클래스를 초기화합니다 |

설정은 앱 실행 후 화면 우측 하단에 나타나는 **Morphe 버튼**을 통해 개별 토글로 제어할 수 있습니다.

## 빌드 방법

### 요구 사항

- JDK 21
- Android SDK (Build Tools 포함)
- morphe-patcher 1.5.0 (mavenLocal에 설치 필요)

### 패치 파일 빌드

```bash
./gradlew :patches:buildAndroid
```

빌드 결과물: `build/libs/patches.mpp`

### APK 패치 적용

```bash
java -jar morphe-cli-<version>-all.jar patch \
    -p build/libs/patches.mpp \
    -o morphe-setlog.apk \
    setlog_1.0_merged.apk
```

## 프로젝트 구조

```
morphe-setlog-patches/
├── patches/
│   └── src/main/kotlin/app/morphe/setlog/
│       ├── SetlogPatches.kt   # 패치 정의 (8개)
│       └── Fingerprints.kt    # 메서드 핑거프린트
└── extensions/setlog-extension/
    └── src/main/java/app/morphe/setlog/ext/
        ├── MorphePrefs.java         # SharedPreferences 래퍼
        ├── GhostModeGuard.java      # 고스트 모드 / 삭제 메시지 유지
        ├── TypingGuard.java         # 타이핑 숨김
        ├── AnalyticsGuard.java      # 추적 제거
        ├── ScreenshotGuard.java     # 스크린샷 허용
        ├── LicenseBypassGuard.java  # 라이센스 우회
        ├── MorpheSettingsButton.java # 플로팅 설정 버튼
        └── MorpheSettingsDialog.java # 설정 다이얼로그 UI
```

## 호환성

| 앱 | 패키지명 | 버전 |
|---|---|---|
| Setlog | `com.newchat.setlog` | 1.0 |
