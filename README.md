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
CSV 기반의 대량 데이터 적재 및 정책 기반 가격 관리 파이프라인을 구축했습니다.

*   **대량 데이터 적재 성능 최적화:**
    *   15만 건의 초기 데이터 적재 시, `JdbcTemplate` 기반의 **Bulk Insert**와 `INSERT IGNORE`를 활용하여 중복 오류를 제어하고 쓰기 성능을 높였습니다.
*   **데이터 보강 및 임베딩 처리:**
    *   초기 데이터의 부족한 정보를 Aladin OpenAPI를 통해 보강하고, 검색을 위한 임베딩 값을 받아 저장하는 배치 파이프라인을 설계했습니다.
    *   외부 API 호출 실패에 대비해 `Spring Retry`를 도입, 재시도 로직을 구현하여 안정성을 확보했습니다.
*   **하이브리드 가격 재계산 구조 (Event + Scheduler):** 
    *   **이벤트 기반:** 할인 정책이 생성/수정되는 즉시 반영되도록 RabbitMQ를 연동하여 지연 시간을 해소했습니다.
    *   **내부 스케줄러 기반:** 할인 정책의 **시작일과 종료일**에 맞춰 판매가가 자동 갱신되도록 서비스 내부의 스케줄러가 매일 자정 배치를 실행하도록 설계했습니다.
*   **WYSIWYG 리소스 정리:** 
    *   에디터 미리보기 시 업로드되었으나 도서 설명에는 포함되지 않은 고아 이미지(Orphan Image)를 삭제하는 배치를 통해 스토리지 비용을 최적화했습니다.

#### 2. Backend API Development
관리자 페이지의 도서 등록 편의성과 데이터 관리를 위한 핵심 API를 개발했습니다.

*   **스마트 도서 검색 및 보강 (`/admin/books/search`):** 
    *   관리자의 도서 등록 수고를 줄이기 위해 **ISBN 기반의 지능형 검색 흐름**을 설계했습니다.
    *   **Flow:** DB 조회 → (없으면) 외부 API 조회 → **Gemini RAG (AI)**를 활용하여 도서 정보(설명, 태그 등) 자동 생성 및 보강 → 등록 폼 프리필(Pre-fill).
*   **관리자 도서 관리 (`/admin/books`):** 도서 CRUD 및 메타데이터(작가/태그) 매핑 관리.
*   **카테고리 시스템 (`/categories`):** 계층형 카테고리 구조 설계 및 관리 기능 구현.
*   **할인 정책 관리 (`/admin/books/discount-policies`):** 
    *   도서정가제 및 자체 할인 정책 API 개발.
    *   정책 변경 시 RabbitMQ 메시지 발행을 통한 배치 연동 로직 구현.

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
