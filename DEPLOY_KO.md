# 스마트팜 대시보드 GitHub Pages 배포 가이드

이 폴더는 정적 웹페이지로 동작합니다.  
핸드폰에서 URL로 접속하려면 아래 두 가지가 모두 준비되어야 합니다.

1. 대시보드 웹페이지가 GitHub Pages `https://` 주소로 배포되어 있어야 함
2. MQTT 브로커가 외부에서 접근 가능한 `wss://` 주소로 열려 있어야 함

## 1. GitHub 저장소 만들기

GitHub에서 새 저장소를 만듭니다.

예:

```text
smartfarm-dashboard
```

## 2. 이 폴더를 GitHub에 올리기

이 폴더 안 파일들을 저장소에 올립니다.

예시 명령:

```bash
git init
git add .
git commit -m "Add smart farm dashboard"
git branch -M main
git remote add origin https://github.com/yourname/smartfarm-dashboard.git
git push -u origin main
```

이미 다른 저장소가 있다면 그 저장소에 그대로 올리면 됩니다.

## 3. GitHub Pages 켜기

GitHub 저장소에서 아래 순서로 들어갑니다.

1. `Settings`
2. `Pages`
3. `Build and deployment`
4. `Deploy from a branch` 선택
5. 브랜치 `main`, 폴더 `/ (root)` 선택
6. 저장

몇 분 뒤 예를 들어 아래 주소가 생깁니다.

```text
https://yourname.github.io/smartfarm-dashboard
```

이 프로젝트에는 `.nojekyll` 파일도 포함되어 있어서 정적 페이지로 바로 배포하면 됩니다.

## 4. Mosquitto 외부 공개

브라우저는 MQTT 브로커에 직접 붙기 때문에 브로커도 인터넷에서 보여야 합니다.

예시 구조:

- 대시보드: `https://yourname.github.io/smartfarm-dashboard`
- MQTT 브로커: `wss://mqtt.example.com:8084/`

Mosquitto 예시 설정은 [mosquitto.conf.example](/Users/zionkoo/Documents/Codex/2026-04-24/mqtt-mosquitto-mqtt-websocket-ws-wss/mosquitto.conf.example) 에 넣어두었습니다.

핵심은 아래입니다.

```conf
listener 8084
protocol websockets
```

외부 공개용이면 보통 인증서가 필요하므로 `wss://` 를 권장합니다.

## 5. 대시보드에서 브로커 주소 연결

이 대시보드는 URL 파라미터로 브로커 주소를 바꿀 수 있습니다.

예:

```text
https://yourname.github.io/smartfarm-dashboard/?host=mqtt.example.com&port=8084&path=%2F&ssl=true
```

설명:

- `host=mqtt.example.com` : MQTT 브로커 도메인
- `port=8084` : WebSocket 포트
- `path=%2F` : `/` 경로
- `ssl=true` : `wss://` 사용

만약 브로커가 `/mqtt` 경로를 쓴다면:

```text
https://yourname.github.io/smartfarm-dashboard/?host=mqtt.example.com&port=8084&path=%2Fmqtt&ssl=true
```

## 6. 꼭 확인할 것

- GitHub Pages 주소가 `https://` 로 열리는지
- MQTT 브로커가 `wss://` 로 접속 가능한지
- 서버 방화벽에서 `8084` 포트가 열려 있는지
- 도메인과 인증서가 브로커 서버와 맞는지

## 7. 테스트 방법

1. 핸드폰에서 GitHub Pages 배포 주소를 엽니다.
2. 상단 브로커 표시가 `wss://...` 로 보이는지 확인합니다.
3. 상태가 `Connected` 로 바뀌는지 확인합니다.
4. 센서값이 들어오면 숫자와 그래프가 갱신되는지 봅니다.
5. `Pump ON`, `Pump OFF` 버튼을 눌러 `farm/pump` publish가 되는지 확인합니다.

## 8. 지금 바로 필요한 값

실제 공개에 필요한 값은 보통 아래 4개입니다.

- GitHub Pages 배포 주소
- MQTT 브로커 도메인
- WebSocket 포트
- WebSocket 경로(`/` 또는 `/mqtt`)

이 값만 정해지면 핸드폰에서 바로 접속 가능한 최종 URL을 만들 수 있습니다.
