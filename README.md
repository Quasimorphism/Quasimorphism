# 박상민 (PARK SANGMIN) | Backend Developer

> **"기능 구현을 넘어, 운영 가능한 시스템을 설계하는 개발자"**  
> 실패를 예측하고, 장애의 전파를 막으며, 자원을 효율적으로 사용하는 **이유 있는 설계**를 지향합니다.

<div>
  <a href="mailto:bak575755@gmail.com"><img src="https://img.shields.io/badge/Email-bak575755%40gmail.com-d14836?style=flat-square&logo=gmail&logoColor=white" alt="Email"/></a>
  <a href="https://github.com/Quasimorphism"><img src="https://img.shields.io/badge/GitHub-Quasimorphism-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub"/></a>
</div>

---

## About Me

**[이유 있는 설계를 고민합니다]**  
프로젝트 초기에는 "기능이 동작하는 것"에 집중했습니다. 하지만 대용량 데이터 처리 중 발생한 503 에러와 DB 커넥션 고갈로 인한 서비스 마비 경험은 저를 변화시켰습니다. 
단순한 버그 수정이 아니라, "**이 기능이 운영 환경에서 어떤 자원을 점유하는가?**", "**외부 시스템의 장애가 우리 서비스로 전파되지 않으려면 어떻게 해야 하는가?**"를 먼저 고민하게 되었습니다.

**[운영 가능한 서비스를 만듭니다]**  
백엔드 개발의 핵심은 성공 케이스가 아니라 **실패 상황을 통제하는 능력**에 있다고 믿습니다. 
API 할당량 제한, 네트워크 타임아웃, 대량 데이터 처리에 따른 DB 부하 등 예측 가능한 문제들을 설계 단계에서 정의하고, 트레이드오프를 고려하여 시스템의 안정성을 확보하는 데 주력합니다.

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

#### 1. Batch Service (Data Pipeline)
대용량 데이터의 안정적 처리와 이기종 저장소(MySQL, Elasticsearch) 간의 데이터 정합성을 보장하는 배치 시스템을 설계했습니다.

*   **성능 최적화 (Performance Optimization)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/Performance_Optimization.md)]
    *   **문제:** 15만 건의 초기 데이터를 JPA로 적재 시 과도한 시간 소요 및 성능 저하.
    *   **해결:** **Tasklet + In-Memory 캐시** 전략 채택 및 JDBC Bulk Insert(`rewriteBatchedStatements`) 구현.
    *   **성과:** I/O 오버헤드를 제거하여 쓰기 성능을 극대화하고 초기 데이터 적재 시간을 획기적으로 단축.

*   **외부 연동 및 파이프라인 (External Integration)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/External_Integrations.md)]
    *   **문제:** 알라딘 API의 엄격한 호출 제한(일 5,000회)과 Ollama API의 긴 응답 시간으로 인한 병목.
    *   **해결:** **Multi-Key Rotation** 적용, 보강와 임베딩 단계 분리, 청크 기반 처리 도입. 
    *   **성과:** 긴 트랜잭션으로 인한 DB 커넥션 고갈 방지.

*   **가격 재계산 및 동기화 (Event-Driven Architecture)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/batch_jobs/DiscountRepriceJob.md)]
    *   **문제:** 할인 정책 변경 시 배치의 스케줄러가 돌기 전까지 가격 미반영 및 검색 결과 불일치 발생.
    *   **해결:** RabbitMQ 이벤트 기반 트리거 및 **CompositeWriter**를 통한 DB-Elasticsearch 동시 업데이트.
    *   **성과:** 정책 변경 즉시 판매가 갱신 및 이기종 저장소 간 실시간 데이터 정합성 보장.

*   **장애 허용 및 신뢰성 (Fault Tolerance)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/Fault_Tolerance_and_Reliability.md)]
    *   **문제:** 네트워크 등 일시적 장애로 인해 대량 배치 작업 전체가 실패하는 비효율.
    *   **해결:** 정교한 **Retry/Skip 정책** 적용 및 재시작 가능한(Restartable) **멱등성(Idempotency)** 설계.
    *   **성과:** 장애 발생 시 자동 복구 및 실패 지점부터 중복 없이 안전한 재처리 보장.

*   **리소스 최적화 (Resource Optimization)** [[👉 Tech Log](https://github.com/nhnacademy-be12-4vidia/4vidia-batch-service/blob/main/docs/wiki/batch_jobs/ContentImageCleanupJob.md)]
    *   **문제:** 에디터 작성 중단 등으로 방치된 고아(Orphan) 이미지가 스토리지 비용 낭비.
    *   **해결:** **24h/48h 안전 윈도우**를 적용한 본문 미사용 이미지 식별 및 삭제 파이프라인.
    *   **성과:** 서비스 영향도(오삭제 위험) 없이 불필요한 스토리지 비용 자동 절감.

#### 2. Backend API Development
관리자 페이지의 도서 등록 편의성과 데이터 관리를 위한 핵심 API를 개발했습니다.

*   **지능형 도서 검색 및 자동 보강 (Smart Search & Enrichment)**
    *   **문제:** 관리자가 도서 등록 시 모든 정보를 수동 입력해야 하는 번거로움과 데이터 일관성 부족.
    *   **해결:** ISBN 검색 파이프라인(DB → 외부 API → Gemini)을 구축하고, 캐시를 적용해 중복 호출 방지.
    *   **성과:** 도서 등록 편의성을 획기적으로 개선하고, 외부 API의 호출 절약.

*   **계층형 카테고리 및 도서 관리 (Category & Book Management)**
    *   **문제:** 대규모 도서 분류를 위한 복잡한 계층 구조 설계와 조회 성능 최적화 필요.
    *   **해결:** **한국십진분류법(KDC)** 표준을 채택하여 데이터 정합성을 확보하고, Path 기반 트리 구조 및 Self-Referencing 엔티티로 계층형 API 구현.
    *   **성과:** 표준화된 분류 체계 구축 및 효율적인 인덱싱을 통해 대량 데이터 조회 성능 확보.

*   **할인 정책 및 이벤트 연동 (Discount Policy & Event Integration)**
    *   **문제:** 할인 정책 변경 시 수만 권의 도서 가격을 동기적으로 재계산하면 **API 응답 지연 및 시스템 과부하** 발생.
    *   **해결:** 정책 관리 API 개발 및 변경 이벤트를 **RabbitMQ로 발행**하여, 무거운 재계산 로직을 배치 서비스가 비동기로 처리하도록 이관.
    *   **성과:** API 서버의 부하를 제거하여 **사용자 응답 속도(Latency)를 저해하지 않으면서** 대규모 가격 갱신을 안정적으로 수행.

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
