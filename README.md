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
배치 처리가 요구되는 데이터의 안정적 처리와 이기종 저장소(MySQL, Elasticsearch) 간의 데이터 정합성을 보장하는 파이프라인을 설계했습니다.

*   **성능 최적화 (Performance Optimization)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/Performance_Optimization.md)]
    *   **문제:** 15만 건의 도서 데이터 적재 시 JPA의 N+1 Insert 발생 및 과도한 시간 소요.
    *   **해결:** **InMemoryReferenceDataCache**를 구현하여 반복 조회 비용을 제거하고, JDBC Bulk Insert(`rewriteBatchedStatements`)를 적용.
    *   **성과:** I/O 오버헤드를 줄여 초기 데이터 적재 속도 개선.

*   **외부 연동 및 파이프라인 (External Integration)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/External_Integrations.md)]
    *   **문제:** 알라딘 API의 일일 호출 제한(5,000회) 및 외부 API 응답 지연.
    *   **해결:** **Multi-Key Rotation** 전략으로 호출 한도를 확장하고, 처리 속도를 조절(Throttling)하여 시스템 안정성 확보.
    *   **성과:** 외부 API 호출 제한 준수 및 긴 트랜잭션으로 인한 DB 부하 방지.

*   **가격 재계산 및 검색 반영 (Event-Driven Architecture)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/batch_jobs/DiscountRepriceJob.md)]
    *   **문제:** 할인 정책 변경 시 다수의 도서 가격 변경 필요.
    *   **해결:** RabbitMQ 이벤트를 트리거로 배치를 실행하고, **CompositeWriter**를 활용하여 변경된 가격을 DB와 검색 인덱스(ES)에 반영.
    *   **성과:** 정책 변경 사항을 비동기로 안전하게 반영하여 데이터 일관성 유지.

*   **장애 허용 및 신뢰성 (Fault Tolerance)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/Fault_Tolerance_and_Reliability.md)]
    *   **문제:** 네트워크 등 일시적 장애로 인한 배치 작업 실패.
    *   **해결:** Step별 **Retry/Skip 정책**을 적용하고, 재시작 시 중복 처리가 없도록 멱등성(Idempotency) 보장 설계.
    *   **성과:** 장애 발생 시 자동 복구 시도 및 안전한 재처리 보장.

*   **리소스 최적화 (Resource Optimization)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/batch_jobs/ContentImageCleanupJob.md)]
    *   **문제:** 에디터 작성 중단 등으로 방치된 고아(Orphan) 이미지가 스토리지 공간 점유.
    *   **해결:** 생성 후 24시간이 지난 미사용 이미지를 식별하여 MinIO에서 삭제하는 파이프라인 구축.
    *   **성과:** 불필요한 스토리지 비용 절감.

#### 2. [[Backend API Development](https://github.com/nhnacademy-be12-4vidia/4vidia-bookstore-service)]
관리자 페이지의 도서 등록 편의성과 데이터 관리를 위한 핵심 API를 개발했습니다.

*   **Gemini 기반 도서 정보 보강 (AI Enrichment)**
    *   **문제:** 관리자가 도서 등록 시 모든 정보를 수동 입력해야 하는 번거로움.
    *   **해결:** ISBN 검색 파이프라인(DB → 외부 API → Gemini API)을 설계해 부족한 정보를 자동으로 생성 및 보강.
    *   **성과:** 관리자의 도서 등록 작업 효율성 향상.

*   **계층형 카테고리 및 도서 관리 (Category & Book Management)**
    *   **문제:** 대규모 도서 분류를 위한 계층 구조 설계 필요.
    *   **해결:** **한국십진분류법(KDC)** 표준을 기반으로 Path 기반 트리 구조 및 Self-Referencing 엔티티 설계.
    *   **성과:** 표준화된 분류 체계 구축 및 계층형 데이터 조회 기능 구현.

*   **할인 정책 및 이벤트 연동 (Discount Policy & Event Integration)**
    *   **문제:** 할인 정책 변경 시 발생하는 대량의 가격 변경 작업이 API 서버에 부하 유발.
    *   **해결:** 정책 변경 이벤트를 **RabbitMQ로 발행**하여 배치 서비스가 비동기로 가격을 재계산하도록 위임.
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