# StrInStrUm 아키텍처 (Rust + Python)

## ✅ 개요

StrInStrUm은 자연어 기반 도메인 특화 언어 (NL-DSL)를 사용하는 자동화 시스템으로, Rust 기반 DSL 파서와 Python 실행 엔진을 결합하여 사용자에게 직관적이고 강력한 자동화 기능을 제공합니다. 사용자에게는 목록형 DSL로 직관적 명령어를 입력하도록 하며, 내부적으로는 JSON으로 처리하여 실행합니다.

### ✅ 핵심 구성 요소

1. **DSL 파서 (Rust)**

   * Rust로 작성된 고성능 DSL 파서.
   * 사용자 입력 자연어를 **목록형 DSL**로 변환 (JSON은 내부 처리용).
   * 안전성, 속도, 메모리 최적화 제공.
   * 패턴 기반 문법 지원, 자연어를 목록형 명령으로 변환.
   * LLM (대형 언어 모델) 플러그인 지원 (plugin-strin)을 통해 동적 자연어 변환.

### ✅ Rust 기반 DSL 파서의 구조 및 코드 설명

* Rust DSL 파서는 다음 주요 구성 요소로 나뉩니다.

#### 📌 1. Tokenizer (토큰 분리기)

* 사용자 입력 자연어 명령을 공백이나 구두점 기준으로 토큰화.
* 예: "파일 다운로드 report.csv" → \["파일", "다운로드", "report.csv"]

#### 📌 2. Parser (파서)

* 토큰을 목록형 DSL 명령어로 파싱.
* 패턴 매칭을 통해 명령어와 매개변수를 인식.
* 예: "파일 다운로드 report.csv" →

```plaintext
1. 파일 다운로드:
   - 파일명: "report.csv"
```

#### 📌 3. JSON 변환기

* 파싱된 목록형 DSL을 내부 JSON 형식으로 변환 (StrUm에서 처리하기 위해)
* JSON은 사용자가 직접 접근할 필요 없음.
* 예:

```json
{
  "steps": [
    { "action": "file_download", "filename": "report.csv" }
  ]
}
```

### ✅ Python 실행 엔진 (StrUm)

* Rust 파서로부터 전달된 JSON DSL을 받아 실행.
* 각 명령어를 처리할 수 있는 실행 모듈을 로드하여 실행.

#### 📌 Python 실행 예제 코드

```python
import json

class StrInStrUmEngine:
    def __init__(self, dsl_json):
        self.dsl = json.loads(dsl_json)

    def execute(self):
        for step in self.dsl['steps']:
            if step['action'] == 'file_download':
                print(f"다운로드 실행: {step['filename']}")

# 예제 실행
dsl_json = '{"steps": [{"action": "file_download", "filename": "report.csv"}]}'
engine = StrInStrUmEngine(dsl_json)
engine.execute()
```

### ✅ LLM 플러그인 (plugin-strin)

* 사용자 명령어를 LLM을 통해 동적으로 파싱 가능.
* 예: "이메일 보내기 report.csv" → LLM 변환 → "파일 다운로드 report.csv"

### ✅ 보안 및 접속 제어

* API 키 기반 인증: strinstrum.deb.kr에서 사용자 인증 및 권한 제어.
* 각 사용자에게 발급된 API 키로 접속 및 사용 제어.
* JWT 또는 HMAC 방식으로 API 요청 검증.
