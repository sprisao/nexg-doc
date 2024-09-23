작성일: 2024년 9월 19일  
버전: 1.0

---

## 목차

1. **고도화 파트**
   - 1. 전원 모니터링 기능 - 전류/전압
     - 1.1 주요 파일 및 컴포넌트
     - 1.2 주요 기능 설명
     - 1.3 API 연동
   - 2. 대시보드 고도화
     - 2.1 주요 파일 및 컴포넌트
     - 2.2 주요 기능 설명
     - 2.3 API 연동
   - 3. 로그 고도화 - 패킷덤프
     - 3.1 파일명
     - 3.2 주요 기능
     - 3.3 컴포넌트 구조
     - 3.4 동작 흐름
     - 3.5 의존성
   - 4. 로그 고도화 - 실시간 로그
     - 4.1 파일명
     - 4.2 주요 기능
     - 4.3 컴포넌트 구조
     - 4.4 동작 흐름
     - 4.5 의존성
   - 5. 토폴로지 고도화
     - 5.1 파일 구조
     - 5.2 주요 기능 설명
     - 5.3 디바이스 이미지 변경 가이드
     - 5.4 주요 함수 목록
   - 6. BWT 기능
     - 6.1 파일명
     - 6.2 컴포넌트 구조
     - 6.3 컴포넌트 간 상호 작용
     - 6.4 주요 기능 흐름
     - 6.5 API 연동
     - 6.6 주요 상태 관리
     - 6.7 컴포넌트별 주요 Props
     - 6.8 오류 처리
   - 7. ARP 기능
     - 7.1 파일명
     - 7.2 주요 기능
     - 7.3 컴포넌트 구조
     - 7.4 데이터 구조
     - 7.5 API 연동
     - 7.6 컴포넌트 렌더링
     - 7.7 주요 로직 흐름
     - 7.8 오류 처리
   - 8. 미사용 룰 검색 기능
     - 8.1 파일명
     - 8.2 주요 기능
     - 8.3 컴포넌트 구조
     - 8.4 API 연동
     - 8.5 주요 로직 흐름
     - 8.6 오류 처리
   - 9. 인터페이스/VPN(EIX) 장애 유예시간 설정 및 적용
     - 9.1 파일명
     - 9.2 주요 기능
     - 9.3 주요 상태
     - 9.4 API 연동
     - 9.5 EIX와 인터페이스 관련 주요 설정
   - 10. 장애 알람 적용 주기 기능
     - 10.1 파일명
     - 10.2 컴포넌트 구조
     - 10.3 컴포넌트 간 상호 작용
     - 10.4 주요 기능 흐름
     - 10.5 공통 상태 및 함수
     - 10.6 API 연동
     - 10.7 주요 상태 관리
     - 10.8 컴포넌트별 주요 Props
     - 10.9 오류 처리
   - 11. 장애 등급 설정 및 등급에 따른 팝업/알림 기능 개선
     - 11.1 파일명
     - 11.2 컴포넌트 구조
     - 11.3 컴포넌트 간 상호 작용
     - 11.4 주요 기능 흐름
     - 11.5 공통 상태 및 함수
     - 11.6 API 연동
     - 11.7 주요 상태 관리
     - 11.8 컴포넌트별 주요 Props
     - 11.9 오류 처리

2. **백엔드 연결**
   - 1. 환경설정
     - 1.1 application.properties 설정
     - 1.2 mariadb 설정
     - 1.3 mariadb DB 설정이 두 개 이상일 경우
     - 1.4 mongo DB 설정
     - 1.5 RabbitMQ 설정
     - 1.6 RRD 파일
     - 1.7 EsmIO 설정
   - 2. 인증
     - 2.1 application.properties
     - 2.2 JWT 기능
     - 2.3 권한 - 접근 권한
   - 3. Cross 도메인 설정
   - 4. REST API 설정

---

## 1. 고도화 파트

### 1. 전원 모니터링 기능 – 전류/전압

#### 1.1 주요 파일 및 컴포넌트

**1. constants.ts**
- 역할: 전류 및 전압 관련 상수 정의
- 주요 내용:
  - 전류와 전압 차트를 위한 `LineData` 정의
  - 차트 색상 및 단위 설정

**2. TopNComponent.tsx**
- 역할: 전류 및 전압 Top N 데이터 표시
- 주요 기능:
  - 전류 Top N 및 전압 Top N 테이블 렌더링
  - 데이터 정렬 및 필터링

**3. dashboard_currentTopN_columns.tsx**
- 역할: 전류 Top N 테이블 컬럼 정의
- 주요 기능:
  - 전류 값 포맷팅 (mA, A, kA 단위 변환)
  - 정렬 기능 구현

**4. dashboard_voltageTopN_columns.tsx**
- 역할: 전압 Top N 테이블 컬럼 정의
- 주요 기능:
  - 전압 값 포맷팅 (V, kV 단위 변환)
  - 정렬 기능 구현

**5. MainDeviceInformation.tsx**
- 역할: 메인 대시보드에서 전류 및 전압 정보 표시
- 주요 기능:
  - 전류 및 전압 Top N 데이터 조회 및 표시
  - 데이터 갱신 주기 관리

**6. TotalDeviceInformation_TableColumns.tsx**
- 역할: 전체 장비 목록 테이블에 전류 및 전압 컬럼 추가
- 주요 기능:
  - 전류 및 전압 값 포맷팅 및 표시
  - 정렬 및 필터링 기능 구현

**7. Status.tsx**
- 역할: 개별 장비 상태 페이지에 전류 및 전압 정보 표시
- 주요 기능:
  - 실시간 전류 및 전압 데이터 조회 및 표시
  - 데이터 갱신 관리

