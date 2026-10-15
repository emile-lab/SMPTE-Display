# 변경 내역 · Changelog

## Beta 0.4 — 2026-10-15

Beta 0.3의 변경 사항(아래)이 모두 포함된 첫 공개 버전입니다. Beta 0.2에서 바로 업데이트할 수 있습니다.

### 버그 수정
- **Arrangement의 클립 선택이 계속 풀리던 문제 해결.** 영상이 있는 Set에서 디바이스가 정지·클릭 직후 재생 헤드를 아주 조금 옮겼고, 이때 클립 선택이 풀렸습니다(특히 24 fps / 120 BPM처럼 격자선이 프레임 경계와 겹칠 때). 이제 디바이스는 **재생 헤드를 스스로 옮기지 않습니다.** 타임코드를 입력해 이동할 때만 옮깁니다.
  - 프레임 경계에서 정지·클릭했을 때는 Live 영상 창과 같은 프레임을 **표시**하는 방식으로 바꿨습니다. Live에는 아무것도 쓰지 않습니다.
  - 그 대신 Beta 0.2의 "정지 후 프레임 정렬"은 없앴습니다. 프레임이 끝나기 직전에서 정지하면 드물게 Live 영상 창이 1프레임 앞을 보여 줄 수 있습니다.

### 검증
- 단위 테스트 155개 통과. 실제 Live 12.4.6에서 정지 8회·클릭 후 재생 헤드 이동 0회, 시작 마커와 Undo 기록 변화 없음. 타임코드 입력 이동 24회 모두 Live 영상 창과 일치.
- 정지 36회 중 5회(프레임 끝 근처에서 정지)는 Live 영상 창이 1프레임 앞이었습니다(위의 알려진 한계).

### What's new (English)
- **Fixed: clip selection in the Arrangement kept getting cleared.** With a video in the Set, the device nudged the playhead right after you stopped or clicked, which cleared the selection. The device now **never moves the playhead on its own** — only when you type a timecode. Frame-boundary positions are handled on the display side instead, showing the same frame as Live's video window. As a trade-off the Beta 0.2 post-stop alignment is gone: stopping just before the end of a frame can occasionally leave Live's video window one frame ahead.
- Includes everything from Beta 0.3 below (burn-in at a different frame rate than the file, Export timecode / Stamp WAV…).

---

## Beta 0.3 — 2026-10-09 (공개되지 않은 빌드 · Beta 0.4에 포함 · not released separately)

### 새 기능 · 개선
- **화면의 타임코드와 파일의 프레임레이트가 달라도 자동 싱크.** 24 fps로 편집한 영상을 30 fps로 내보낸 파일처럼 화면에 찍힌 타임코드(24 fps)와 파일(30 fps)의 프레임레이트가 다르면, 이전 버전은 `BITC rate != 30 fps`를 띄우고 Offset을 맞추지 않았습니다. 이제 화면의 타임코드를 기준으로 삼습니다.
  - 프레임레이트 10종을 모두 시험해 화면 타임코드의 프레임레이트를 찾고, FPS `Auto`는 그 값을 씁니다. 정보 줄 예: `auto 24 fps (burn-in, video 30)`.
  - 영상 클립 구간에서는 5프레임마다 반복되는 프레임까지 **화면과 똑같은 타임코드**를 표시합니다. 타임코드를 입력해 이동하면 그 타임코드가 처음 보이는 프레임으로 갑니다.
  - 인식에 쓰는 프레임을 5개에서 11개(퍼진 위치 5개 + 연속 6개)로 늘렸습니다.
- **Export 타임코드 (Stamp WAV…).** Live가 Export한 WAV에 Render Start 지점의 타임코드를 BWF 타임스탬프(`bext` TimeReference + `iXML`)로 기록합니다. 편집 프로그램과 DAW에서 영상과 같은 타임코드 위치에 샘플 단위로 정확히 놓입니다.
  - Live Set은 바꾸지 않습니다. 클립·오토메이션·트랙 싱크가 그대로이고 오디오 데이터도 바뀌지 않습니다.
  - Render Start는 파일 길이로 찾습니다 (영상 클립 / 루프 구간 / 로케이터 사이 / Set 전체). 맞는 구간이 없으면 시작 마커를 씁니다. *All Individual Tracks* Export는 파일 하나만 고르면 함께 기록됩니다.

