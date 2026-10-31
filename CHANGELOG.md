# 변경 내역 · Changelog

## 1.0 — 2026-11-01

정식 버전입니다. Beta 0.4 이후 사용자 파일의 안전과 동작 오류를 중심으로 코드를 점검하고 고쳤습니다.

### Export 타임코드 (Stamp WAV…)
- **다른 Export의 파일을 덮어쓰지 않습니다.** 이전에는 같은 폴더에서 10분 안에 만들어진 같은 길이의 WAV를 모두 같은 Export로 보고 기록했습니다. 그래서 다른 Export에 이미 맞게 기록된 타임코드를 덮어쓸 수 있었습니다. 이제는 고른 파일과 1분 안에 함께 저장되었고, 아직 기록 전이거나 같은 값이 기록된 파일만 함께 기록합니다. 기록한 파일 이름은 Max Console에 남습니다.
- **외장 드라이브의 파일도 기록됩니다.** 이전에는 외장 드라이브의 파일을 고르면 실패했습니다. 시동 디스크에 같은 경로의 파일이 있으면 그 파일에 기록되기도 했습니다. **Choose…**로 외장 드라이브의 영상을 고를 때도 같은 문제가 있어 함께 고쳤습니다.
- **Render Start:** 시간 선택으로 Export하면 시작 마커를 씁니다(시간 선택이 시작 마커를 그 시작으로 옮기기 때문입니다). 파일 길이가 Set 전체 길이와 같으면 0에서 시작합니다. 파일 길이가 루프 구간 등과 우연히 같을 때도 그 구간으로 잘못 기록하지 않고, 상태 줄에 알려 줍니다. 그래서 "시작 마커를 옮기고 다시 기록"하는 해결법이 언제나 통합니다.
- 디스크가 거의 찬 경우처럼 쓰기가 중간에 짧게 끝나도 오디오에 빈 구간이 생기지 않습니다. 새 파일의 오디오를 원본과 바이트 단위로 비교한 뒤에만 원본을 바꿉니다.
- 읽기 전용 파일은 바꾸지 않습니다. 중단된 기록이 남긴 임시 파일을 정리합니다. 다시 기록하면 bext 설명도 새 타임코드로 바뀝니다.
- 타임스탬프는 해당 타임코드 프레임 안의 첫 샘플입니다. 이전에는 가장 가까운 샘플을 써서, 29.97·59.94 fps에서 일부 프레임이 한 프레임 앞으로 읽힐 수 있었습니다.

### 이동과 표시
- 템포 오토메이션 구간에서 이동하는 중에 재생을 누르면, 그 뒤에는 Enter만 눌러도 재생이 시작되던 문제를 고쳤습니다.
- Set 끝 너머로 이동하다 Set을 늘리지 못하면 표시가 목표 위치에 멈춰 있던 문제를 고쳤습니다.
- 자정(00:00:00:00)을 넘는 타임코드를 입력할 수 있습니다. 예를 들어 번인이 00:00:00:00에서 시작하는 영상을 앞에 여유를 두고 놓은 경우입니다.
- Offset의 프레임 값이 FPS 범위를 넘으면 타임코드 입력을 거부합니다. 이전에는 Offset을 0으로 보고 1시간 떨어진 곳으로 이동한 뒤 Set을 늘렸습니다.
- 저장한 Set을 다시 열 때 손으로 맞춘 Offset을 번인 값으로 덮어쓰지 않습니다. 번인 값으로 바꾸려면 Rescan을 누르세요.
- FPS 메뉴를 `Auto (video)`로 바꾸면 번인 결과가 바로 적용됩니다.
- Set을 열 때 영상 분석 요청이 사라져 FPS가 24 fps 기본값에 머무르던 경우를 막았습니다.
- 정지한 재생 헤드가 영상 클립의 프레임 경계에 정확히 놓이면(예: 120 BPM에서 박 위치를 클릭), Live 영상 창과 같은 프레임을 표시합니다. Beta 0.4는 이때 항상 이전 프레임을 표시해서, 30 fps 영상(24 fps로 편집해 30 fps로 내보낸 영상 포함) 등에서 1프레임 차이가 날 수 있었습니다. Live가 어느 프레임을 보여 줄지는 영상 파일의 타이밍 정보로 계산합니다.

### 번인 타임코드
- 인식 도구가 실행되지 못하면 이유를 보여 줍니다: `reader blocked (xattr)`(macOS가 막음), `reader missing`, `reader not executable`, `reader timed out`.
- 정보 줄을 짧게 바꿔 결과가 잘리지 않게 했습니다. 예: `auto 24 fps (file 30) | BITC 01:04:19:10 ok`.

### 기타
- 디바이스 패널 아래쪽에 버전(`SMPTE Display 1.0`)이 표시됩니다.
- 설치된 파일을 바꿔도 실행 중인 디바이스가 초기화되지 않습니다.