**8. StatisticsPerformance.tsx**
- 역할: 전류 및 전압 통계 차트 표시
- 주요 기능:
  - 시간별, 장비별 전류 및 전압 데이터 차트 렌더링
  - 차트 데이터 필터링 및 기간 선택 기능

**9. DeviceGroupThresholdSetting_IndividualSetting.tsx**
- 역할: 개별 장비의 전류 및 전압 임계값 설정
- 주요 기능:
  - 전류 및 전압 임계값 설정 UI 제공
  - 임계값 저장 및 적용

**10. DeviceGroupThresholdSetting_DefaultSetting.tsx**
- 역할: 기본 전류 및 전압 임계값 설정
- 주요 기능:
  - 전체 장비에 적용될 기본 전류 및 전압 임계값 설정 UI 제공
  - 기본 임계값 저장 및 적용

**11. SmtpSendEvent.tsx**
- 역할: 전류 및 전압 관련 SMTP 이벤트 설정 표시
- 주요 기능:
  - 현재 설정된 전류 및 전압 관련 SMTP 이벤트 설정 조회 및 표시

**12. SmtpSendEvent_Setting.tsx**
- 역할: 전류 및 전압 관련 SMTP 이벤트 설정 관리
- 주요 기능:
  - 전류 및 전압 관련 SMTP 이벤트 설정 UI 제공
  - 설정 저장 및 적용

#### 1.2 주요 기능 설명

**1. 전류 및 전압 데이터 표시**
- 구현 위치: `MainDeviceInformation.tsx`, `TotalDeviceInformation_TableColumns.tsx`, `Status.tsx`
- 주요 로직:
  - API를 통해 전류 및 전압 데이터 조회
  - 데이터 단위 변환 (mA → A → kA, V → kV)
  - 테이블 및 차트를 통한 데이터 시각화

**2. Top N 기능**
- 구현 위치: `TopNComponent.tsx`, `dashboard_currentTopN_columns.tsx`, `dashboard_voltageTopN_columns.tsx`
- 주요 로직:
  - 전류 및 전압 사용량 기준 상위 N개 장비 데이터 조회
  - 데이터 정렬 및 필터링
  - 테이블 형태로 Top N 데이터 표시

**3. 통계 및 차트**
- 구현 위치: `StatisticsPerformance.tsx`
- 주요 로직:
  - 시간별, 장비별 전류 및 전압 데이터 조회
  - Line 차트를 사용한 데이터 시각화
  - 기간 선택 및 데이터 필터링 기능

**4. 임계값 설정**
- 구현 위치: `DeviceGroupThresholdSetting_IndividualSetting.tsx`, `DeviceGroupThresholdSetting_DefaultSetting.tsx`
- 주요 로직:
  - 개별 장비 및 기본 임계값 설정 UI 제공
  - 입력값 유효성 검사
  - API를 통한 임계값 저장 및 적용

**5. SMTP 이벤트 설정**
- 구현 위치: `SmtpSendEvent.tsx`, `SmtpSendEvent_Setting.tsx`
- 주요 로직:
  - 전류 및 전압 관련 SMTP 이벤트 설정 조회 및 표시
  - 설정 변경 UI 제공
  - API를 통한 설정 저장 및 적용

#### 1.3 API 연동

**1. 전류 및 전압 데이터 조회**
- API 엔드포인트: `/api/monitoring/power`
- 요청 파라미터: `deviceID`, `startTime`, `endTime`
- 응답 데이터 형식:

  ```typescript
  interface PowerData {
    timestamp: string;
    current: number;
    voltage: number;
  }
  ```

**2. 임계값 설정**
- API 엔드포인트: `/api/settings/threshold`
- 요청 데이터 형식:

  ```typescript
  interface ThresholdSettings {
    deviceID?: string; // 개별 설정 시 사용
    currentThreshold: number;
    voltageThreshold: number;
  }
  ```

**3. SMTP 이벤트 설정**
- API 엔드포인트: `/api/settings/smtp-events`
- 요청 데이터 형식:

  ```typescript
  interface SmtpEventSettings {
    currentEnabled: boolean;
    voltageEnabled: boolean;
  }
  ```

---

### 2. 대시보드 고도화

#### 2.1 주요 파일 및 컴포넌트

**1. Dashboard.tsx**
- 역할: 대시보드의 메인 컴포넌트
- 주요 기능:
  - 위젯 표시 및 레이아웃 관리
  - 위젯 추가/제거
  - 레이아웃 저장 및 로드
  - 갱신 주기 설정

**2. GridItem.tsx**
- 역할: 개별 위젯의 컨테이너 컴포넌트
- 주요 기능:
  - 위젯 타이틀 표시
  - 드래그 핸들 제공
  - 스크롤 가능한 콘텐츠 영역 제공

**3. src/screens/Dashboard/constants.tsx**
- 역할: 대시보드 관련 상수 및 설정 정의
- 주요 내용:
  - 컴포넌트 맵 정의
  - 기본 위젯 설정

**4. src/screens/Dashboard/types.ts**
- 역할: 대시보드 관련 타입 정의

#### 2.2 주요 기능 설명

**1. 위젯 관리**
- 사용자는 다양한 위젯을 추가/제거할 수 있습니다.
- 위젯 목록은 `constants.tsx`의 `allWidgets` 배열에 정의되어 있습니다.
- 위젯 추가/제거는 `Dashboard.tsx`의 `addWidget` 및 `removeWidget` 함수로 처리됩니다.

**2. 레이아웃 관리**
- `react-grid-layout` 라이브러리를 사용하여 드래그 앤 드롭 기능을 구현했습니다.
- 레이아웃 변경 시 `onLayoutChange` 함수가 호출되어 상태를 업데이트합니다.
- 레이아웃은 사용자별로 저장되며, 페이지 로드 시 자동으로 불러옵니다.

