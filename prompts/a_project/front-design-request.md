# WR Frontend 프로젝트 설계 요청서

## 프로젝트 기본 정보

| 항목 | 내용 |
| :--- | :--- |
| **프로젝트명** | WR |
| **Frontend 저장소명** | `wrs-app-front` |
| **Backend 저장소명** | `wrs-app-backend` |
| **개발 환경** | IntelliJ IDEA + Claude Code |
| **기술 스택** | React, TypeScript, Vite, Capacitor, Node.js 22 LTS |
| **문서 상태** | 설계 요청 |
| **작업 단계** | 설계 단계 (구현 금지) |

---

## 1. 프로젝트 개요
프로젝트명은 **WR**이다.
Frontend는 React 기반 웹 애플리케이션으로 개발하며, Capacitor를 이용해 Android와 iOS 하이브리드 앱으로도 실행할 수 있도록 설계한다.
웹, Android, iOS에서 동일한 React 화면과 기능을 최대한 공유하는 것을 목표로 한다.
Backend는 별도 Git 저장소인 `wrs-app-backend`에서 관리한다.

### 1.1 Git 저장소 관리 원칙
* WR은 최상위 작업 폴더이며 Git 저장소로 만들지 않는다.
* `wrs-app-front`는 독립적인 Git 저장소로 관리한다.
* `wrs-app-backend`는 독립적인 Git 저장소로 관리한다.
* Frontend와 Backend는 Git 이력을 공유하지 않는다.
* Frontend 설계 문서와 프롬프트도 Frontend 저장소에서 관리한다.
* 설계 변경 사항은 Markdown 문서를 수정하고 Git으로 이력을 관리한다.

#### 예상 작업 폴더 구조
```text
WR/
├── wrs-app-front/
│   ├── docs/
│   └── prompts/
│
└── wrs-app-backend/
```

---

## 2. 기술 스택
다음 기술을 기준으로 설계한다.
* React
* TypeScript
* Vite
* Capacitor
* MUI
* React Big Schedule
* React Router
* Zustand
* Axios
* Node.js 22 LTS

각 기술의 역할을 명확하게 구분한다.

### 2.1 UI 구성
MUI를 기본 UI 컴포넌트 라이브러리로 사용한다.
* **주요 사용 영역:**
    * 버튼, 입력 필드, Select
    * Dialog, Drawer, Snackbar
    * AppBar, Navigation
    * 공통 레이아웃
    * 일반 데이터 목록 및 관리 화면
* AdminLTE는 사용하지 않는다.

