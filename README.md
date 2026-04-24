# Smart Farm MQTT Dashboard

GitHub Pages로 배포할 수 있는 정적 스마트팜 대시보드입니다.

## 포함 파일

- `index.html`: 대시보드 본체
- `mosquitto.conf.example`: 외부 `wss://` 브로커용 예시 설정
- `DEPLOY_KO.md`: GitHub Pages 배포 가이드

## 핵심 구조

- 웹페이지: GitHub Pages
- MQTT 브로커: 외부에서 접근 가능한 Mosquitto WebSocket 서버

예:

```text
https://yourname.github.io/smartfarm-dashboard/?host=mqtt.example.com&port=8084&path=%2F&ssl=true
```