**3. 데이터 갱신**
- 각 위젯은 독립적으로 데이터를 갱신합니다.
- 갱신 주기는 사용자가 설정할 수 있으며, `DeviceContext`를 통해 관리됩니다.

**4. 위젯 종류**
- 장비 현황 (PieChart)
- 주간 로그 발생 통계 (WeeklyLogs)
- 장비 장애 로그 (DeviceFaultLog)
- 최근 장애 목록 (RecentFailureList)
- 리소스 TopN (ResourceTopN)
- 주요 데몬 상태 (DaemonStatus)
- 시스템 자원 현황 (SystemResource)

#### 2.3 API 연동

**1. 사용자 대시보드 위젯 관리**
- `getUserDashboardWidgets`: 사용자의 대시보드 위젯 설정 조회
- `setUserDashboardWidgets`: 사용자의 대시보드 위젯 설정 저장

**2. 위젯별 데이터 조회**
- 각 위젯 컴포넌트에서 필요한 API를 호출하여 데이터를 조회합니다.
- 주요 API 목록:
  - `getDeviceFaultStatus`: 장비 현황 데이터 조회
  - `WeeklyLog`: 주간 로그 발생 통계 조회
  - `EtcLogsLastErrorList`: 장비 장애 로그 조회
  - `LastFailDevice`: 최근 장애 목록 조회
  - 각종 TopN API (CPU, Memory, Disk, Session 등)

---

### 3. 로그 고도화 – 패킷덤프

#### 3.1 파일명

- `PacketDump.tsx`

#### 3.2 주요 기능

- 디바이스 인터페이스 목록 조회 및 선택
- WebSocket을 통한 실시간 패킷 덤프 데이터 수신
- 패킷 덤프 시작/정지 제어
- 실시간 로그 표시 (최대 100개 항목)

#### 3.3 컴포넌트 구조

**1. Props**

- `deviceID`: `string` - 대상 디바이스 ID
- `mode`: `string` - 동작 모드

**2. 주요 상태(State)**

- `logs`: 수신된 로그 데이터 배열
- `connectionStatus`: WebSocket 연결 상태
- `selectedUseDropDownValue`: 선택된 인터페이스 값
- `formattedResponse`: 인터페이스 목록 (드롭다운용)

**3. 주요 함수**

- `fetchGetDeviceInterface()`: 디바이스 인터페이스 목록 조회
- `connect()`: WebSocket 연결 시작
- `disconnect()`: WebSocket 연결 종료
- `onMessageReceived()`: WebSocket 메시지 수신 및 처리
- `handleStartStop()`: 패킷 덤프 시작/정지 토글
- `handleDropdownChange()`: 인터페이스 선택 변경 처리

#### 3.4 동작 흐름

1. 컴포넌트 마운트 시 `fetchGetDeviceInterface()`를 호출하여 인터페이스 목록을 가져옵니다.
2. 사용자가 인터페이스를 선택하고 **시작** 버튼을 클릭하면 `connect()` 함수가 호출됩니다.
3. WebSocket 연결이 성공하면 `/sub/tcpdump` 주소를 호출하고, `/pub/tcpdump`로 초기 메시지를 전송합니다.
4. 서버로부터 패킷 덤프 데이터를 수신하면 `onMessageReceived()` 함수에서 처리하여 `logs` 상태를 업데이트합니다.
5. 로그는 화면에 실시간으로 표시되며, 최신 100개 항목만 유지됩니다.
6. 사용자가 **정지** 버튼을 클릭하면 `disconnect()` 함수가 호출되어 WebSocket 연결을 종료합니다.

#### 3.5 의존성

- React
- SockJS
- `@stomp/stompjs`
- 기타 UI 컴포넌트 (`@/components/ui/button`, `@/components/NormalDropdown` 등)

#### 3.6 환경 설정

- `REACT_APP_BACKEND_TOBE_API_HOST` 환경 변수: WebSocket 연결에 사용되는 백엔드 API 호스트 주소

---

### 4. 로그 고도화 – 실시간 로그

#### 4.1 파일명

- `MonitoringLog.tsx`

#### 4.2 주요 기능

- 로그 유형 선택 (드롭다운 메뉴)
- WebSocket을 통한 실시간 로그 데이터 수신
- 로그 스트리밍 시작/정지 제어
- 실시간 로그 표시 (최대 100개 항목)
- 로그 레벨에 따른 색상 구분

#### 4.3 컴포넌트 구조

**1. 주요 상태(State)**

- `logs`: 수신된 로그 데이터 배열
- `isStreaming`: 로그 스트리밍 상태
- `isConnecting`: WebSocket 연결 시도 상태
- `selectedDropDownValue`: 선택된 로그 유형 값

**2. 컨텍스트 사용**

- `ModeContext`: 현재 모드 정보 제공
- `AuthContext`: 인증 토큰 제공

**3. 주요 함수**

- `connect()`: WebSocket 연결 시작
- `disconnect()`: WebSocket 연결 종료
- `onMessageReceived()`: WebSocket 메시지 수신 및 처리
- `handleStartStop()`: 로그 스트리밍 시작/정지 토글
- `handleDropdownChange()`: 로그 유형 선택 변경 처리
- `getDropdownOptions()`: 모드에 따른 드롭다운 옵션 생성

#### 4.4 동작 흐름