### 검증
- Ableton Live 12.4.6 / Max 9.1.5 / macOS 15.7 (Apple Silicon)에서 단위 테스트 152개와 실제 Live 통합 테스트(번인 6종, Export 타임코드 포함)를 실행했습니다. 사용자 영상(30 fps 파일 + 24 fps 번인)에서 11/11 판독 일치, 무작위 40지점 표시 = 화면 타임코드.

### What's new (English)
- **Auto sync when the picture's timecode runs at another rate than the file** (e.g. 24 fps burn-in in a 30 fps file, every 5th frame repeated). Beta 0.2 showed `BITC rate != 30 fps` and applied nothing. Now the burn-in rate is detected among all 10 rates, FPS `Auto` follows it, and inside the video clip the display shows exactly what the picture shows, repeated frames included. Typed timecodes go to the first frame that shows them. Eleven frames are read (was five).
- **Export timecode (Stamp WAV…):** writes the timecode of the Render Start into WAV files exported from Live as a BWF timestamp (`bext` TimeReference + `iXML`), sample-accurate. The Live Set and the audio data are not changed. The Render Start is recognised from the file length (video clip, loop brace, locator pair, whole Set), else the start marker; *All Individual Tracks* exports are stamped together.

---

## Beta 0.2 — 2026-10-06

### 새 기능 · 개선
- **영상 창과 타임코드의 ±1 프레임 차이 해결.** 정지하거나 클릭한 위치가 프레임 경계에 걸리면, Live 영상 창이 이전 프레임이나 다음 프레임을 보여 주는 경우가 있었습니다. 이제 재생 헤드를 같은 프레임 안에서 아주 조금(최대 반 프레임) 옮겨, 영상 창과 SMPTE 표시가 항상 같은 프레임을 가리킵니다. 시작 마커와 Undo 기록은 바뀌지 않습니다.
- **`clip off frame grid` 표시 추가.** 영상 클립이 프레임 경계 사이에 놓여 있으면 정보 줄에 알려 줍니다. 스냅을 켜고 클립을 격자에 맞추면 사라집니다.
- 번인 타임코드 인식 도구(`bitc_reader`) 업데이트.

### 문서
- 설치 안내를 3단계로 정리하고, 다운로드한 파일의 macOS 보안 격리 해제 방법을 추가했습니다. 이 단계를 건너뛰면 번인 타임코드 인식이 동작하지 않습니다.
- 영문 안내서 추가, 라이선스 명시.

### 검증
- Ableton Live 12.4.5 / Max 9.1.5 / macOS 15.7 (Apple Silicon)에서 자동 테스트 136개 통과. 실제 Live의 영상 창을 캡처해 비교한 결과, 영상 3종 60개 지점에서 불일치가 없었습니다.

### What's new (English)
- **Fixed ±1 frame difference between Live's video window and the timecode.** When the playhead stopped exactly on a frame boundary, Live's video window could show the neighbouring frame. The playhead is now nudged within the same frame (at most half a frame) so both always match. Start marker and Undo history are untouched.
- **New `clip off frame grid` notice** when a video clip sits between frame boundaries.
- Updated burned-in timecode reader (`bitc_reader`).
- Three-step install guide, including how to remove the macOS quarantine flag (required for burned-in timecode reading). English guide and license added.

---

## DEMO — 2026-10-06 (한정 배포 · limited release)

첫 공개 버전입니다.
- Arrangement View 플로팅 SMPTE 타임코드 창 (S / M / L / XL)
- 타임코드 입력으로 이동 (Enter), 이동 후 재생 (Space), 숫자만 입력 (`01101010`)
- 영상 클립 FPS 자동 감지 (MOV / MP4 / M4V)
- 영상 번인 타임코드(BITC) 인식으로 Offset 자동 설정, 재생 중 싱크 표시 (macOS)
- 템포 오토메이션 구간에서 정확한 표시와 이동
- Set 끝 너머로 이동하면 Set 자동 연장
- 지원 프레임레이트: 23.976 / 24 / 25 / 29.97 NDF·DF / 30 / 50 / 59.94 NDF·DF / 60
