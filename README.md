# 🗂️ 장지혁 포트폴리오

KH정보교육원 팀 프로젝트 2개 + 개인 프로젝트 저장소입니다.

📄 나의 포트폴리오 PPT: [Canva 링크](https://canva.link/keghmh1725gz40x)

---

## 📌 목차

| # | 프로젝트 | 유형 | 기간 |
|---|---------|------|------|
| 1 | [♻️ C2C - 중고거래 및 커뮤니티 사이트](#1-c2c---중고거래-및-커뮤니티-사이트) | KH 첫 번째 팀 프로젝트 | 2026.03.24 ~ 2026.04.22 |
| 2 | [🍔 ESG - 햄버거 메뉴 비교 서비스](#2-esg---햄버거-메뉴-비교-서비스) | KH 두 번째 팀 프로젝트 | 2026.05.11 ~ 2026.06.09 |
| 3 | [🥗 Meal Plan - 식단 기록 사이트](#3-meal-plan---식단-기록-사이트) | 개인 프로젝트 | 2026.06.11 ~ 2026.06.21 |

---

<br>

## 1. ♻️ C2C - 중고거래 및 커뮤니티 사이트

기여도: 10%

📁 GitHub: https://github.com/onlyworkjay/react_semi  

📄 발표자료: [C2C 세미 프로젝트.pdf](./C2C_세미_프로젝트.pdf)

📊 ERD CLOUD: [https://www.erdcloud.com/d/DbbW24AQSumQDHv3B](https://www.erdcloud.com/d/83X9JCE2vJupJspf5)


> 중고 거래를 중심으로 커뮤니티 기능을 결합한 통합 서비스로, 이용자들이 중고 거래를 통해 자원 재사용을 실천하고 탄소 배출 감소에 기여할 수 있도록 돕는 프로젝트입니다.

---

### 🛠 기술 스택

| 분류 | 기술 |
|------|------|
| **Frontend** | React 19, JSX, CSS Modules, Vite |
| **UI 라이브러리** | MUI (Material UI), Emotion |
| **에디터** | Tiptap (리치 텍스트 에디터) |
| **상태 관리** | Zustand |
| **HTTP 통신** | Axios |
| **실시간 통신** | WebSocket, STOMP, SockJS |
| **인증** | JWT (jwt-decode) |
| **알림** | SweetAlert2 (SWAL) |
| **차트** | Recharts |
| **주소 검색** | 카카오 우편번호 API |
| **Backend** | Spring Boot 4.0, Spring Security, Spring WebSocket |
| **ORM** | MyBatis |
| **Database** | Oracle |
| **인증** | JWT (jjwt) |
| **이메일** | Spring Mail |
| **기타** | Lombok, Jsoup |
| **버전 관리** | Git |

---

### 👨‍💻 담당 기능

- 커뮤니티 게시글 작성
- 커뮤니티 게시글 수정
- 커뮤니티 상세보기
- 커뮤니티 좋아요 / 싫어요 / 신고

---

### 📌 주요 기능 및 기능 상세

1. **회원가입 및 로그인** — 이메일 인증, 아이디·닉네임·이메일 중복 확인, JWT 기반 로그인 / 자동 로그아웃
2. **중고거래 게시판** — 게시글 등록·수정·삭제, 사진 업로드, 거래 요청·확정, 1:1 채팅 연동
3. **커뮤니티 게시판** — Tiptap 리치 에디터 기반 글 작성, 좋아요·싫어요·신고, 댓글·대댓글 CRUD
4. **1:1 채팅** — WebSocket + STOMP 기반 실시간 채팅, 거래 완료 시 채팅방 자동 삭제
5. **마이페이지** — 내 게시글·댓글 관리, 거래 현황 차트 (Recharts), 색상 테마 변경
6. **회원 등급** — 슈퍼 관리자 / 관리자 / 일반 유저 구분
7. **관리자 페이지** — 회원 관리, 게시글·댓글 관리, 거래 현황

---

### 💡 핵심 코드

#### 커뮤니티 신고 (CommunityController.java)

게시글 신고 등록·조회·취소를 REST API로 구현했습니다. 신고 시 신고 사유(reportReason)를 함께 전달하며, 본인이 이미 신고한 글인지 여부도 함께 반환해 프론트에서 신고/취소 버튼을 동적으로 제어할 수 있도록 했습니다.

```java
// 신고 정보 조회 (로그인 여부와 관계없이 조회 가능)
@GetMapping(value = "/{communityNo}/reports")
public ResponseEntity<?> selectReportInfo(
        @PathVariable Integer communityNo,
        @RequestHeader(required = false, name = "Authorization") String token) {
    Map<String, Object> reportInfo = communityService.selectReportInfo(communityNo, token);
    return ResponseEntity.ok(reportInfo);
}

// 신고 등록
@PostMapping(value = "/{communityNo}/reports")
public ResponseEntity<?> reportOn(
        @PathVariable Integer communityNo,
        @RequestHeader(name = "Authorization") String token,
        @RequestBody Map<String, String> body) {
    String reportReason = body.get("reportReason");
    int result = communityService.insertReport(communityNo, token, reportReason);
    return ResponseEntity.ok(result);
}

// 신고 취소
@DeleteMapping(value = "/{communityNo}/reports")
public ResponseEntity<?> deleteReport(
        @PathVariable Integer communityNo,
        @RequestHeader(name = "Authorization") String token) {
    int result = communityService.deleteReport(communityNo, token);
    return ResponseEntity.ok(result);
}
```

---

### 📋 기능 목록

| 페이지 | 세부 기능 | 기능 설명 | 로그인 여부 |
|--------|-----------|-----------|:-----------:|
| **회원** | 회원가입 | 이메일 인증 후 아이디·비밀번호·닉네임 입력하여 가입 | X |
| | 로그인 | 아이디·비밀번호 입력 후 JWT 토큰 발급 / 토큰 만료 시 자동 로그아웃 | X |
| | 아이디 찾기 | 이메일·닉네임 입력 시 아이디 조회 | X |
| | 비밀번호 찾기 | 이메일·아이디 일치 확인 후 임시 비밀번호 이메일 전송 | X |
| | 이메일 인증 | 6자리 랜덤 인증코드 이메일 발송 | X |
| **마이페이지** | 내 정보 수정 | 닉네임·비밀번호 변경, 프로필 썸네일 변경 | O |
| | 회원 탈퇴 | 회원 정보 및 프로필 이미지 삭제 | O |
| | 내 커뮤니티 게시글 | 내가 작성한 커뮤니티 글 목록 조회·숨김·삭제 | O |
| | 내 거래 게시글 | 내가 작성한 마켓 글 목록 조회·숨김·삭제 | O |
| | 내 댓글 관리 | 커뮤니티·마켓 댓글 목록 조회·수정·삭제 | O |
| | 좋아요·싫어요·신고 게시글 | 좋아요/싫어요/신고한 게시글 목록 조회 | O |
| | 거래 현황 차트 | Recharts로 거래 현황 시각화 | O |
| | 나의 인기글·최신글 | 내 인기글 및 최신글 조회 | O |
| | 색상 테마 변경 | 프로필 색상 테마 선택 및 변경 | O |
| **커뮤니티** | 게시글 목록 조회 | 필터·검색·페이지네이션 | X |
| | 게시글 등록 | Tiptap 에디터로 이미지 포함 글 작성 | O |
| | 게시글 상세 조회 | 게시글 단건 조회 (조회수 포함) | X |
| | 게시글 수정 | 본인 게시글 수정 | O |
| | 게시글 삭제 | 본인 게시글 삭제 | O |
| | 좋아요·싫어요 | 게시글 좋아요·싫어요 등록·취소 | O |
| | 게시글 신고 | 게시글 신고 등록·취소 | O |
| | 댓글·대댓글 CRUD | 댓글 및 대댓글 작성·수정·삭제 | O |
| | 댓글 좋아요·싫어요 | 댓글 좋아요·싫어요 등록·취소 | O |
| | 댓글 신고 | 댓글 신고 등록 | O |
| **중고거래 (마켓)** | 게시글 목록 조회 | 필터·검색·페이지네이션 | X |
| | 게시글 등록 | 이미지 포함 거래 글 작성 | O |
| | 게시글 상세 조회 | 게시글 단건 조회 (조회수 포함) | X |
| | 게시글 수정 | 본인 게시글 수정 (이미지 추가·삭제) | O |
| | 게시글 삭제 | 본인 게시글 삭제 (이미지 함께 삭제) | O |
| | 좋아요 | 게시글 좋아요 등록·취소 | O |
| | 게시글 신고 | 게시글 신고 등록·취소 | O |
| | 거래 요청 | 거래 요청 등록·조회·취소 | O |
| | 거래 확정 | 거래 확정 처리 | O |
| | 댓글·대댓글 CRUD | 댓글 및 대댓글 작성·수정·삭제 | O |
| | 댓글 신고 | 댓글 신고 등록 | O |
| | 탄소 기여도 | 회원별 탄소 기여도 목록 조회 | X |
| **채팅** | 실시간 채팅 | WebSocket + STOMP 기반 1:1 실시간 채팅 | O |
| | 채팅방 생성·조회 | 마켓 게시글 기준 채팅방 개설 또는 기존 방 반환 | O |
| | 채팅 내역 조회 | 이전 채팅 메시지 목록 조회 | O |
| | 메시지 읽음 처리 | 채팅 메시지 읽음 상태 처리 | O |
| | 내 채팅방 목록 | 참여 중인 채팅방 목록 조회 | O |
| | 거래 완료 시 채팅방 삭제 | 거래 확정 시 해당 채팅방 자동 삭제 | O |
| **지도** | 지도 조회 | 카카오 지도 API 기반 지역 검색 | X |
| **메인 페이지** | 커뮤니티 인기글·최신글 | 메인용 커뮤니티 게시글 목록 조회 | X |
| | 마켓 최신 목록 | 메인용 마켓 게시글 목록 조회 | X |

---

### 📡 API 명세

#### 👤 회원 (Member)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| POST | /members | 회원가입 | X |
| GET | /members/exists?memberId= | 아이디 중복 확인 | X |
| POST | /members/email-verification | 이메일 인증번호 발송 | X |
| POST | /members/login | 로그인 (JWT 발급) | X |
| POST | /members/find-id | 아이디 찾기 | X |
| POST | /members/find-pw | 비밀번호 찾기 (임시 비밀번호 이메일 전송) | X |
| GET | /members/{memberId} | 회원 정보 조회 | X |
| GET | /members | 전체 회원 목록 조회 | X |
| POST | /members/pw-auth | 비밀번호 인증 | O |
| PATCH | /members/{memberId}/thumbnail/update | 프로필 썸네일 변경 | O |
| PATCH | /members/{memberId} | 회원 정보 수정 | O |
| PATCH | /members/update-pw | 비밀번호 변경 | O |
| DELETE | /members/{memberId} | 회원 탈퇴 | O |

#### 🏠 마이페이지 (Mypage)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /mypages/board/community | 내 커뮤니티 게시글 목록 조회 | O |
| PATCH | /mypages/board/community/{boardNo} | 커뮤니티 게시글 숨김 처리 | O |
| DELETE | /mypages/board/community/{boardNo} | 커뮤니티 게시글 삭제 | O |
| GET | /mypages/board/market | 내 마켓 게시글 목록 조회 | O |
| PATCH | /mypages/board/market/{boardNo} | 마켓 게시글 숨김 처리 | O |
| DELETE | /mypages/board/market/{boardNo} | 마켓 게시글 삭제 | O |
| GET | /mypages/comment/market | 내 마켓 댓글 목록 조회 | O |
| GET | /mypages/comment/community | 내 커뮤니티 댓글 목록 조회 | O |
| PATCH | /mypages/comment/{commentNo} | 댓글 수정 | O |
| DELETE | /mypages/comment/{commentNo} | 댓글 삭제 | O |
| GET | /mypages/board/likedislike | 좋아요·싫어요·신고 게시글 목록 조회 | O |
| GET | /mypages/tradestatus/chart | 거래 현황 차트 데이터 조회 | O |
| GET | /mypages/tradestatus/list | 거래 현황 목록 조회 | O |
| GET | /mypages/today/{memberId} | 오늘의 활동 통계 조회 | O |
| GET | /mypages/my-best/{memberId} | 내 인기글 조회 | O |
| GET | /mypages/my-recent/{memberId} | 내 최신글 조회 | O |
| GET | /mypages/color | 색상 테마 목록 조회 | O |
| PATCH | /mypages/color | 색상 테마 변경 | O |

#### 📝 커뮤니티 (Community)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /communities | 게시글 목록 조회 | X |
| GET | /communities/main?type= | 메인 페이지용 게시글 목록 조회 | X |
| POST | /communities | 게시글 등록 | O |
| GET | /communities/{communityNo} | 게시글 상세 조회 | X |
| PUT | /communities/{communityNo} | 게시글 수정 | O |
| DELETE | /communities/{communityNo} | 게시글 삭제 | O |
| PATCH | /communities/view/{communityNo} | 조회수 증가 | X |
| GET | /communities/{communityNo}/likes | 좋아요 정보 조회 | X |
| POST | /communities/{communityNo}/likes | 좋아요 등록 | O |
| DELETE | /communities/{communityNo}/likes | 좋아요 취소 | O |
| GET | /communities/{communityNo}/dislikes | 싫어요 정보 조회 | X |
| POST | /communities/{communityNo}/dislikes | 싫어요 등록 | O |
| DELETE | /communities/{communityNo}/dislikes | 싫어요 취소 | O |
| GET | /communities/{communityNo}/reports | 신고 정보 조회 | X |
| POST | /communities/{communityNo}/reports | 신고 등록 | O |
| DELETE | /communities/{communityNo}/reports | 신고 취소 | O |
| GET | /communities/{communityNo}/comments | 댓글 목록 조회 | X |
| POST | /communities/comments | 댓글·대댓글 등록 | O |
| PATCH | /communities/comments | 댓글 수정 | O |
| DELETE | /communities/comments/{communityCommentNo} | 댓글 삭제 | O |
| GET | /communities/comments/{communityCommentNo}/likes | 댓글 좋아요 정보 조회 | X |
| POST | /communities/comments/{communityCommentNo}/likes | 댓글 좋아요 등록 | O |
| DELETE | /communities/comments/{communityCommentNo}/likes | 댓글 좋아요 취소 | O |
| POST | /communities/comments/{communityCommentNo}/dislikes | 댓글 싫어요 등록 | O |
| DELETE | /communities/comments/{communityCommentNo}/dislikes | 댓글 싫어요 취소 | O |
| POST | /communities/comments/reports | 댓글 신고 | O |

#### 🛒 중고거래 (Market)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /markets | 게시글 목록 조회 | X |
| GET | /markets/main | 메인 페이지용 게시글 목록 조회 | X |
| POST | /markets | 게시글 등록 (이미지 포함) | O |
| GET | /markets/{marketNo} | 게시글 상세 조회 | X |
| PUT | /markets/{marketNo} | 게시글 수정 | O |
| DELETE | /markets/{marketNo} | 게시글 삭제 (이미지 함께 삭제) | O |
| POST | /markets/{marketNo}/likes | 좋아요 등록 | O |
| DELETE | /markets/{marketNo}/likes | 좋아요 취소 | O |
| POST | /markets/{marketNo}/reports | 신고 등록 | O |
| DELETE | /markets/{marketNo}/reports | 신고 취소 | O |
| POST | /markets/{marketNo}/request | 거래 요청 등록 | O |
| GET | /markets/{marketNo}/requests | 거래 요청 목록 조회 | O |
| PATCH | /markets/{marketNo}/requests/{buyerId} | 거래 확정 | O |
| DELETE | /markets/{marketNo}/request | 거래 요청 취소 | O |
| GET | /markets/{marketNo}/comments | 댓글 목록 조회 | X |
| POST | /markets/comments | 댓글·대댓글 등록 | O |
| PATCH | /markets/comments | 댓글 수정 | O |
| DELETE | /markets/comments/{commentNo} | 댓글 삭제 | O |
| POST | /markets/comments/reports | 댓글 신고 | O |
| GET | /markets/carbon-contribution/{memberId} | 탄소 기여도 조회 | X |

#### 💬 채팅 (Chat)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /chat/history/{roomId} | 이전 채팅 메시지 조회 | O |
| POST | /chat/room/{roomId}/read | 메시지 읽음 처리 | O |
| GET | /chat/my/rooms | 내 채팅방 목록 조회 | O |
| POST | /chat/room/private/create | 1:1 채팅방 생성 또는 기존 방 반환 | O |
| DELETE | /chat/room/private/{marketNo} | 거래 완료 시 채팅방 삭제 | O |
| STOMP | /publish/{roomId} | 실시간 메시지 전송 | O |
| STOMP | /topic/{roomId} | 실시간 메시지 수신 (구독) | O |

---

<br>

## 2. 🍔 ESG - 햄버거 메뉴 비교 서비스

기여도: 20% (팀원 5명 배분해서 각각 20% 가짐)

📁 GitHub: https://github.com/onlyworkjay/ESG-MAIN  

📄 발표자료: [ESG 파이널프로젝트.pdf](./ESG_파이널프로젝트.pdf)

📊 ERD CLOUD: https://www.erdcloud.com/d/DbbW24AQSumQDHv3B

> 사용자가 햄버거 메뉴를 검색하고 가격·영양정보·후기를 비교하여 더 쉽게, 더 좋은 메뉴를 선택할 수 있도록 돕는 서비스입니다. 비교 후 선택한 메뉴를 기록하고, 그 기록을 바탕으로 후기를 작성해 다른 사용자와 경험을 공유할 수 있습니다.

---

### 🛠 기술 스택

| 분류 | 기술 |
|------|------|
| **Frontend** | React, JSX, CSS Modules, Vite |
| **상태 관리** | Zustand |
| **HTTP 통신** | Axios |
| **인증** | JWT (jwt-decode) |
| **알림** | SweetAlert2 (SWAL) |
| **Backend** | Spring Boot, MyBatis, Spring Security |
| **Database** | MySQL |
| **Cloud** | AWS S3 (이미지 저장) |
| **인증** | JWT (jjwt) |
| **이메일** | Spring Mail |
| **기타** | Lombok, Thumbnailator (이미지 압축) |
| **버전 관리** | Git |

---

### 👨‍💻 담당 기능

**메인페이지**
- 인기 메뉴 랭킹
- 사용자 랭킹

**메뉴 탐색 페이지**
- 메뉴 탐색 카테고리
- 메뉴 탐색 검색
- 메뉴 탐색 즐겨찾기
- 메뉴 제보하기
- 메뉴 상세 MODAL

**후기 (Gram)**
- 후기 목록
- 후기 작성
- 후기 상세보기
- 후기 수정 및 삭제
- 후기 댓글 목록
- 후기 댓글 수정 및 삭제
- 후기 좋아요
- 후기 신고

---

### 📌 주요 기능 및 기능 상세

1. **회원가입 및 로그인** — JWT 발급, 정지 계정 로그인 차단, 토큰 만료 5분 전 자동 재발급
2. **메뉴 탐색** — 브랜드·메뉴명 기반 검색, 즐겨찾기, 메뉴 상세 MODAL
3. **랜덤 추첨** — 전체 브랜드·메뉴 중 랜덤으로 하나 출력
4. **비교하기 (Choice)** — 비회원 2개, 회원 3개까지 메뉴 선택 가능 / 선택 후 choiceId 발급
5. **후기 작성** — choiceId(영수증 역할)가 있어야만 후기 작성 가능 / 이미지 최대 5장
6. **마이페이지** — 회원정보, 최근 선택 메뉴, 내 게시글·후기, 제보/신고 내역, 즐겨찾기
7. **관리자 페이지** — 회원 관리·정지, 메뉴 등록·수정, 신고 처리

---

### 💡 핵심 코드

#### choiceId 기반 후기 작성 플로우

후기를 아무나 바로 작성하지 못하도록 "비교하기 → 메뉴 선택 → choiceId 발급 → 후기 작성" 순서를 강제했습니다. choiceId는 일상에서 영수증이나 결제 내역과 같은 역할을 하며 이것이 없으면 후기를 쓸 수 없습니다.

**① Gram 페이지 - 작성 버튼 클릭 시 이동 페이지 선택**

```jsx
const handleWrite = async () => {
  if (!userId) {
    Swal.fire({ title: "로그인을 해야 사용할 수 있는 기능입니다." });
    return;
  }

  const { value: selected } = await Swal.fire({
    title: "후기 작성 전에 이동할 페이지를 선택하세요",
    html: `
      <div style="display:flex; gap:16px; justify-content:center;">
        <label><input type="radio" name="redirect" value="stat"/><span>📊 비교하기</span></label>
        <label><input type="radio" name="redirect" value="mypage"/><span>👤 마이페이지</span></label>
      </div>
    `,
    preConfirm: () => {
      const checked = document.querySelector('input[name="redirect"]:checked');
      if (!checked) { Swal.showValidationMessage("하나를 선택해주세요!"); return false; }
      return checked.value;
    },
    confirmButtonText: "이동하기",
    showCancelButton: true,
  });

  if (!selected) return;
  if (selected === "stat") navigate("/esg/stat");
  else navigate("/esg/mypage");
};
```

**② Stat 페이지 - 회원 기준 최대 3개 메뉴 선택 후 choiceId 발급**

```jsx
const MAX_COMPARE = 3; // 회원 기준 (비회원은 2개)

const handleAddMenu = (menu) => {
  if (isAlreadyAdded(menu.id)) { showWarning("이미 추가된 메뉴입니다."); return; }
  if (compareList.length >= MAX_COMPARE) { showWarning("최대 3개까지 비교할 수 있습니다."); return; }
  addCompareMenu(menu);
};

// 메뉴 선택 확정 → choices POST → choiceId 발급 → Choice 페이지로 이동
const handleConfirmSelectedMenu = async () => {
  const response = await axios.post(`${import.meta.env.VITE_BACKSERVER}/choices`, {
    userId,
    selectedProductId: selectedMenu.id,
    items: compareList.map((menu, index) => ({
      productId: menu.id,
      displayOrder: index + 1,
    })),
  });

  const { choiceId, choiceGroupId } = response.data;
  navigate(`/choice/${choiceId}`, {
    state: { choiceId, choiceGroupId, isMember: true, menus: compareList },
  });
};
```

**③ Gram 작성 페이지 - choiceId 없으면 작성 불가**

```jsx
// URL 파라미터 또는 store에서 choiceId 획득 (영수증 역할)
const choiceId = Number(paramChoiceId ?? storeChoiceId);

const handleSubmit = () => {
  // choiceId 없으면 작성 불가 (영수증 없는 리뷰 방지)
  if (!choiceId || Number.isNaN(choiceId)) {
    Swal.fire({
      title: "선택 기록을 찾을 수 없습니다.",
      text: "비교하기에서 메뉴를 다시 선택한 뒤 후기를 작성해주세요.",
    });
    return;
  }

  const form = new FormData();
  form.append("title", gram.title.trim());
  form.append("content", gram.content.trim());
  form.append("userId", String(loginUserId));
  form.append("choiceId", String(choiceId)); // 핵심: 선택 기록 연결
  files.forEach((file) => form.append("files", file));

  axios.post(`${import.meta.env.VITE_BACKSERVER}/grams`, form, {
    headers: { Authorization: token },
  }).then(() => {
    Swal.fire({ title: "등록되었습니다.", icon: "success" })
      .then(() => navigate("/esg/gram"));
  });
};
```

> **요약 흐름:** Gram 페이지 → Stat 페이지(최대 3개 선택) → Choice 페이지(choiceId 발급) → Gram 작성 페이지

---

### 📋 기능 목록

| 분류 | 기능 | 설명 | 로그인 여부 |
|------|------|------|:-----------:|
| **회원** | 회원가입 | 아이디·닉네임·비밀번호·이메일(선택) 입력 후 가입 / 중복 불가 | X |
| | 로그인 | JWT 토큰 발급 / 정지 계정 로그인 차단 | X |
| | 관리자 로그인 | 별도 관리자 전용 로그인 페이지 (/showmethemoney) | X |
| | 아이디·비밀번호 찾기 | 닉네임·이메일 일치 확인 후 이메일로 제공 | X |
| | 토큰 재발급 | 토큰 만료 5분 전 자동 재발급 (세션 연장) | O |
| | 회원 탈퇴 | 아이디 기준 회원 삭제 | O |
| **마이페이지** | 프로필 이미지 변경 | 이미지 업로드 및 기본 이미지 초기화 | O |
| | 닉네임·이메일 수정 | 닉네임 중복 검사 후 수정 | O |
| | 내 게시글 조회 | 본인이 작성한 게시글 목록 조회 | O |
| | 즐겨찾기한 회원 목록 | 즐겨찾기한 회원 목록 조회 | O |
| **메뉴 탐색 (Eat)** | 메뉴 목록 조회 | 브랜드별·조건별 필터링 및 페이지네이션 | X |
| | 메뉴 상세 조회 | 개별 메뉴 상세 정보 조회 | X |
| | 메뉴 제보하기 | 회원이 메뉴 정보 오류 제보 등록 | O |
| **메뉴 선택 (Choice)** | 메뉴 선택 저장 | 비교 후 최종 선택한 메뉴 저장 및 choiceId 발급 | O |
| | 일일 선택 횟수 확인 | 하루 선택 가능 횟수 확인 | O |
| | 최근 선택 조회 | 마이페이지에서 최근 선택 이력 조회 | O |
| **후기 (Gram)** | 후기 목록 조회 | 전체 후기 게시글 목록 (필터·페이지네이션) | X |
| | 후기 등록 | 이미지 포함 후기 작성 (choiceId 보유자만 가능) | O |
| | 후기 상세 조회 | 후기 단건 조회 (조회수 포함) | X |
| | 후기 수정·삭제 | 본인 후기 수정·삭제 | O |
| | 좋아요 토글 | 후기 좋아요 등록·취소 | O |
| | 후기 신고 | 후기 신고 등록 | O |
| | 댓글 CRUD | 후기별 댓글 작성·수정·삭제 | O |
| **게시판 (Post)** | 게시글 CRUD | 이미지 포함 게시글 작성·수정·삭제 | O |
| | 좋아요·신고 | 게시글 좋아요 토글, 신고 등록·취소 | O |
| **즐겨찾기 메뉴** | 즐겨찾기 등록·취소 | 메뉴 즐겨찾기 추가·삭제 | O |
| **랭킹·인기 메뉴** | 주간 사용자 랭킹 | 최근 7일 사용자 랭킹 조회 | X |
| | 주간 인기 메뉴 | 최근 7일 인기 메뉴 조회 | X |
| **관리자 (Admin)** | 회원 관리 | 회원 목록 조회·상세·정지 처리 | O |
| | 메뉴 관리 | 메뉴 등록·수정 (이미지 포함) | O |
| | 신고 처리 | 게시글·후기 숨김, 신고 반려·승인 | O |

---

### 📡 API 명세

#### 👤 회원 (User)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| POST | /users/join | 회원가입 | X |
| GET | /users/check-id?loginId= | 아이디 중복 확인 | X |
| POST | /users/login | 로그인 (JWT 발급) | X |
| POST | /users/adminlogin | 관리자 로그인 | X |
| POST | /users/find-id | 아이디 찾기 (이메일 전송) | X |
| POST | /users/find-pw | 비밀번호 찾기 (임시 비밀번호 이메일 전송) | X |
| POST | /users/refresh | 토큰 재발급 | O |
| POST | /users/profile-image | 프로필 이미지 변경 | O |
| PATCH | /users/profile-image/default | 기본 프로필 이미지로 초기화 | O |
| PATCH | /users/email | 이메일 수정 | O |
| PATCH | /users/nickname | 닉네임 수정 | O |
| GET | /users/my-posts | 내 게시글 목록 조회 | O |
| DELETE | /users/delete?loginId= | 회원 탈퇴 | O |
| GET | /users/ranking | 주간 사용자 랭킹 | X |
| GET | /users/popular-menus | 주간 인기 메뉴 | X |

#### 🍔 메뉴 탐색 (Eat)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /eats | 메뉴 목록 조회 (필터·페이지네이션) | X |
| GET | /eats/{productId} | 메뉴 상세 조회 | X |
| GET | /eats/brands | 브랜드 목록 조회 | X |
| POST | /eats/suggestions | 메뉴 제보 등록 | O |
| GET | /eats/suggestions?userId= | 내 제보 내역 조회 | O |

#### 🎯 메뉴 선택 (Choice)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /api/product-items | 전체 상품 목록 조회 | X |
| POST | /choices | 메뉴 선택 저장 (choiceId 발급) | O |
| GET | /choices/users/{userId}/daily-status | 일일 선택 횟수 확인 | O |
| GET | /choices/users/{userId}/latest | 최근 선택 이력 조회 | O |
| GET | /choices/{choiceId} | 선택 결과 조회 | X |

#### 📝 후기 (Gram)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /grams | 후기 목록 조회 | X |
| GET | /grams/my-grams | 내 후기 목록 조회 | O |
| POST | /grams | 후기 등록 (이미지 포함) | O |
| GET | /grams/{gramNo} | 후기 상세 조회 | X |
| PUT | /grams/{gramNo} | 후기 수정 | O |
| DELETE | /grams/{gramNo} | 후기 삭제 | O |
| POST | /grams/{gramId}/like | 좋아요 토글 | O |
| GET | /grams/{gramId}/like | 좋아요 상태 조회 | X |
| POST | /grams/{gramId}/report | 후기 신고 | O |
| GET | /grams/{gramId}/comments | 댓글 목록 조회 | X |
| POST | /grams/{gramId}/comments | 댓글 등록 | O |
| PUT | /grams/{gramId}/comments/{commentNo} | 댓글 수정 | O |
| DELETE | /grams/{gramId}/comments/{commentNo} | 댓글 삭제 | O |

#### 📰 게시판 (Post)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /posts | 게시글 목록 조회 | X |
| POST | /posts | 게시글 등록 (이미지 포함) | O |
| GET | /posts/{postId} | 게시글 상세 조회 | X |
| PUT | /posts/{postId} | 게시글 수정 | O |
| DELETE | /posts/{postId} | 게시글 삭제 | O |
| POST | /posts/{postId}/likes | 좋아요 등록 | O |
| DELETE | /posts/{postId}/likes | 좋아요 취소 | O |
| POST | /posts/{postId}/reports | 게시글 신고 | O |
| DELETE | /posts/{postId}/reports | 신고 취소 | O |

#### ⭐ 즐겨찾기 메뉴 (Favorite)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /favorites?userId= | 즐겨찾기 목록 조회 | O |
| GET | /favorites/check/{productId}?userId= | 즐겨찾기 여부 확인 | O |
| POST | /favorites | 즐겨찾기 등록 | O |
| DELETE | /favorites/{productId}?userId= | 즐겨찾기 취소 | O |

#### 🔧 관리자 (Admin)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| GET | /admin/selectUsers | 회원 목록 조회 (검색·필터·정렬) | O |
| PATCH | /admin/groundUser | 회원 정지 처리 | O |
| POST | /admin/insertMenue | 메뉴 등록 (이미지 포함) | O |
| PATCH | /admin/updateMenue | 메뉴 수정 | O |
| GET | /admin/getReportList?page=&status= | 신고 목록 조회 | O |
| PATCH | /admin/hidePost | 게시글 숨김 처리 | O |
| PATCH | /admin/hideGram | 후기 숨김 처리 | O |
| PATCH | /admin/denyReport | 신고 반려 처리 | O |
| GET | /admin/dashboard | 관리자 대시보드 조회 | O |

---

<br>

## 3. 🥗 Meal Plan - 식단 기록 사이트

http://mealplan-jay-portfolio.s3-website.ap-northeast-2.amazonaws.com/

📄 포트폴리오 PPT: [Canva 링크](https://canva.link/pdyvn5z6xrw7x76)

📁 GitHub: https://github.com/onlyworkjay/MealPlan-Private-Portfolio  

📄 발표자료: [MealPlan_Private_Portfolio.pdf](./MealPlan_Private_Portfolio.pdf)

📊 ERD CLOUD: https://www.erdcloud.com/d/jWiZEGb86F7yWnYAi

> 운동 및 식단 관리를 하는 사람들을 위해 만든 1인 개발 개인 포트폴리오 프로젝트입니다. 사진과 함께 피드를 작성해 식단을 기록하고, 통계 페이지에서 체중 변화 그래프와 사진 슬라이드로 변화 추이를 확인할 수 있습니다.

---

### 🛠 기술 스택

| 분류 | 기술 |
|------|------|
| **Frontend** | React 18, JSX, CSS, Vite |
| **HTTP 통신** | Axios |
| **차트** | Recharts |
| **알림** | SweetAlert2 (SWAL) |
| **인증** | JWT (Context API 기반 전역 상태 관리) |
| **Backend** | Spring Boot 3.5, Spring Security Crypto |
| **ORM** | JPA |
| **Database** | MySQL |
| **Cloud** | AWS S3 (이미지 저장) |
| **인증** | JWT (jjwt) |
| **이메일** | Spring Mail |
| **기타** | Lombok |
| **버전 관리** | Git |

---

### 📌 주요 기능 및 기능 상세

1. **회원가입 및 로그인** — 이메일 인증 (6자리, 유효시간 3분), 아이디·닉네임·이메일 중복 확인, JWT 기반 로그인 / 5분 전 세션 만료 경고 및 연장 / 자동 로그아웃
2. **피드 등록** — 사진 필수 (최대 4장, JPG·JPEG·PNG·WEBP, 장당 최대 10MB), 사진 없이는 칼로리·제목 입력 불가 (순서 강제), 하루 최대 3번 업로드 제한
3. **피드 조회** — 메인페이지는 당일 피드만, 피드 페이지는 전체 피드 출력 / 키워드 검색, 페이지네이션 (10개) / 사진 슬라이드 뷰어, 칼로리·작성일(시분 포함) 표시
4. **날짜별 조회 (캘린더)** — 나의 피드만 반영 / 기록 있는 날짜에 점(dot) 표시 / 미래 날짜 비활성화 / 연도→월 빠른 선택 가능
5. **통계 페이지** — 날짜별 체중 입력 (소수점 둘째 자리까지, 1~300kg 제한) / Recharts 꺾은선 그래프 (같은 날 여러 건 입력 시 마지막 값 기준) / 사진 슬라이드 뷰어 + MODAL 원본 크기 출력
6. **마이페이지** — 프로필 사진 변경, 닉네임·이메일·비밀번호 변경 → 헤더에 즉시 반영, 회원 탈퇴 (Hard Delete)

---

### 💡 핵심 코드

#### ① JWT 세션 관리 (AuthContext.jsx)

5분 남았을 때 자동으로 연장 여부를 묻고, 만료 시 자동 로그아웃 처리

```jsx
useEffect(() => {
  const tick = () => {
    const exp = getTokenExpiry(token);
    const remaining = Math.max(0, Math.round((exp - Date.now()) / 1000));
    setRemainingSeconds(remaining);

    if (remaining <= 0) { logout(); return; }

    if (remaining <= 300 && !warnedRef.current) {
      warnedRef.current = true;
      Swal.fire({
        title: "세션이 곧 종료됩니다",
        text: "5분 후 자동 로그아웃됩니다. 연장하시겠습니까?",
        showCancelButton: true,
        confirmButtonText: "연장하기",
      }).then((result) => { if (result.isConfirmed) extendSession(); });
    }
  };
  tick();
  const interval = setInterval(tick, 1000);
  return () => clearInterval(interval);
}, [token]);
```

#### ② 피드 등록 유효성 검사 (WritePage.jsx)

사진 첨부 → 칼로리 → 제목 순서를 강제하고, 허용되지 않는 파일 형식을 차단

```jsx
const ALLOWED_IMAGE_TYPES = ["image/jpeg", "image/jpg", "image/png", "image/webp"];

const handleImgChange = (idx, e) => {
  const file = e.target.files[0];
  if (!ALLOWED_IMAGE_TYPES.includes(file.type)) {
    showSwal({ type: "warning", title: "JPG·JPEG·PNG·WEBP 형식만 첨부할 수 있습니다." });
    e.target.value = "";
    return;
  }
};

const handleSubmit = (e) => {
  if (!hasImage) { showSwal({ title: "사진을 1장 이상 첨부해주세요." }); return; }
  if (!title.trim()) { showSwal({ title: "제목을 입력해주세요." }); return; }
  if (!calories) { showSwal({ title: "칼로리를 입력해주세요." }); return; }
};
```

#### ③ 체중 그래프 - 날짜별 마지막 값 기준 처리 (StatPage.jsx)

같은 날 여러 번 입력해도 마지막 값 하나만 그래프에 반영

```jsx
const chartData = useMemo(() => {
  const lastWeightByDate = new Map();
  // Map.set은 같은 키를 덮어쓰므로, 자연히 "마지막 입력값"이 남음
  stats.forEach((s) => { lastWeightByDate.set(s.date, s.weight); });

  return Array.from(lastWeightByDate.entries())
    .sort(([a], [b]) => new Date(a) - new Date(b))
    .map(([date, weight]) => {
      const [, m, d] = date.split("-");
      return { date: `${Number(m)}.${Number(d)}`, weight };
    });
}, [stats]);
```

#### ④ 캘린더 - 기록 있는 날짜 dot 표시 및 미래 날짜 비활성화 (CalendarPage.jsx)

내가 게시물을 올린 날짜에만 점을 찍고, 미래 날짜는 클릭 불가 처리

```jsx
// 내가 게시물을 올린 날짜 Set으로 관리
const myDates = new Set(myPosts.map((w) => toDateKey(w.createdAt)));

const isFuture = d !== null && new Date(year, month, d) > todayMidnight;

<div
  className={[
    styles.miniCalDay,
    dateKey && recordDates.has(dateKey) ? styles.hasRecord : "", // dot 표시
  ].filter(Boolean).join(" ")}
  style={isFuture ? { color: "#cbd5e1", cursor: "not-allowed", pointerEvents: "none" } : undefined}
  onClick={() => dateKey && !isFuture && onDateSelect(dateKey)}
>
  {d ?? ""}
</div>
```

---

### 📋 기능 목록

| 페이지 | 세부 기능 | 기능 설명 | 로그인 여부 |
|--------|-----------|-----------|:-----------:|
| **로그인 및 회원가입** | 회원가입 | 이메일 인증 후 아이디·닉네임·비밀번호 입력 / 중복 불가 | X |
| | 로그인 | JWT 토큰 발급 / 만료 시 자동 로그아웃 | X |
| | 아이디·비밀번호 찾기 | 닉네임+이메일 인증 후 아이디 조회 또는 비밀번호 재설정 | X |
| **메인 페이지** | 오늘의 피드 | 당일 피드 카드형 표시, 페이지네이션 10개 / 첫 번째 사진이 썸네일 | X |
| | 키워드 검색 | 제목·내용 검색 후 해당 피드만 필터링 | X |
| **피드 페이지** | 전체 피드 | 전체 피드 출력 (메인은 당일만) | X |
| | 기록하기 버튼 | 하루 최대 3번 업로드 가능 / 초과 시 SWAL 경고 | O |
| | 사진 업로드 | 최대 4장, JPG·JPEG·PNG·WEBP, 장당 최대 10MB / AWS S3 저장 | O |
| | 칼로리 입력 | 사진 첨부 후 칼로리 수기 입력 (정수만 허용) | O |
| | 제목/내용 입력 | 제목 필수 (최대 50자), 내용 선택 (최대 1,000자) | O |
| **피드 상세 페이지** | 상세보기 | 사진 슬라이드 뷰어, 칼로리·작성일(시분)·제목·내용 표시 | X |
| | 수정 | 본인만 수정 가능 / 사진 추가·삭제 포함 | O |
| | 삭제 | 본인만 삭제 가능 / S3 이미지 함께 제거 | O |
| **날짜별 조회** | 캘린더 UI | 나의 피드만 반영 / 기록 있는 날짜에 점(dot) 표시 / 미래 날짜 비활성화 | X |
| | 날짜 클릭 | 해당일 피드만 필터링 표시 | X |
| **마이 페이지** | 내 정보 관리 | 프로필 사진 변경, 닉네임·이메일·비밀번호 변경, 회원 탈퇴 (Hard Delete) | O |
| | 내 피드 탭 | 본인이 작성한 피드 출력 | O |
| **통계 페이지** | 체중 변화 그래프 | Recharts 꺾은선 그래프 / 같은 날 여러 건 입력 시 마지막 값 기준 | O |
| | 사진 슬라이드 뷰어 | 날짜별 사진 슬라이드 / 클릭 시 MODAL로 원본 크기 출력 | O |
| | 체중 입력 | 날짜별 체중(kg) 수기 입력 / 소수점 둘째 자리까지 | O |

---

### 📡 API 명세

#### 👤 회원 (User)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| POST | /users/join | 회원가입 | X |
| GET | /users/check-id?loginId= | 아이디 중복 확인 | X |
| GET | /users/check-nickname?nickname= | 닉네임 중복 확인 | X |
| GET | /users/check-email?email= | 이메일 중복 확인 | X |
| POST | /users/login | 로그인 (JWT 토큰 발급) | X |
| POST | /users/refresh | 토큰 재발급 (세션 연장) | O |
| POST | /users/find-id | 아이디 찾기 (닉네임 + 이메일 확인) | X |
| POST | /users/reset-password | 비밀번호 재설정 | X |
| POST | /users/profile | 내 정보 수정 (닉네임·이메일·프로필 사진) | O |
| POST | /users/password | 비밀번호 변경 | O |
| POST | /users/withdraw | 회원 탈퇴 (Hard Delete) | O |

#### 📧 이메일 인증 (Email)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| POST | /users/email-verification | 인증번호 전송 (유효시간 3분) | X |
| POST | /users/email-verification/confirm | 인증번호 확인 | X |

#### 🍽 피드 (Write)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| POST | /writes | 피드 등록 (사진 포함, S3 업로드) | O |
| GET | /writes | 전체 피드 목록 조회 | X |
| GET | /writes/my | 내가 작성한 피드 목록 조회 | O |
| GET | /writes/{writeId} | 피드 상세 조회 | X |
| POST | /writes/{writeId} | 피드 수정 (제목·내용·칼로리·사진) | O |
| DELETE | /writes/{writeId} | 피드 삭제 (S3 이미지 함께 삭제) | O |

#### 📊 통계 (Stat)

| Method | URL | 설명 | 인증 필요 |
|--------|-----|------|:---------:|
| POST | /stats | 체중 입력 | O |
| GET | /stats/my | 내 체중 변화 기록 전체 조회 (그래프용) | O |