1. 컴포넌트 마운트 시 현재 모드에 따른 드롭다운 옵션을 설정합니다.
2. 사용자가 로그 유형을 선택하고 **시작** 버튼을 클릭하면 `connect()` 함수가 호출됩니다.
3. WebSocket 연결이 성공하면 `/sub/tcpdump` 주소를 호출하고, `/pub/tcpdump`로 초기 메시지를 전송합니다.
4. 서버로부터 로그 데이터를 수신하면 `onMessageReceived()` 함수에서 처리하여 `logs` 상태를 업데이트합니다.
5. 로그는 화면에 실시간으로 표시되며, 최신 100개 항목만 유지됩니다.
6. 사용자가 **정지** 버튼을 클릭하면 `disconnect()` 함수가 호출되어 WebSocket 연결을 종료합니다.
7. 로그 유형을 변경하면 WebSocket 연결이 활성화된 상태에서 새로운 메시지를 서버로 전송합니다.

#### 4.5 의존성

- React
- SockJS
- `@stomp/stompjs`
- 기타 UI 컴포넌트 (`@/components/ui/button`, `@/components/NormalDropdown` 등)

#### 4.6 환경 설정

- `REACT_APP_BACKEND_TOBE_API_HOST` 환경 변수: WebSocket 연결에 사용되는 백엔드 API 호스트 주소

---

### 5. 토폴로지 고도화

#### 5.1 파일 구조

토폴로지 기능은 다음과 같은 주요 컴포넌트로 구성되어 있습니다:

- `Topology.tsx`: 메인 토폴로지 컴포넌트
- `TopologyHooks.ts`: 토폴로지 데이터 관리 훅
- `TopologyUtils.ts`: 노드 렌더링 및 유틸리티 함수
- `TopologyContextMenu.tsx`: 컨텍스트 메뉴 구현
- `Topology_ActionPanel.tsx`: 액션 버튼 패널
- `windowUtils.ts`: 새 창 관련 유틸리티 함수

#### 5.2 주요 기능 설명

**1. 노드 렌더링 및 상태 표시**
- 디바이스 종류에 따라 다른 아이콘 사용 (small, medium, large)
- 디바이스 상태에 따른 색상 변경 (normal, error, warning, alert)
- 인터페이스 상태를 캡슐 모양으로 표시

**2. 노드 접기/펼치기**
- 그룹 노드 더블 클릭 시 하위 노드 접기/펼치기 기능
- 재귀적으로 하위 노드의 `viewFlag` 업데이트

**3. 데이터 저장 및 복원**
- 노드 위치 정보를 `localStorage`에 저장
- 페이지 새로고침 시 저장된 위치 정보 복원

**4. 줌 및 패닝**
- `ForceGraph2D`의 줌 및 패닝 기능 활용
- 줌 레벨 및 위치 정보 `localStorage`에 저장 및 복원

**5. 컨텍스트 메뉴**
- 디바이스 및 그룹 노드별 다른 메뉴 항목 제공
- 새 창에서 상세 정보 열기 기능

#### 5.3 디바이스 이미지 변경 가이드

디바이스 이미지를 변경하려면 다음 단계를 따르세요:

1. `TopologyUtils.ts` 파일을 엽니다.
2. 파일 상단의 이미지 `import` 섹션을 찾아 필요한 이미지를 수정하거나 추가합니다.
3. `deviceImages` 객체를 찾아 해당 이미지 참조를 업데이트합니다.
4. 필요한 경우 `drawDeviceNode` 함수 내의 로직을 수정하여 새로운 이미지 처리 방식을 구현합니다.
5. 이미지 크기나 위치 조정이 필요한 경우, `drawImage` 함수 호출 부분을 수정합니다.

#### 5.4 주요 함수 목록

| 파일명                 | 함수명                | 설명                   |
|------------------------|-----------------------|------------------------|
| **Topology.tsx**       | `Topology`            | 전체 토폴로지 렌더링    |
|                        | `handleRefresh`       | 토폴로지 데이터 새로고침 |
|                        | `handleReset`         | 토폴로지 데이터 초기화   |
| **TopologyHooks.ts**   | `useTopologyData`     | 토폴로지 데이터 관리 훅  |
|                        | `processDevices`      | 디바이스 데이터 처리     |
|                        | `toggleFoldChildNodes`| 자식 노드 접기/펼치기    |
| **TopologyUtils.ts**   | `paintNode`           | 노드 렌더링             |
|                        | `drawDeviceNode`      | 디바이스 노드 그리기     |
|                        | `drawInterfaceStatus` | 인터페이스 상태 표시     |
| **TopologyContextMenu.tsx** | `TopologyContextMenu` | 디바이스 노드 컨텍스트 메뉴 |
| **Topology_ActionPanel.tsx** | `Topology_ActionPanel` | 액션 버튼 패널          |

---

### 6. BWT 기능

#### 6.1 파일명

- `BWTSetting.tsx`
- `BWTSetting_defaultSetting.tsx`
- `BWTSetting_individualSetting.tsx`

#### 6.2 컴포넌트 구조

**1. `BWTSetting` (메인 컴포넌트)**
- 역할: 전체 BWT 설정 페이지의 레이아웃과 로직을 관리
- 주요 기능:
  - 장비/그룹 선택
  - 개별 설정 및 기본 설정 표시
  - 설정 모달 열기/닫기 제어

**2. `BWTSetting_defaultSetting`**
- 역할: 기본 BWT 설정을 관리하는 모달 컴포넌트
- 주요 기능:
  - 기본 BWT 설정 조회 및 수정
  - 설정 적용 및 결과 처리

**3. `BWTSetting_individualSetting`**
- 역할: 개별 장비/그룹의 BWT 설정을 관리하는 모달 컴포넌트
- 주요 기능:
  - 선택된 장비/그룹의 BWT 설정 조회 및 수정
  - 설정 적용 및 결과 처리

