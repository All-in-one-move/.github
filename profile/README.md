![iScreen Shoter - Preview - 250108160251](https://github.com/user-attachments/assets/9453f03d-aa96-48fb-9f56-00b3a1dc8e63)

# Pitching! (Speaking + Teaching = Pitching)

🎤 **Speak, Easy! 자신 있게 말하세요!**

**Pitching!**은 "모두가 편하게 소통하고, 자신있게 말하며 연결된 세상을 만들자." 라는 모토를 가지고 만든 플랫폼입니다.
발표를 준비하고 연습하는 과정에서 사용자의 발표 태도, 시선 처리, 음성 톤 등을 분석하여 즉각적이고 실질적인 개선 포인트를 제공합니다.
또한 채팅, 화상 & 음성통화 기능을 통해 현대인의 발표 능력 향상과 원활한 커뮤니케이션을 지원합니다.

---

## 🎯 서비스 목표

- 서비스 비전: "모두가 편하게 소통하고, 자신 있게 말할 수 있는 환경을 제공하여 발표 불안을 극복하고 소통에 대한 자신감을 향상시키자."
1. 현대인의 발표 & 소통 능력 향상에 도움이 될 수 있는 AI 발표 피드백 솔루션을 제공
2. 소통 & 협업의 도움: 원활한 커뮤니케이션을 위한 채팅, 화상 & 음성통화 기능 제공
3. 도전과 배움. 그리고 성장: 개발팀의 새로운 기술적 Challenge & 성취를 통해 지속가능한 성장 추구

---

## 📝 **Information Architecture(IA)**
![iScreen_Shoter_-_Figma_-_241222015234](https://github.com/user-attachments/assets/f9c364bb-bbb8-4ca5-a686-ebf7a0bd28c9)

### 주요기능

크게 4가지 Feature로 구성했습니다.

### 1. **화상통화 (Video Call)**
- 사용자가 서로 얼굴을 보며 의사소통할 수 있도록 지원하는 기능입니다.
- 발표 연습 또는 협업에 활용 가능하며, 실시간 피드백도 가능합니다.

### 2. **음성통화 (Voice Call)**
- 음성 기반으로 소통할 수 있는 기능으로, 발표 연습 또는 회의 진행에 적합합니다.
- 마이크 음소거 및 연결 종료와 같은 기본적인 기능도 포함되어 있습니다.

### 3. **채팅 (Chatting)**
- 텍스트 기반의 소통을 지원하며, 채널 생성, 메시지 작성, 그룹 채팅 등의 기능을 제공합니다.
- 1:1 채팅과 메시지 검색도 가능해 다양한 소통 상황을 지원합니다.

### 4. **AI 발표 피드백**
- 사용자의 발표 영상을 분석하여 다음과 같은 피드백을 제공합니다:
  - **시선처리**: 사용자가 발표 중 시선을 어떻게 관리하는지 분석.
  - **표정 평가**: 발표자의 표정이 적절한지 분석.
  - **제스처 평가**: 손동작과 같은 제스처의 적절성을 평가.
  - **음성 분석**: 억양, 발음, 목소리 크기와 속도에 대한 종합적인 피드백.

---

## **ERD**

- 사용자 정보, 메세지 정보, user-server를 연결시켜주는 user_server_membership, 서버랑 채널 정보 이렇게 5가지로 구성되어 있습니다.
  
<img width="705" alt="스크린샷_2024-12-21_오후_2 50 29" src="https://github.com/user-attachments/assets/d87a4d12-148a-4400-8b85-a07984815d50" />

---

## **Sequence Diagram**
- Login은 jwt 관련한 access, refresh token 로직을 axios interceptor를 통해 관리됩니다. (좌측 사진)
- Websocket은 토큰과 heartbeat로 서버에 연결후, WebRTC를 통해 **ICE Candidate**를 교환후 P2P 미디어 스트리밍을 시작하며, 모든 상태 변경과 채널 퇴장은 알람을 통해 실시간으로 모든 참가자에게 전달됩니다. (우측 사진)

<p align="center">
  <img src="https://github.com/user-attachments/assets/d17a65c7-9f60-40c5-8844-2dbcc9584f9b" alt="iScreen Shoter - 20250108003550090" width="46%"/>
  <img src="https://github.com/user-attachments/assets/acfc86e1-13ef-4867-8d7f-9d4b8681f6c4" alt="image 11" width="35%"/>
</p>


- Redis, Rabbitmq는 채팅을 보내면 Rabbitmq에 넣고 consumer가 꺼내어 DB저장, Redis Cache, Broadcast 작업을 진행합니다.
  
![image (10)](https://github.com/user-attachments/assets/e697f63c-ac1a-4fb8-8a91-91896bc6373f)

---

## **AI & Data Pipeline**
- **영상 처리는 OpenCV**를 사용하여 영상의 프레임을 추출한후, **컴퓨터 비전으로** 발표 중 문제 행동이 포함된 프레임을 **Vision API**에 전송하여 피드백을 받습니다.
- **음성 처리는 Whisper**를 이용해 영상의 음성을 **STT**로 변환후, 오디오 정보를 추출하고 실제 발표 음성과 비교하기 위해 **TTS**로 변환후, **발음 정확도**와 **말하기 속도**를 분석하여 피드백을 제공합니다.

![iScreen Shoter - 20250108004015679](https://github.com/user-attachments/assets/6e87dad8-feb2-4f2c-b6c1-055639010bfd)

---

## **Cloud Architecture**
- 12개의 NAT 인스턴스와 프론트를 제외한 모든 인스턴스를 프라이빗 환경에서 관리하며, 로드밸런서, Terraform, Ansible, Prometheus, CI/CD 자동화 및 알림봇으로 효율적이고 안전한 인프라 운영을 구현했습니다.

![image (12)](https://github.com/user-attachments/assets/0bca83aa-a641-46ff-b5e9-f8a74ff53be5)

---

## 💻 사용 스택

| **구분**         | **기술**                                                                                                                                                                                                                                                                                                                                                                    |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Frontend**     | <img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=React&logoColor=white"/> <img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=Vite&logoColor=white"/> <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=TypeScript&logoColor=white"/> <img src="https://img.shields.io/badge/TailwindCSS-06B6D4?style=flat-square&logo=TailwindCSS&logoColor=white"/> <img src="https://img.shields.io/badge/Zustand-181717?style=flat-square&logo=Zustand&logoColor=white"/> |
| **Backend**      | <img src="https://img.shields.io/badge/Java-007396?style=flat-square&logo=Java&logoColor=white"/> <img src="https://img.shields.io/badge/SpringWebFlux-6DB33F?style=flat-square&logo=Spring&logoColor=white"/> <img src="https://img.shields.io/badge/WebSocket-000000?style=flat-square&logo=WebSocket&logoColor=white"/> <img src="https://img.shields.io/badge/WebRTC-333333?style=flat-square&logo=WebRTC&logoColor=white"/> <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=Redis&logoColor=white"/> <img src="https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=RabbitMQ&logoColor=white"/> <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=PostgreSQL&logoColor=white"/> <img src="https://img.shields.io/badge/S3-569A31?style=flat-square&logo=AmazonS3&logoColor=white"/> <img src="https://img.shields.io/badge/OAuth2-3A2F3B?style=flat-square&logo=OAuth&logoColor=white"/> <img src="https://img.shields.io/badge/JWT-000000?style=flat-square&logo=JSONWebTokens&logoColor=white"/> |
| **AI & Data**    | <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/> <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=flat-square&logo=TensorFlow&logoColor=white"/> <img src="https://img.shields.io/badge/MediaPipe-3776AB?style=flat-square&logo=MediaPipe&logoColor=white"/> <img src="https://img.shields.io/badge/OpenAI-412991?style=flat-square&logo=OpenAI&logoColor=white"/> <img src="https://img.shields.io/badge/Whisper-000000?style=flat-square&logo=Whisper&logoColor=white"/> <img src="https://img.shields.io/badge/Librosa-FF6F00?style=flat-square&logo=Librosa&logoColor=white"/> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=OpenCV&logoColor=white"/> <img src="https://img.shields.io/badge/FFmpeg-007808?style=flat-square&logo=FFmpeg&logoColor=white"/> <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=FastAPI&logoColor=white"/> |
| **Cloud & Infra**| <img src="https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=Linux&logoColor=black"/> <img src="https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=AmazonAWS&logoColor=white"/> <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=Docker&logoColor=white"/> <img src="https://img.shields.io/badge/Jenkins-D24939?style=flat-square&logo=Jenkins&logoColor=white"/> <img src="https://img.shields.io/badge/Terraform-623CE4?style=flat-square&logo=Terraform&logoColor=white"/> <img src="https://img.shields.io/badge/Ansible-EE0000?style=flat-square&logo=Ansible&logoColor=white"/> <img src="https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=Grafana&logoColor=white"/> <img src="https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=Prometheus&logoColor=white"/> <img src="https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=Nginx&logoColor=white"/> |
| **협업툴**       | <img src="https://img.shields.io/badge/Git-F05032?style=flat-square&logo=Git&logoColor=white"/> <img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=GitHub&logoColor=white"/> <img src="https://img.shields.io/badge/GitLens-2D4457?style=flat-square&logo=GitLens&logoColor=white"/> <img src="https://img.shields.io/badge/Notion-000000?style=flat-square&logo=Notion&logoColor=white"/> <img src="https://img.shields.io/badge/Jira-0052CC?style=flat-square&logo=Jira&logoColor=white"/> <img src="https://img.shields.io/badge/Slack-4A154B?style=flat-square&logo=Slack&logoColor=white"/> |

- FE(프론트엔드): React, Vite, TypeScript, TailwindCSS, Zustand
- BE(백엔드): Java, Spring WebFlux, WebSocket, WebRTC, Redis, RabbitMQ, PostgreSQL, S3, OAuth2, JWT
- AI & Data: Python, TensorFlow, Mediapipe, OpenAI, Whisper, Librosa, OpenCV, FFmpeg, FastAPI
- Cloud & Infra: AWS (EC2, S3, RDS), Docker, Jenkins, Terraform, Ansible, Prometheus, Grafana, Nginx, Linux
- 협업툴: Git, GitHub, GitLens, Notion, Jira, Slack

---

## 📅 프로젝트 개발 기간
- 총 14주의 Sprint를 진행 (09.23 ~ 12.27)

![_kakaotech_14_2025-01-08_03 46pm](https://github.com/user-attachments/assets/507c79d2-512f-4a27-b8a1-9d21d033c17a)

| **SPRINT**      | **상세내용**                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------|
| **Week 1**       | 팀 개발 주제 Ideation & 구체화, Ground Rule, 협업 환경 구성 (Notion, Jira, Github)                             |
| **Week 2~3**     | 기획안 발표, 기능 MVP 정의, 피드백 내용 정리, 팀 개발 Process 확립 (Agile)                                     |
| **Week 4**       | Login UX/UI Design, FE 개발 (메인페이지)                                                                     |
| **Week 5**       | Login UX/UI Design, FE 개발, AI 모델 Research, Cloud 기술 STUDY                                             |
| **Week 6**       | BE 개발 시작, ERD DESIGN, AI 영상 & 음성처리 모델 TEST, Cloud ARCHITECTURE 설계                              |
| **Week 7**       | GATEWAY, WEBSOCKET, OAUTH 개발, AI Data Pipeline, 음성 모델 개발, RabbitMQ Test                              |
| **Week 8**       | OAUTH, JWT, USER 로직 개발, AI용 FastAPI 개발 & TEST, 음성처리 모델 테스트, CI/CD Test                      |
| **Week 9**       | Chatting 기능 개발, UDP Server 개발, 음성서버 모듈화, k8s Study, Docker Image 용량 줄이기                    |
| **Week 10**      | WebRTC 서버 PeerConnection, BE Exception, Front E2E Test & Refactoring, 음성서버 Router 개발, CI/CD 2차 Test (Docker, Jenkins) |
| **Week 11**      | Group (음성, 영상) 통화 동기화 문제 해결, Chatting 기능 개발, AI Server Log Level 설정 및 음성처리 서버 성능 개선, CI/CD 2차 Test & Monitoring 도구 장착 |
| **Week 12**      | WebRTC Test, Chatting Test Code 작성 (STOMP), AI 영상서버 성능개선, Monitoring 도구 장착, Jenkins             |
| **Week 13**      | RabbitMQ 테스트, WebRTC Test 마무리, Nginx, AI 영상서버 Computer Vision 적용, LoadBalancer & Jenkins         |
| **Week 14**      | 통합테스트 및 최종 발표 준비                                                                                |

---

## 📌 프로젝트 결과물

### 채팅 & 영상 & 음성 통화 기능

<p align="center">
 <img src="https://github.com/user-attachments/assets/ee53058a-6ea3-4c0b-b0e1-8d9cf0ed5220" alt="image 13" width="33%"/>
  <img src="https://github.com/user-attachments/assets/f6736b16-e0b6-41aa-9804-579696d1a3ad" alt="image 14" width="31%"/>
  <img src="https://github.com/user-attachments/assets/cdfdf883-af20-48f6-a6db-0d1f2118a0f4" alt="image 15" width="33%"/>
</p>

### 발표 & AI 피드백 기능

<p align="center">
  <img src="https://github.com/user-attachments/assets/42ac683c-cf56-4f26-ab39-8086baa63307" alt="image 16" width="33%"/>
  <img src="https://github.com/user-attachments/assets/f1036921-e7a7-4b5e-a68f-44f441bef4a3" alt="image 17" width="33%"/>
  <img src="https://github.com/user-attachments/assets/734e6c88-473a-430d-ba76-0c58cec814f7" alt="pronun feedback" width="33%"/>
</p>


---

## 👀 기대 효과

사용자들은 종합적인 피드백을 통해 발표 능력을 크게 향상시키고, 반복적인 연습과 맞춤형 조언으로 발표에 대한 자신감을 높일 수 있습니다. **표준어 사용**과 **비언어적 요소의 개선**을 통해 효과적인 커뮤니케이션 능력을 강화하며, **화상 회의와 채팅 기능**을 통해 팀 내 협업의 효율성을 증대시킵니다. 또한, **발표에 도움이 되는 칼럼**을 통해 최신 발표 트렌드와 노하우를 습득하여 지속적인 성장을 이룰 수 있습니다.

---

## **팀 소개: 일사천리**

- **Toby.Kim (김대현)** - PM & AI / AI Video 피드백 기능 & Data 처리 파이프라인 구축
- **Ella.Kim (김혜현)** - AI / AI Voice 피드백 기능 개발
- **Teddy.Kim (김영진)** - BE / Chatting & Login OAuth 개발
- **Neo.lee (이정진)** - BE / Media Chat 구축 (Video & Voice Call)
- **Selina.lee (이소민)** - Cloud / Infra & CI/CD 구축
- **David.lee (이찬영)** - Cloud / Infra & Architeture 설계

---

## **Service PR PPT**
[AIM_Pitching_Final.pdf](https://github.com/user-attachments/files/18335113/AIM_Pitching_Final.pdf)

---

## MIT License

Copyright (c) 2024 Kakaotech-Pitching Develop Team.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

