# StrInStrUm 아키텍처 (Rust + Python)

## ✅ 개요

StrInStrUm은 자연어 기반 도메인 특화 언어 (NL-DSL)를 사용하는 자동화 시스템으로, 고성능 Rust 기반 DSL 파서와 Python 실행 엔진을 사용합니다.

### ✅ 핵심 구성 요소

1. **DSL 파서 (Rust)**

   * Rust로 작성된 고성능 DSL 파서.
   * 자연어 DSL을 트리 구조 JSON으로 변환.
   * 안전성, 속도, 메모리 최적화 제공.
   * 패턴 기반 문법 지원, 자연어를 트리 구조 명령으로 변환.
   * LLM (대형 언어 모델) 플러그인 지원 (plugin-strin)을 통해 동적 자연어 변환.

2. **실행 엔진 (Python)**

   * 파싱된 DSL 명령을 실행.
   * 웹 자동화, 파일 처리, API 요청 등 다양한 자동화 작업 처리.

### ✅ LLM 통합 (plugin-strin)

* 각 LLM (GPT, Bard, Claude 등)은 플러그인 (plugin-strin)을 통해 확장 가능.
* 플러그인은 자연어 명령을 DSL 트리 구조로 변환하는 역할을 담당.
* 다국어 지원 (i18n)을 통해 언어별 문법 매핑 지원.
* 접속 제어: 사용자 인증, 토큰 할당, strinstrum.deb.kr에서 키 기반 권한 확인.

### ✅ LLM 플러그인 설정

* 플러그인은 표준 API 엔드포인트로 제공되며, HTTP/HTTPS로 접근 가능.
* LLM (OpenAI, Google Bard 등)에서 직접 이 엔드포인트를 호출 가능.
* 설정 옵션:

  * API URL: `https://strinstrum.deb.kr/plugin-strin`
  * 인증: API 키 헤더 (`Authorization: Bearer YOUR_TOKEN`)
  * 언어 파라미터: 입력 언어 지정 (기본값: ko)
  * 출력 형식: JSON (트리 구조 DSL)
* 사용 예제:

```http
POST /plugin-strin HTTP/1.1
Host: strinstrum.deb.kr
Authorization: Bearer YOUR_TOKEN
Content-Type: application/json

{
  "language": "ko",
  "command": "사용자에게 이메일을 보내고 보고서 첨부"
}
```

* 응답 예제:

```json
{
  "status": "success",
  "dsl": {
    "type": "email",
    "action": "send",
    "to": "user@example.com",
    "attachment": "report"
  }
}
```

### ✅ 다국어 지원 (i18n)

* 자연어를 DSL로 매핑할 때 다국어 지원 (한국어, 영어 등).
* 언어별 문법 규칙은 i18n 디렉토리에서 관리되며, 언어별 단어 매핑 포함.
* 각 언어 모델은 자연어를 DSL 명령어로 매핑할 수 있도록 훈련.

### ✅ 보안 및 접속 제어

* 토큰 할당 및 사용자 권한 제어는 strinstrum.deb.kr에서 관리.
* LLM 기반 DSL 변환은 인증된 키로만 접근 가능.

### ✅ 아키텍처 다이어그램 (추가 예정)

* 자연어 입력 → DSL 파싱 → Python 실행 엔진으로 연결되는 흐름을 시각화.