#### 6.3 컴포넌트 간 상호 작용

1. **`BWTSetting` (메인 컴포넌트)**:
   - 장비/그룹 선택 시 `selectedItem` 상태 업데이트
   - 개별 설정 버튼 클릭 시 `BWTSetting_individualSetting` 모달 열기
   - 기본 설정 버튼 클릭 시 `BWTSetting_defaultSetting` 모달 열기
   - 각 설정 모달이 닫힐 때 `fetchBWTConfigInfo` 및 `fetchDefaultBWTConfigInfo` 호출하여 데이터 새로고침

2. **`BWTSetting_defaultSetting`**:
   - 열릴 때 `fetchDefaultBWTConfigInfo`를 통해 기본 설정 데이터 로드
   - 설정 변경 및 적용 시 `setBwtConfigInfo` API 호출
   - 적용 완료 후 결과 모달 표시 및 `BWTSetting`의 `onApply` 콜백 호출

3. **`BWTSetting_individualSetting`**:
   - 열릴 때 `fetchBWTConfigInfo`를 통해 선택된 장비/그룹의 설정 데이터 로드
   - 설정 변경 및 적용 시 `setBwtConfigInfo` API 호출
   - 적용 완료 후 결과 모달 표시 및 `BWTSetting`의 `onApply` 콜백 호출

#### 6.4 주요 기능 흐름

1. 사용자가 `BWTSetting` 페이지 진입
2. 기본 설정 및 장비/그룹 목록 로드
3. 사용자가 장비/그룹 선택
4. 선택된 항목의 BWT 설정 정보 표시
5. 사용자가 개별 설정 또는 기본 설정 버튼 클릭
6. 해당 설정 모달 열림
7. 사용자가 설정 변경 및 적용
8. 설정 적용 결과 표시 및 메인 화면 데이터 새로고침

#### 6.5 API 연동

- `getBwtConfigInfo`: BWT 설정 정보 조회
- `setBwtConfigInfo`: BWT 설정 정보 업데이트

#### 6.6 주요 상태 관리

- `isIndividualSettingModalOpen`: 개별 설정 모달 열림 상태
- `isNormalSettingModalOpen`: 기본 설정 모달 열림 상태
- `alarmInfo`: 현재 선택된 항목의 BWT 설정 정보
- `basicAlarmInfo`: 기본 BWT 설정 정보

#### 6.7 컴포넌트별 주요 Props

**`BWTSetting_defaultSetting`**
- `isOpen`: 모달 열림 상태
- `onClose`: 모달 닫기 함수
- `onApply`: 설정 적용 후 호출되는 콜백 함수

**`BWTSetting_individualSetting`**
- `isOpen`: 모달 열림 상태
- `onClose`: 모달 닫기 함수
- `selectedItem`: 선택된 장비/그룹 정보
- `onApply`: 설정 적용 후 호출되는 콜백 함수

#### 6.8 오류 처리

- API 호출 실패 시 콘솔에 에러 로깅
- 사용자에게 오류 메시지 표시 (`AlertModal` 사용)

---

### 7. ARP 기능

#### 7.1 파일명

- `ARP.tsx`

#### 7.2 주요 기능

- 특정 디바이스의 ARP 테이블 조회
- ARP 엔트리 목록 표시
- ARP 테이블 갱신 기능

#### 7.3 컴포넌트 구조

**1. Props**

- `deviceID`: `string` - 대상 디바이스 ID

**2. 주요 상태(State)**

- `arpEntries`: `ARPEntry[]` - ARP 테이블 엔트리 목록

**3. 주요 함수**

- `fetchSendUTMIPRuleArp()`: ARP 테이블 데이터 조회
- `parseArpEntries(message: string)`: API 응답 메시지를 파싱하여 ARP 엔트리 객체 배열로 변환

#### 7.4 데이터 구조

**1. `ARPEntry` 인터페이스**

```typescript
interface ARPEntry {
    ipAddress: string;
    macAddress: string;
    interface: string;
    type: string;
}
```

#### 7.5 API 연동

- `sendUTMIPRuleArp`: ARP 테이블 조회 API

**1. 요청 타입**

```typescript
interface SendUTMIPRuleArpRequest {
    deviceID: string;
}
```

**2. 응답 타입**

```typescript
interface BasicSingleObjectResponse<SendUTMIPRuleArpData> {
    success: string;
    entitys?: {
        message: string;
    };
}
```

#### 7.6 컴포넌트 렌더링

- `SectionTitle`: "ARP" 제목 표시
- `Button`: ARP 테이블 갱신 버튼
- `CommonTable`: ARP 엔트리 목록 표시

**1. 테이블 컬럼 정의**

```typescript
const columns: ColumnDef<ARPEntry>[] = [
    { header: 'IP Address', accessorKey: 'ipAddress' },
    { header: 'MAC Address', accessorKey: 'macAddress' },
    { header: 'Interface', accessorKey: 'interface' },
    { header: 'Type', accessorKey: 'type' },
];
```

#### 7.7 주요 로직 흐름

1. 컴포넌트 마운트 시 `fetchSendUTMIPRuleArp` 함수 호출
2. API로부터 ARP 테이블 데이터 수신
3. `parseArpEntries` 함수를 통해 수신된 데이터 파싱
4. 파싱된 데이터로 `arpEntries` 상태 업데이트
5. `CommonTable`을 통해 ARP 엔트리 목록 표시
6. 사용자가 갱신 버튼 클릭 시 `fetchSendUTMIPRuleArp` 함수 재호출

#### 7.8 오류 처리

- API 호출 실패 시 콘솔에 에러 로깅

---

### 8. 미사용 룰 검색 기능