### 라이선스와 문서
- 라이선스(LICENSE)를 1.0에 맞게 고쳤습니다. 함께 배포된 버전에만 적용되고(이미 무료로 배포된 버전은 그 조건 그대로), "beta" 문구를 뺐습니다. 고의·중대한 과실로 인한 책임과 법령상 제한할 수 없는 권리는 제한하지 않으며, 소비자는 자기 나라 강행규정의 보호를 받습니다. 같은 작업의 공동 작업자에게 Live Set을 공유하는 것과 개인적 사용을 위한 수정을 허용했습니다. 영어본과 한국어본은 같은 효력을 가집니다.
- 상표 고지를 보강했습니다(SMPTE, Apple 포함, 제휴·보증 없음). 디바이스가 인터넷에 연결하지 않고 개인정보를 수집하지 않는다는 안내를 넣었습니다.
- 저장소 이름을 `SMPTE-Display`로 바꿨습니다. 예전 주소(`M4L-SMPTE-Display-For-Ableton-Live`)는 새 주소로 자동 연결됩니다.

### 검증
- 단위 테스트 176개 통과. 이번에 고친 결함마다 테스트를 먼저 써서, 수정 전 코드에서 실패하는 것을 확인했습니다.
- Stamp WAV 결과를 ffprobe, MediaInfo, BWF MetaEdit로 읽어 10개 경우(44.1~96 kHz, DF 포함)에서 모두 정확한 값을 확인했고, EBU Tech 3285 형식 오류는 0이었습니다.
- 외장 볼륨(HFS+ 디스크 이미지)에서 Stamp WAV를 실측했습니다.
- 실제 Ableton Live 12.4.6 · Max 9.1.5(macOS 15.7, Apple Silicon)에서 자동 통합 테스트를 실행했습니다. 자동으로 판정하는 항목은 정지 위치에서 Live 영상 창과 표시를 비교하는 2개를 빼고 모두 통과했고, Max Console 에러는 0건이었습니다. 탐색 70회(프레임 경계 30회 포함)에서는 Live 영상 창과 표시가 모두 같았습니다. 정지 60회 중 12회는 Live 영상 창이 1프레임 앞이었고, 모두 프레임이 끝나기 전 약 15 ms 안에서 정지한 경우였습니다(아래 알려진 한계).

### 알려진 한계
- 프레임이 끝나기 전 약 15 ms 안에서 정지하면, Live 영상 창이 다음 프레임을 보여 줄 때가 있습니다(Beta 0.4와 같음). Live 영상 창의 동작이라 디바이스에서 맞출 수 없습니다.

### What's new in 1.0 (English)
- **Stamp WAV never rewrites files from another export.** Files are stamped together only if they were saved within one minute of the picked file and are unstamped or carry the same stamp. The names of stamped files go to the Max Console.
- **Files on external drives** can be stamped (they used to fail, or a file with the same path on the startup disk was stamped instead). Choose… works on external drives too.
- **Render Start** is the start marker for a time-selection export and 0 for a whole-Set export. A coincidental length match is only mentioned, so moving the start marker and stamping again always works.
- A short write can no longer leave a gap in the audio: the new file's audio is byte-compared with the original before it replaces it. Read-only files are left alone. The timestamp is the first sample inside the labelled frame.
- Fixed: surprise playback after an Enter-only locate, a display frozen after a failed Set extension, timecodes past midnight refused, an invalid Offset treated as 0, a hand-set Offset overwritten when a Set is reopened, FPS Auto not applying a burn-in result until Rescan.
- Stopped exactly on a frame boundary in a video clip (e.g. a beat at 120 BPM), the display shows the same frame as Live's video window. Beta 0.4 always showed the previous frame there, which could be one frame off in 30 fps files, including 24 fps edits exported at 30 fps. Which frame Live shows is worked out from the movie file's timing.
- Burned-in timecode errors now say why (`reader blocked (xattr)`, `reader missing`, …). The panel shows the version.
- The LICENSE now covers the version it ships with, drops "beta", keeps liability for intent and gross negligence and any non-excludable rights, preserves consumers' mandatory protections, allows sharing a Live Set with collaborators on the same production and private modification, and has equally authoritative English and Korean texts. The trademark notice now covers SMPTE and Apple. The repository is now `SMPTE-Display`; the old address redirects.
- 176 unit tests pass. Stamped files read back exactly with ffprobe, MediaInfo and BWF MetaEdit.
- Tested in Ableton Live 12.4.6 with Max 9.1.5 (macOS 15.7, Apple Silicon): every automated check passes except the two that compare the display with Live's video window at stop positions, and there were no Max Console errors. All 70 locates (30 of them exactly on a frame boundary) showed the same frame as Live's video window; in 12 of 60 stops the video window was one frame ahead, each time within about the last 15 ms of a frame.
- Known limitation: stopping within about the last 15 ms of a frame can leave Live's video window one frame ahead (as in Beta 0.4). That is Live's video window and the device cannot correct it.

---

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
