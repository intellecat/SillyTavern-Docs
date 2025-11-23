---
order: 100
icon: person-fill
route: /usage/characters/
---

# 캐릭터

캐릭터는 대화에서 AI의 역할을 형성하기 위해 생성하고 관리할 수 있는 AI 정체성입니다. 각 캐릭터는 이름, 성격, 대화 기록을 가지고 있습니다. 원하는 만큼 많은 캐릭터를 만들고 언제든지 전환할 수 있습니다.

캐릭터는 단독 채팅에 사용하거나 그룹 채팅에 여러 캐릭터를 추가하여 서로 상호작용하도록 할 수 있습니다.

## 캐릭터 관리 패널

네비게이션 바에서 <i class="fa-solid fa-address-card"></i> **Characters** 패널을 열어 캐릭터 목록에 액세스합니다. 캐릭터나 그룹을 클릭하여 채팅하거나 편집하거나, <i class="fa-solid fa-user-plus"></i> **Create New Character**를 선택하여 새 캐릭터를 추가합니다.

### 패널 컨트롤

* <i class="fa-solid fa-lock"></i> **Pin Panel**: 상호작용하는 동안 패널 열어두기
* <i class="fa-solid fa-list-ul"></i> **Character List**: 캐릭터 목록 보기로 돌아가기
* **HotSwap Bar**: 즐겨찾는 캐릭터에 빠르게 액세스

### 캐릭터 목록

* <i class="fa-solid fa-user-plus"></i> **Create New Character**: 새 캐릭터 추가
* <i class="fa-solid fa-file-import"></i> **Import Character**: 파일에서 캐릭터 불러오기
* <i class="fa-solid fa-cloud-arrow-down"></i> **External Import**: URL에서 가져오기
* <i class="fa-solid fa-users-gear"></i> **Create Group**: 새 그룹 채팅 시작

#### 검색 및 정렬

* **Search Bar**: 이름이나 속성으로 캐릭터 필터링
* **Sort Dropdown**: 여러 정렬 옵션:
    - 알파벳순 (A-Z, Z-A)
    - 시간순 (최신순, 오래된순)
    - 사용 기반 (최근, 채팅 횟수 최다/최소)
    - 크기 기반 (토큰 최다/최소)
    - 특수 (즐겨찾기, 무작위)

#### 유형 또는 태그별로 캐릭터 필터링

* <i class="fa-solid fa-star"></i> **Favorites Filter**: 즐겨찾는 캐릭터 표시
* <i class="fa-solid fa-users"></i> **Groups Filter**: 그룹 채팅만 표시
* <i class="fa-solid fa-folder-plus"></i> **Tags as Folders**: 태그 계층으로 정리
* <i class="fa-solid fa-gear"></i> **Manage Tags**: [태그 설정](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Tag List**: 사용 가능한 모든 태그 보기
* <i class="fa-solid fa-filter-circle-xmark"></i> **Clear Filters**: 모든 필터 재설정

### 캐릭터 생성/편집 패널

* **Avatar Image**: 캐릭터 프로필 사진 업로드 및 미리보기
* **Token Count**: 캐릭터의 [토큰 사용량](characterdesign.md#character-tokens)
* <i class="fa-solid fa-ranking-star"></i> **Stats**: 채팅 기록 및 사용 통계
* [태그 관리](/Usage/Characters/Tags.md)

#### 빠른 작업

- <i class="fa-solid fa-star"></i> 즐겨찾기 토글
- <i class="fa-solid fa-book"></i> 고급 정의
- <i class="fa-solid fa-globe"></i> 캐릭터 로어
- <i class="fa-solid fa-passport"></i> 채팅 로어: 채팅을 [World Info](/Usage/worldinfo.md)에 연결
- <i class="fa-solid fa-file-export"></i> 캐릭터 내보내기
- <i class="fa-solid fa-clone"></i> 복제
- <i class="fa-solid fa-skull"></i> 삭제

#### 확장 옵션

* World Info 연결
* 카드 로어 가져오기
* 시나리오 재정의
* 페르소나 변환
* 캐릭터 이름 변경
* 소스 연결
* 교체/업데이트
* 태그 가져오기
* 갤러리 보기

#### 콘텐츠 필드

* **[캐릭터 설명](characterdesign.md#character-description)**: 간단한 캐릭터 요약
* **[첫 메시지](characterdesign.md#first-message)**: 새 채팅을 시작할 때의 초기 인사말 또는 프롬프트
* **대체 인사말**: 채팅을 시작할 때 스와이프할 수 있는 여러 첫 메시지 정의

### 고급 정의 패널

<i class="fa-solid fa-book"></i> **Advanced Definitions** 버튼을 클릭하여 확장된 캐릭터 설정에 액세스합니다.

#### 프롬프트 재정의 (Chat Completion/Instruct Mode)

* **Main Prompt**: 기본 [메인/시스템 프롬프트](/Usage/Prompts/index.md#main-prompt-system-prompt)를 대체하며, \{\{original\}\} 플레이스홀더를 사용하여 원래 프롬프트를 포함할 수 있습니다
* **Post-History Instructions**: 기본 [post-history instructions](/Usage/Prompts/index.md#post-history-instructions)를 재정의합니다

#### 제작자 메타데이터

캐릭터에 대한 비-프롬프트 정보:

- 제작자 이름/연락처
- 캐릭터 버전
- 제작자 노트
- 내장 태그 목록

#### 캐릭터 성격

* **[성격 요약](characterdesign.md#personality-summary)**: 캐릭터의 특성에 대한 간단한 개요
* **[시나리오](characterdesign.md#scenario)**: 대화의 맥락과 상황
* **Character's Note**: 선택 가능한 깊이 및 메시지 역할이 있는 사용자 정의 메시지 ([작가 노트](/Usage/Characters/Author's-Note.md)도 참조)
* **Talkativeness** (그룹 채팅): 수줍음 → 보통 → 수다스러움 슬라이더
* **Example Messages**: 캐릭터의 작문 스타일 예시

### 그룹 채팅 관리

그룹 채팅인 경우, 이 패널에서 그룹 멤버와 설정을 관리할 수 있습니다.

자세한 내용은 [그룹 채팅](/Usage/Characters/groupchats.md)을 참조하세요.