#### 8.1 파일명

- `NotUsedPolicy.tsx`

#### 8.2 주요 기능

- 미사용 정책 검색 조건 설정 (시간 단위 및 값)
- 미사용 정책 조회 및 표시
- 데이터 새로고침

#### 8.3 컴포넌트 구조

**1. Props**

- `deviceID`: `string` - 대상 디바이스 ID

**2. 주요 상태(State)**

- `selectedOption`: `string` - 선택된 시간 단위 (`hour`, `day`, `month`, `year`)
- `inputValue`: `string` - 입력된 시간 값

**3. 주요 함수**

- `fetchSendNonUTMIPPule()`: 미사용 정책 데이터 조회
- `handleAction1Click()`: 테이블 액션 버튼 클릭 핸들러 (현재 구현되지 않음)
- `getMaxValue()`: 선택된 시간 단위에 따른 최대 입력 값 반환
- `getLabel()`: 선택된 시간 단위에 따른 입력 범위 라벨 반환

#### 8.4 API 연동

- `SendNonUTMIPPolicy`: 미사용 정책 조회 API

**1. 요청 타입**

```typescript
interface SendNonUTMIPPolicyRequest {
    deviceID: string;
    type: string;
    typeVal: string;
}
```

#### 8.5 주요 로직 흐름

1. 컴포넌트 마운트 시 `fetchSendNonUTMIPPule` 함수 호출
2. 사용자가 시간 단위 및 값 선택/입력
3. 검색 버튼 클릭 시 `fetchSendNonUTMIPPule` 함수 호출
4. API로부터 미사용 정책 데이터 수신
5. `CommonTable`을 통해 미사용 정책 목록 표시
6. 새로고침 버튼 클릭 시 `fetchSendNonUTMIPPule` 함수 재호출

#### 8.6 오류 처리

- API 호출 실패 시 콘솔에 에러 로깅

---

### 9. 인터페이스/VPN(EIX) 장애 유예시간 설정 및 적용

#### 9.1 파일명

- `ErrorLineManagement.tsx`

#### 9.2 주요 기능

- Up/Down 반복 회선 임계치 표시
- 장시간 Down 회선 임계치 표시
- 장애 유예 시간 (인터페이스 및 EIX) 표시
- 설정 변경을 위한 모달 열기

#### 9.3 주요 상태

- `InterfaceConfig`: 인터페이스 설정 데이터
- `isLoading`: 데이터 로딩 상태
- `isModalOpen`: 설정 모달 열림 상태

#### 9.4 API 연동

- `getInterfaceConfig`: 인터페이스 설정 정보 조회

#### 9.5 EIX와 인터페이스 관련 주요 설정

- **EIX 장애 유예 시간**: EIX 관련 장애 발생 시 알림을 지연시키는 시간
- **인터페이스 장애 유예 시간**: 인터페이스 관련 장애 발생 시 알림을 지연시키는 시간
- **Up/Down 반복 회선 임계치**:
  - 임계 시간: Up/Down 반복을 체크하는 시간 범위
  - 반복수 제한: 해당 시간 내 Up/Down 반복 횟수 제한
- **장시간 Down 회선 임계치**: 회선이 연속으로 Down 상태일 때 장애로 간주하는 시간

---

### 10. 장애 알람 적용 주기 기능

#### 10.1 파일명

- `AlarmExceptionTime.tsx`
- `AlarmExceptionTime_DefaultSetting.tsx`
- `AlarmExceptionTime_IndividualSetting.tsx`

#### 10.2 컴포넌트 구조

**1. `AlarmExceptionTime` (메인 컴포넌트)**
- 역할: 전체 알람 예외 시간 설정 페이지의 레이아웃과 로직을 관리
- 주요 기능:
  - 장비/그룹 선택
  - 개별 설정 및 기본 설정 표시
  - 설정 모달 열기/닫기 제어

**2. `AlarmExceptionTime_DefaultSetting`**
- 역할: 기본 알람 예외 시간 설정을 관리하는 모달 컴포넌트
- 주요 기능:
  - 기본 알람 예외 시간 설정 조회 및 수정
  - 설정 적용 및 결과 처리

**3. `AlarmExceptionTime_IndividualSetting`**
- 역할: 개별 장비/그룹의 알람 예외 시간 설정을 관리하는 모달 컴포넌트
- 주요 기능:
  - 선택된 장비/그룹의 알람 예외 시간 설정 조회 및 수정
  - 설정 적용 및 결과 처리

#### 10.3 컴포넌트 간 상호 작용

1. **`AlarmExceptionTime` (메인 컴포넌트)**:
   - 장비/그룹 선택 시 `selectedItem` 상태 업데이트
   - 개별 설정 버튼 클릭 시 `AlarmExceptionTime_IndividualSetting` 모달 열기
   - 기본 설정 버튼 클릭 시 `AlarmExceptionTime_DefaultSetting` 모달 열기
   - 각 설정 모달이 닫힐 때 `fetchIndividualAlarmInfo` 및 `fetchBasicAlarmInfo` 호출하여 데이터 새로고침

2. **`AlarmExceptionTime_DefaultSetting`**:
   - 열릴 때 `getValidInformationStr`를 통해 기본 설정 데이터 로드
   - 설정 변경 및 적용 시 `UpdateValidInformation` 및 `setAlarmExceptTime` API 호출
   - 적용 완료 후 결과 모달 표시 및 `AlarmExceptionTime`의 `onApply` 콜백 호출

3. **`AlarmExceptionTime_IndividualSetting`**:
   - 열릴 때 `getValidInformationStr`를 통해 선택된 장비/그룹의 설정 데이터 로드
   - 설정 변경 및 적용 시 `UpdateValidInformation` 및 `setAlarmExceptTime` API 호출
   - 적용 완료 후 결과 모달 표시 및 `AlarmExceptionTime`의 `onApply` 콜백 호출

