# SSStudio Apps Terms & Conditions

여러 SSStudio 앱이 하나의 웹 페이지를 공유하면서 앱 이름, 언어, 사용하는 제3자 서비스를 URL 매개변수로 설정할 수 있는 이용약관 페이지입니다.

## 페이지 주소

```text
https://mintaeh0.github.io/ssstudio-apps-terms-and-conditions/
```

모든 앱은 위 주소를 공통 진입점으로 사용합니다. 약관 내용은 `lang` 매개변수에 따라 영어 또는 한국어 파일에서 불러옵니다.

## 파일 구조

```text
.
├── index.html
├── README.md
└── terms/
    ├── en.html
    └── ko.html
```

- `index.html`: URL 매개변수를 읽고 해당 언어의 약관을 불러오는 공통 진입점
- `terms/en.html`: 영문 이용약관 본문
- `terms/ko.html`: 한국어 이용약관 본문
- 언어별 파일은 독립적인 전체 HTML 문서가 아니라 `index.html`에 삽입되는 HTML 조각입니다.

## URL 형식

```text
https://mintaeh0.github.io/ssstudio-apps-terms-and-conditions/
  ?app={앱 이름}
  &lang={ko|en}
  &gplay={true|false}
  &admob={true|false}
  &fanal={true|false}
  &fcrash={true|false}
  &rcat={true|false}
```

실제 URL에서는 줄바꿈과 공백 없이 연결합니다.

## 매개변수

| 매개변수 | 필수 | 값 | 동작 |
|---|---:|---|---|
| `app` | 아니요 | 앱 이름 | 약관 본문의 앱 이름과 브라우저 제목에 사용합니다. |
| `lang` | 아니요 | `ko`, `en` | `ko`는 한국어, `en`은 영어입니다. 생략하거나 지원하지 않는 값을 넣으면 영어가 표시됩니다. |
| `gplay` | 아니요 | `true` | Google Play 서비스 약관 링크를 표시합니다. |
| `admob` | 아니요 | `true` | AdMob 약관 링크를 표시합니다. |
| `fanal` | 아니요 | `true` | Google Analytics for Firebase 약관 링크를 표시합니다. |
| `fcrash` | 아니요 | `true` | Firebase Crashlytics 약관 링크를 표시합니다. |
| `rcat` | 아니요 | `true` | RevenueCat 약관 링크를 표시합니다. |

제3자 서비스 매개변수는 값이 정확히 소문자 `true`일 때만 표시됩니다. 생략하거나 `false` 또는 다른 값을 사용하면 숨겨집니다.

`app`을 생략하면 한국어에서는 `애플리케이션`, 영어에서는 `this`가 기본값으로 사용됩니다.

## 사용 예시

### ShoX 한국어

```text
https://mintaeh0.github.io/ssstudio-apps-terms-and-conditions/?app=ShoX&lang=ko&gplay=true&admob=true&fanal=true&fcrash=true
```

### ShoX 영어

```text
https://mintaeh0.github.io/ssstudio-apps-terms-and-conditions/?app=ShoX&lang=en&gplay=true&admob=true&fanal=true&fcrash=true
```

### Banbancha 한국어 + RevenueCat

```text
https://mintaeh0.github.io/ssstudio-apps-terms-and-conditions/?app=Banbancha&lang=ko&gplay=true&admob=true&fanal=true&fcrash=true&rcat=true
```

## Flutter에서 URL 만들기

문자열을 직접 연결하기보다 `Uri`를 사용하면 앱 이름의 공백이나 특수문자가 자동으로 인코딩됩니다.

```dart
final termsUri = Uri.https(
  'mintaeh0.github.io',
  '/ssstudio-apps-terms-and-conditions/',
  {
    'app': 'ShoX',
    'lang': 'ko',
    'gplay': 'true',
    'admob': 'true',
    'fanal': 'true',
    'fcrash': 'true',
    // RevenueCat을 사용하는 앱에서만 추가합니다.
    // 'rcat': 'true',
  },
);
```

기기 또는 앱의 현재 언어에 맞춰 `lang` 값을 정하려면 다음처럼 구성할 수 있습니다.

```dart
final languageCode =
    Localizations.localeOf(context).languageCode == 'ko' ? 'ko' : 'en';

final termsUri = Uri.https(
  'mintaeh0.github.io',
  '/ssstudio-apps-terms-and-conditions/',
  {
    'app': 'ShoX',
    'lang': languageCode,
    'gplay': 'true',
    'admob': 'true',
    'fanal': 'true',
    'fcrash': 'true',
  },
);
```

생성한 `termsUri`를 앱 내부 WebView 또는 외부 브라우저에서 열어 사용합니다.

## 약관 수정 방법

약관 내용을 변경할 때는 다음 두 파일을 함께 확인합니다.

```text
terms/en.html
terms/ko.html
```

한쪽 언어의 조항을 추가, 삭제 또는 변경했다면 다른 언어에도 동일한 의미로 반영해야 합니다. 시행일도 두 파일에서 함께 변경합니다.

앱 이름이 들어갈 위치에는 다음 요소를 사용합니다.

```html
<span class="app-name">애플리케이션</span>
```

`index.html`이 해당 요소의 텍스트를 `app` 매개변수 값으로 안전하게 교체합니다.

## 제3자 서비스 추가 방법

새로운 제3자 서비스를 조건부로 표시하려면 다음 작업이 모두 필요합니다.

1. `terms/en.html`과 `terms/ko.html`에 동일한 ID를 가진 항목을 추가합니다.

```html
<li id="service-name" class="conditional-content">
  <a href="https://example.com/terms" target="_blank" rel="noopener noreferrer">
    Service Name
  </a>
</li>
```

2. `index.html`의 서비스 배열에 같은 ID를 추가합니다.

```js
["gplay", "admob", "fanal", "fcrash", "rcat", "service-name"]
```

3. URL에 `service-name=true`를 추가합니다.

## 로컬에서 확인하기

약관 파일은 `fetch()`로 불러오므로 `index.html`을 파일 탐색기에서 직접 열면 브라우저 보안 정책으로 로딩이 실패할 수 있습니다. 저장소 루트에서 간단한 로컬 서버를 실행합니다.

```bash
python -m http.server 8000
```

그다음 브라우저에서 다음 주소를 엽니다.

```text
http://localhost:8000/?app=ShoX&lang=ko&gplay=true&admob=true
```

## GitHub Pages 배포

GitHub Pages가 `main` 브랜치의 저장소 루트를 배포하도록 설정되어 있으면 `main`에 변경사항을 반영한 뒤 공개 페이지도 갱신됩니다.

설정 위치:

```text
Repository Settings → Pages → Deploy from a branch → main / (root)
```

## 관리 시 주의사항

- 지원하지 않는 `lang` 값은 영어로 처리됩니다.
- 제3자 서비스 링크는 해당 앱이 실제로 사용하는 서비스만 `true`로 전달합니다.
- 약관의 내용이 앱의 실제 기능 및 운영 방식과 일치하는지 정기적으로 확인합니다.
- 현재 DSA 조항에는 EU 내 법적 대리인 지정과 정기 투명성 보고서 발행에 관한 문구가 포함되어 있습니다. 실제 이행 여부와 다르면 해당 문구를 수정하거나 제거해야 합니다.
- 이 저장소의 문서는 법률 자문을 대체하지 않습니다.
