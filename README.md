# 박상민 (PARK SANGMIN)

<div>
  <a href="mailto:bak575755@gmail.com"><img src="https://img.shields.io/badge/Email-bak575755%40gmail.com-d14836?style=flat-square&logo=gmail&logoColor=white" alt="Email"/></a>
  <a href="https://github.com/Quasimorphism"><img src="https://img.shields.io/badge/GitHub-Quasimorphism-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub"/></a>
</div>

---

## Tech Stack

| Category | Skills |
| --- | --- |
| **Language** | ![Java](https://img.shields.io/badge/Java_21-ED8B00?style=flat-square&logo=openjdk&logoColor=white) |
| **Framework** | ![Spring Boot](https://img.shields.io/badge/Spring_Boot_3.x-6DB33F?style=flat-square&logo=springboot&logoColor=white) ![Spring Batch](https://img.shields.io/badge/Spring_Batch-6DB33F?style=flat-square&logo=spring&logoColor=white) ![Spring Data JPA](https://img.shields.io/badge/Spring_Data_JPA-6DB33F?style=flat-square&logo=spring&logoColor=white) |
| **Database** | ![MySQL](https://img.shields.io/badge/MySQL_8.0-4479A1?style=flat-square&logo=mysql&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) |
| **Tools** | ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white) ![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white) |

---

## Key Project: 4VIDIA Online Bookstore

> **MSA 기반의 온라인 서점 팀 프로젝트**  
> 2025.11 ~ 2025.12 (8명 / Backend Developer)

### 👨‍💻 My Contributions
저는 **도서 도메인의 관리자 API**와 데이터 처리를 위한 **Batch System**의 설계를 담당했습니다.

#### 1. [[Batch Service (Data Pipeline)](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service)]
배치 처리가 요구되는 데이터의 처리와 이기종 저장소(MySQL, Elasticsearch) 간의 데이터 반영을 위한 파이프라인을 설계했습니다.

*   **성능 최적화 (Performance Optimization)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/Performance_Optimization.md)]
    *   **문제:** 15만 건의 도서 데이터 적재 시 JPA의 N+1 Insert 발생으로 인한 속도 저하.
    *   **해결:** 참조 데이터를 메모리에 올려 반복적인 DB 조회를 제거하고, JDBC Bulk Insert를 적용.
    *   **성과:** 초기 데이터 적재 속도 개선.

*   **AI 임베딩 파이프라인 최적화 (AI Embedding Optimization)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/External_Integrations.md)]
    *   **문제:** 외부 API(Ollama) 호출 대기 시간 동안 DB Connection 및 Row Lock을 점유하는 현상 발생.
    *   **해결:** 데이터 보강과 임베딩 생성을 별도 Step으로 분리하여 트랜잭션을 나누고, Fetch Join과 JDBC Batch Update 적용.
    *   **성과:** 도서 정보의 벡터 데이터를 생성하여 Elasticsearch에 적재.

*   **가격 재계산 및 검색 반영 (Event-Driven Architecture)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/batch_jobs/DiscountRepriceJob.md)]
    *   **문제:** 할인 정책 변경 시 다수의 도서 가격 갱신 필요.
    *   **해결:** RabbitMQ 이벤트를 트리거로 배치를 실행하고, CompositeWriter를 통해 변경된 가격을 DB와 검색 인덱스(ES)에 반영.
    *   **성과:** 가격 정책 변경 사항을 비동기로 저장소에 일괄 반영.

*   **장애 허용 및 신뢰성 (Fault Tolerance)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/Fault_Tolerance_and_Reliability.md)]
    *   **문제:** 외부 API 호출이나 네트워크 상태에 따른 배치 작업 실패 가능성.
    *   **해결:** Step별 Retry/Skip 정책을 설정하고, 재시작 시 중복 처리가 없도록 설계.
    *   **성과:** 예외 상황 발생 시 자동 복구 시도 및 재처리 보장.

#### 2. [[Backend API Development](https://github.com/nhnacademy-be12-4vidia/4vidia-bookstore-service)]
관리자용 도서 등록 편의성과 데이터 관리를 위한 API를 개발했습니다.

*   **Gemini 기반 도서 정보 보강 (AI Enrichment)**
    *   **문제:** 도서 등록 시 상세 정보를 수동으로 입력해야 하는 번거로움.
    *   **해결:** ISBN 검색 시 외부 API와 Gemini API를 연동하여 설명, 목차 등 부족한 정보를 자동으로 보강하도록 구현.
    *   **성과:** 관리자의 도서 등록 작업 효율 개선.

*   **계층형 카테고리 및 도서 관리 (Category & Book Management)**
    *   **문제:** 대규모 도서 분류를 위한 계층 구조 설계 및 조회 성능 필요.
    *   **해결:** 한국십진분류법(KDC) 표준을 기반으로 Path 기반 트리 구조 및 Self-Referencing 엔티티 설계.
    *   **성과:** 표준화된 분류 체계 구축 및 계층형 데이터 조회 기능 구현.

*   **할인 정책 및 이벤트 연동 (Discount Policy & Event Integration)**
    *   **문제:** 할인 정책 변경 시 발생하는 가격 재계산 작업이 API 서버 부하 유발.
    *   **해결:** 정책 변경 이벤트를 발행하여 무거운 계산 로직을 배치 서버가 비동기로 처리하도록 분리.
    *   **성과:** API 서버의 트랜잭션 부하를 제거하고 사용자 응답 속도 유지.

---

## 📜 Experience

### NHN SW Academy | Java Backend
*2025.07 ~ 2025.12*
*   CS(네트워크, DB) 기초 및 Java, Spring Framework 심화 학습
*   **GitHub Project**를 활용한 협업 및 애자일(Scrum) 프로세스 경험

---

## 🎓 Education
*   **조선대학교 컴퓨터공학전공** 
    *   2020.03 ~ 2026.02 (졸업 예정)
    *   성적: 4.18 / 4.5
*   **자격증** 
    *   TOEIC 820
    *   TOPCIT 3수준