#### 10.4 주요 기능 흐름

1. 사용자가 `AlarmExceptionTime` 페이지 진입
2. 기본 설정 및 장비/그룹 목록 로드
3. 사용자가 장비/그룹 선택
4. 선택된 항목의 알람 예외 시간 설정 정보 표시
5. 사용자가 개별 설정 또는 기본 설정 버튼 클릭
6. 해당 설정 모달 열림
7. 사용자가 설정 변경 및 적용
8. 설정 적용 결과 표시 및 메인 화면 데이터 새로고침

#### 10.5 공통 상태 및 함수

- `selectedItem`: 현재 선택된 장비/그룹 정보
- `fetchIndividualAlarmInfo`: 개별 알람 예외 시간 설정 정보 조회
- `fetchBasicAlarmInfo`: 기본 알람 예외 시간 설정 정보 조회
- `setAlarmExceptTime`: 알람 예외 시간 설정 정보 업데이트 API 호출

#### 10.6 API 연동

- `getValidInformationStr`: 알람 예외 시간 설정 정보 조회
- `UpdateValidInformation`: 알람 예외 시간 설정 정보 업데이트
- `setAlarmExceptTime`: 알람 예외 시간 설정 적용
- `getDeviceInfo`: 디바이스 정보 조회
- `getDeviceGroupInfo`: 디바이스 그룹 정보 조회

#### 10.7 주요 상태 관리

- `isIndividualSettingModalOpen`: 개별 설정 모달 열림 상태
- `isNormalSettingModalOpen`: 기본 설정 모달 열림 상태
- `individualAlarmInfo`: 현재 선택된 항목의 알람 예외 시간 설정 정보
- `basicAlarmInfo`: 기본 알람 예외 시간 설정 정보
- `daySettings`: 각 요일별 알람 예외 시간 설정

#### 10.8 컴포넌트별 주요 Props

**`AlarmExceptionTime_DefaultSetting`**
- `isOpen`: 모달 열림 상태
- `onClose`: 모달 닫기 함수
- `onApply`: 설정 적용 후 호출되는 콜백 함수

**`AlarmExceptionTime_IndividualSetting`**
- `isOpen`: 모달 열림 상태
- `onClose`: 모달 닫기 함수
- `selectedItem`: 선택된 장비/그룹 정보
- `onApply`: 설정 적용 후 호출되는 콜백 함수

#### 10.9 오류 처리

- API 호출 실패 시 콘솔에 에러 로깅
- 사용자에게 오류 메시지 표시 (`AlertModal` 사용)

---

### 11. 장애 등급 설정 및 등급에 따른 팝업/알림 기능 개선

#### 11.1 파일명

- `EventLevelSetting.tsx`
- `EventLevelSetting_Setting.tsx`

#### 11.2 컴포넌트 구조

**1. `EventLevelSetting` (메인 컴포넌트)**
- 역할: 이벤트 등급 설정 목록을 표시하고 관리하는 페이지
- 주요 기능:
  - 이벤트 등급 목록 조회 및 표시
  - 개별 이벤트 등급 설정 모달 열기

**2. `EventLevelSetting_Setting`**
- 역할: 개별 이벤트 등급 설정을 관리하는 모달 컴포넌트
- 주요 기능:
  - 선택된 이벤트의 등급, 알람 설정, 설명, 알람 종류 조회 및 수정
  - 설정 적용 및 결과 처리

#### 11.3 컴포넌트 간 상호 작용

1. **`EventLevelSetting` (메인 컴포넌트)**:
   - 이벤트 등급 목록 조회 (`fetchGetEventClassification`)
   - 테이블의 수정 버튼 클릭 시 `EventLevelSetting_Setting` 모달 열기
   - 선택된 이벤트 정보를 `EventLevelSetting_Setting`에 전달

2. **`EventLevelSetting_Setting`**:
   - 전달받은 이벤트 정보로 초기 상태 설정
   - 설정 변경 및 적용 시 `setEventClassification` API 호출
   - 적용 완료 후 결과 모달 표시 및 `EventLevelSetting`의 `onApply` 콜백 호출

#### 11.4 주요 기능 흐름

1. 사용자가 `EventLevelSetting` 페이지 진입
2. 이벤트 등급 목록 로드 및 표시
3. 사용자가 특정 이벤트의 수정 버튼 클릭
4. `EventLevelSetting_Setting` 모달 열림
5. 사용자가 이벤트 설정 변경 및 적용
6. 설정 적용 결과 표시 및 메인 화면 데이터 새로고침

#### 11.5 공통 상태 및 함수

- `eventClassificationList`: 이벤트 등급 목록
- `selectedEventLevelID`: 선택된 이벤트 ID
- `fetchGetEventClassification`: 이벤트 등급 목록 조회 함수

#### 11.6 API 연동

- `getEventClassification`: 이벤트 등급 목록 조회
- `setEventClassification`: 이벤트 등급 설정 업데이트

#### 11.7 주요 상태 관리

**`EventLevelSetting`**
- `isLoading`: 데이터 로딩 상태
- `eventClassificationList`: 이벤트 등급 목록
- `selectedEventLevelID`: 선택된 이벤트 ID
- `isModifyEventLevelModalOpen`: 수정 모달 열림 상태

**`EventLevelSetting_Setting`**
- `selectedLevelValue`: 선택된 등급
- `selectedAlarmValue`: 선택된 알람 설정
- `selectedAlarmTypes`: 선택된 알람 종류
- `description`: 이벤트 설명