### 2.2 핵심 스케줄러
일정 관리는 WR의 핵심 기능이므로 React Big Schedule을 우선 스케줄러로 채택한다.
* 공식 저장소: [https://github.com/ansulagrawal/react-big-schedule](https://github.com/ansulagrawal/react-big-schedule)
* 공식 문서: [https://react-big-schedule.ansulagrawal.com/](https://react-big-schedule.ansulagrawal.com/)
* 라이선스: MIT

#### 필수 화면 구조
* 상단 열: 사람 이름
* 왼쪽 행: 시간
* 각 사람의 시간대에 일정 표시
* 일간 일정표를 기본 화면으로 구성
* 필요에 따라 주간 및 기타 뷰 검토

#### 필수 기능
1. 일정 조회
2. 일정 생성
3. 일정 수정
4. 일정 삭제
5. 일정 드래그 이동
6. 다른 사람에게 일정 이동 및 담당자 변경
7. 일정 시작·종료 시간 변경 (Resize)
8. 일정 복사 및 붙여넣기
9. 일정 중복 및 충돌 검증
10. 일정 상세 정보 표시

#### 중요 설계 원칙
* React Big Schedule이 제공하는 기본 기능을 최대한 활용한다.
* 라이브러리의 내부 동작을 불필요하게 수정하지 않는다.
* 일정 복사·붙여넣기는 기본 제공 여부를 먼저 확인한다.
* 기본 제공하지 않는 기능은 라이브러리 확장 API를 활용한 별도 구현 범위로 분리한다.
* 일정 충돌 검증은 프론트엔드뿐 아니라 Backend에서도 최종 검증한다.
* 일정 변경 시 Backend 저장 결과를 기준으로 화면 상태를 확정한다.
* 라이브러리의 MIT 라이선스와 상용 이용 조건을 확인한다.

React Big Schedule은 최근 v9 기준으로 검토한다. 구버전 API와 혼용하지 않는다.

#### 설계 단계 필수 검증 항목
* Vertical Resource View의 실제 사용 방법
* MUI 테마와 스케줄러 CSS 간 충돌 가능성
* React Big Schedule의 일정 복사·붙여넣기 지원 여부
* 모바일 터치 드래그 및 스크롤 동작
* 일정이 많아졌을 때 렌더링 성능
* 라이브러리 업데이트 및 유지보수 위험

---

## 3. 프로젝트 실행 환경
개발 환경은 다음과 같다.
* macOS
* IntelliJ IDEA
* Claude Code
* Node.js 22 LTS
* Android Studio
* Xcode

웹 개발 및 빌드에는 Vite를 사용한다.
모바일 앱은 Capacitor로 Android 및 iOS 프로젝트를 관리한다.
Android와 iOS 네이티브 UI를 별도로 개발하지 않고 React 화면을 공통으로 사용한다.

---

## 4. 프로젝트 디렉터리 구조
다음 구조를 기본안으로 검토한다.

```text
wrs-app-front/
├── android/
├── ios/
├── prompts/
│   ├── a_project/
│   ├── b_architecture/
├── public/
├── src/
│   ├── api/
│   ├── assets/
│   ├── components/
│   │   ├── common/
│   │   └── layout/
│   ├── features/
│   │   ├── scheduler/
│   │   └── user/
│   ├── hooks/
│   ├── layouts/
│   ├── pages/
│   ├── router/
│   ├── stores/
│   ├── types/
│   ├── utils/
│   ├── App.tsx
│   └── main.tsx
├── capacitor.config.ts
├── package.json
└── vite.config.ts
```
기능 단위로 관리할 수 있도록 설계하되, 불필요하게 복잡한 폴더 구조는 만들지 않는다.

---

## 5. 플랫폼 지원
다음 세 가지 환경을 지원한다.
* Web
* Android
* iOS

공통 React 코드로 기능을 유지한다. 다음 사항을 설계에 반영한다.
* 반응형 레이아웃
* 모바일 Safe Area
* 가상 키보드로 인한 화면 가림
* 모바일 스크롤 및 터치 이벤트
* 브라우저와 앱의 뒤로 가기 동작
* 스케줄러의 모바일 터치 드래그
* 작은 화면에서 사람별 열을 표시하는 방식
* 모바일에서 일정 상세 정보 표시 방식

모바일에서 모든 사람의 열을 한 화면에 억지로 표시하지 않는다. 가로 스크롤 또는 담당자 필터링 등 적절한 UX를 검토한다.

---

## 6. Frontend와 Backend 연동
Backend는 별도 저장소 `wrs-app-backend`에서 개발한다.
Frontend는 Axios를 이용해 REST API를 호출한다.

다음 영역의 API 연동 구조를 설계한다.
* 사용자 정보
* 사람 및 담당자 목록
* 일정 목록 조회
* 일정 상세 조회
* 일정 생성
* 일정 수정
* 일정 삭제
* 일정 이동 및 시간 변경
* 일정 충돌 검증

일정 조회는 조회 기간을 기준으로 데이터를 요청하도록 설계한다. 불필요하게 전체 일정을 한 번에 내려받지 않는다.
API 요청 및 응답 DTO는 Backend와 협의할 수 있도록 별도의 API 명세로 정의한다.

---

## 7. 상태 관리
Zustand를 사용한다.
다음 상태를 구분한다.
* 로그인 사용자 및 사용자 정보
* 화면 전역 상태
* 일정 조회 조건
* 선택된 담당자
* 선택된 날짜 및 뷰
* 스케줄러 화면 상태

서버 데이터와 화면 상태를 혼합하지 않도록 설계한다.

---

## 8. 현재 개발 범위에서 제외할 사항
다음 기능은 현재 개발하지 않는다.
* 관리자 전용 Back-office 프로젝트
* 관리자 전용 웹 화면
* 카메라 및 사진 촬영
* 푸시 알림
* 생체 인증
* 별도 네이티브 UI
* 불필요한 네이티브 플러그인

관리자 기능은 추후 별도의 프로젝트로 개발할 예정이다. 현재는 초기 데이터 및 관리성 데이터가 필요한 경우 DB에 직접 입력하는 방식으로 운영한다. 따라서 현재 Frontend에 관리자 데이터 등록 화면을 만들지 않는다.

---

## 9. Git 관리
* `wrs-app-front`는 독립적인 Git 저장소다.
* Backend 저장소와 Git 이력을 공유하지 않는다.
* 최상위 작업 폴더 WR은 Git 저장소로 만들지 않는다.
* 설계 문서 및 프로젝트 관련 문서는 Frontend 저장소 내부에서 관리한다.
* 설계 변경 사항은 문서 수정 후 Git 커밋으로 이력을 남긴다.
* 프롬프트도 Git으로 관리하여 작업 지침과 변경 이력을 추적할 수 있도록 한다.

---

## 10. 개발 단계
다음 순서로 개발할 수 있도록 설계한다.
1. Frontend 기본 구조 및 실행 환경 설계
2. MUI 공통 레이아웃 설계
3. React Big Schedule 연동 및 Vertical Resource View 검증
4. 일정 생성·수정·이동·복사 UX 설계
5. 웹 화면 개발
6. Capacitor Android/iOS 연동
7. Backend API 연동
8. 웹·Android·iOS 통합 테스트

스케줄러는 핵심 기능이므로 일반 화면 개발보다 먼저 기술 검증을 진행한다.

---

## 11. Claude Code 작업 지침
현재 요청은 프로젝트 구현이 아닌 설계 단계다. 다음 사항을 반드시 지킨다.
* 코드 작성 금지
* 실제 프로젝트 파일 생성 금지
* 패키지 설치 금지
* 터미널 명령어 실행 금지
* Git 초기화 및 변경 금지
* Android/iOS 프로젝트 생성 금지
* 임의로 기술 스택 변경 금지

먼저 전체 Frontend 설계안을 작성한다.

### 설계안 필수 포함 항목
1. 기술 스택 및 선택 근거
2. 폴더 구조
3. 화면 및 라우팅 구조
4. MUI 공통 UI 구조
5. React Big Schedule 적용 구조
6. 일정 데이터 모델 및 프론트엔드 상태 관리
7. Backend API 연동 설계
8. 모바일 대응 전략
9. 라이브러리 라이선스 및 기술적 위험
10. 개발 로드맵

설계 결과는 Markdown 문서로 정리한다. 사용자가 설계안을 확인하고 승인하기 전까지 어떠한 구현 작업도 시작하지 않는다.

---

## 12. 설계 승인 및 구현 진행 원칙
개발은 다음 절차를 따른다.
1. Claude Code가 전체 설계안을 작성한다.
2. 사용자가 설계안을 검토한다.
3. 필요한 수정 사항을 반영한다.
4. 사용자가 최종 설계안을 승인한다.
5. 승인 후에만 실제 프로젝트 구현을 시작한다.

> **중요:** 사용자의 명시적인 승인 없이 코드 작성, 파일 생성, 패키지 설치 및 Git 변경을 진행하지 않는다.