#### 11.8 컴포넌트별 주요 Props

**`EventLevelSetting_Setting`**
- `isOpen`: 모달 열림 상태
- `onClose`: 모달 닫기 함수
- `onApply`: 설정 적용 후 호출되는 콜백 함수
- `data`: 선택된 이벤트 정보

#### 11.9 오류 처리

- API 호출 실패 시 콘솔에 에러 로깅
- 사용자에게 오류 메시지 표시 (`AlertModal` 사용)

---

## 2. 백엔드 연결

### 1. 환경설정

#### 1.1 application.properties 설정

- 경로: `/nexgEsm/src/main/resources/application.properties`
- 설정:

  ```
  spring.profiles.active=local   # 로컬
  #spring.profiles.active=dev    # 개발
  #spring.profiles.active=prod   # 서버
  ```

- 개발 시 주석 제거 후 Spring Boot App 실행

#### 1.2 mariadb 설정 – 서버 경우 (`application-prod.properties`)

- 경로: `/nexgEsm/src/main/resources/application-prod.properties`
- 설정:

  ```
  spring.datasource.sesm.driverClassName=net.sf.log4jdbc.sql.jdbcapi.DriverSpy
  spring.datasource.sesm.jdbc-url=jdbc:log4jdbc:mariadb://127.0.0.1/sesm?useUnicode=yes&characterEncoding=UTF-8&allowMultiQueries=true&serverTimezone=Asia/Seoul
  spring.datasource.sesm.username=root
  spring.datasource.sesm.password=root123!@#

  # DB2 Dev Info
  spring.datasource.stat.driverClassName=net.sf.log4jdbc.sql.jdbcapi.DriverSpy
  spring.datasource.stat.jdbc-url=jdbc:log4jdbc:mariadb://127.0.0.1/stat?useUnicode=yes&characterEncoding=UTF-8&allowMultiQueries=true&serverTimezone=Asia/Seoul
  spring.datasource.stat.username=root
  spring.datasource.stat.password=root123!@#
  ```

#### 1.3 mariadb DB 설정이 두 개 이상일 경우

- 경로:
  - `/nexgEsm/src/main/java/kr/nexg/esm/common/DatasourceConfiguration1.java`
  - `/nexgEsm/src/main/java/kr/nexg/esm/common/DatasourceConfiguration2.java`

#### 1.4 mongo DB 설정 – 서버 경우 (`application-prod.properties`)

- 경로:
  - `/nexgEsm/src/main/resources/application-prod.properties`
  - `/nexgEsm/src/main/java/kr/nexg/esm/common/MongoClientManager.java`
- 설정:

  ```
  mongo.host=127.0.0.1
  mongo.port=27017
  spring.data.mongodb.uri=mongodb://127.0.0.1:27017
  ```

#### 1.5 RabbitMQ 설정 – 서버 경우 (`application-prod.properties`)

- 경로:
  - `/nexgEsm/src/main/resources/application-prod.properties`
  - `/nexgEsm/src/main/java/kr/nexg/esm/nexgesm/command/RabbitMQConfig.java`
  - `/nexgEsm/src/main/java/kr/nexg/esm/nexgesm/command/service/RabbitMQService.java`
- 설정:

  ```
  spring.rabbitmq.host=localhost
  spring.rabbitmq.port=5672
  spring.rabbitmq.username=esm
  spring.rabbitmq.password=esm
  ```

#### 1.6 RRD 파일

- 경로: `/nexgEsm/src/main/java/kr/nexg/esm/nexgesm/rrd`

#### 1.7 EsmIO 설정 – 서버 경우 (`application-prod.properties`)

- 경로:
  - `/nexgEsm/src/main/resources/application-prod.properties`
  - `/nexgEsm/src/main/java/kr/nexg/esm/nexgesm/common/EsmConfig.java`
  - `/nexgEsm/src/main/java/kr/nexg/esm/nexgesm/common/EsmIO.java`
- 설정:

  ```
  ecollectd.port=2015
  ecollectd.tlsport=2016
  elogd.port=514
  elogd.tlsport=2514
  agent.port=8888
  ```

---

### 2. 인증

#### 2.1 application.properties

- 경로:
  - `/nexgEsm/src/main/resources/application.properties`
  - `/nexgEsm/src/main/java/kr/nexg/esm/default1/controller/DefaultController.java`
  - `/nexgEsm/src/main/java/kr/nexg/esm/jwt/service/CustomUserDetailsService.java`

#### 2.2 JWT 기능

- 경로:
  - `/nexgEsm/src/main/java/kr/nexg/esm/jwt/JwtTokenProvider.java`
  - `/nexgEsm/src/main/java/kr/nexg/esm/jwt/JwtAuthenticationFilter.java`
- 설정 (`application-prod.properties`):

  ```
  spring.rabbitmq.host=localhost
  spring.rabbitmq.port=5672
  spring.rabbitmq.username=esm
  spring.rabbitmq.password=esm
  ```

#### 2.3 권한 - 접근 권한

- 경로: `/nexgEsm/src/main/java/kr/nexg/esm/jwt/SecurityConfig.java`

---

### 3. Cross 도메인 설정

- 경로: `/nexgEsm/src/main/java/kr/nexg/esm/common/WebMvcConfiguration.java`
- 설정:

  ```java
  public void addCorsMappings(CorsRegistry registry) {
      registry.addMapping("/**")
              .allowedOrigins("*");
  }
  ```

---

### 4. REST API 설정

- 경로:
  - `/nexgEsm/src/main/java/kr/nexg/esm/common/RestTemplateConfig.java`
  - `/nexgEsm/src/main/java/kr/nexg/esm/common/ApiService.java`

---