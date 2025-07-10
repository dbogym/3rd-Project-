# Vacation Management System (휴가 관리 시스템)

**Vacation Management System**은 기업의 휴가 관리를 효율적으로 도와주는 웹 기반 서비스입니다.  
휴가 신청, 승인, 통계 확인 등을 할 수 있는 구조로 설계되었습니다.

## 🛠️ 기술 스택

### Backend

- Java 21
- Spring Boot
- Spring Security + JWT 인증
- JPA (Hibernate)
- MySQL
- Redis (토큰 블랙리스트 저장용)
- Gradle

### Frontend

- React
- React Router
- Axios

## 🙋 팀원소개

<table>
  <thead>
    <tr>
      <th align="center">팀장</th>
      <th align="center">팀원</th>
      <th align="center">팀원</th>
      <th align="center">팀원</th>
      <th align="center">팀원</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/gunwoong1630">
          <img src="https://github.com/gunwoong1630.png" width="120" height="120" alt="조건웅"/><br/>
          <sub><b>조건웅</b></sub>
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/dbogym">
          <img src="https://github.com/dbogym.png" width="120" height="120" alt="고영민"/><br/>
          <sub><b>고영민</b></sub>
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/rhwlgns4386">
          <img src="https://github.com/rhwlgns4386.png" width="120" height="120" alt="고지훈"/><br/>
          <sub><b>고지훈</b></sub>
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/min0962">
          <img src="https://github.com/min0962.png" width="120" height="120" alt="민경준"/><br/>
          <sub><b>민경준</b></sub>
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/sehee123">
          <img src="https://github.com/sehee123.png" width="120" height="120" alt="황세희"/><br/>
          <sub><b>황세희</b></sub>
        </a>
      </td>
    </tr>
  </tbody>
</table>

## 🛠️ 역할 분담

| 이름     | 담당 기능                                                        |
|--------|--------------------------------------------------------------|
| **건웅** | - 전체 휴가 신청 목록 필터링 조회 <br> - 휴가 신청 내역 조회 및 관리 <br> - 코드 분류 관리 |
| **영민** | - 본인 연차 조회 <br> - 휴가 신청 내역 조회 <br> - 휴가 신청 <br> - 대기중인 휴가 관리 |
| **지훈** | - 휴가 자동 부여 <br> - 휴가 개수 관리 <br> - 월별 사용자 휴가 사용내역             |
| **경준** | - 1, 2차 결재 <br>- 회원 승인 관리 <br>                               |
| **세희** | - Spring Security, JWT를 통한 인증,인가 <br>- 전체 휴가 캘린더             |

## 시스템 구성도

[시스템 구성도.pdf](https://github.com/user-attachments/files/20358583/default.pdf)

## 🔀 Flow Chart
<details>
<img width="1681" height="847" alt="Image" src="https://github.com/user-attachments/assets/33fdbcb9-6c80-4ff3-8d71-e0c595d0877b" />
</details>

## 💽 ERD
<details>
<img src="https://github.com/user-attachments/assets/f3178333-2fd1-49ad-b9c4-f47c82989a3d"/>
</details>

## 📄 API 명세서
[\[API 명세서\] 06팀_2차 팀프로젝트.pdf](https://github.com/user-attachments/files/21157803/API.06._2.pdf)

## 📦 주요 기능

### 인증 / 인가

- 회원가입 / 로그인 / 로그아웃
- JWT 기반 인증 및 토큰 재발급
- 리프레시 토큰 쿠키 저장 + 블랙리스트 관리 (Redis)

### 휴가 기능

- 휴가 신청 / 수정 / 취소
- 관리자 승인 / 반려
- 부서별 휴가 통계
- 월별 휴가 캘린더 조회 (FullCalendar 활용)

### 관리자 기능

- 사내 부서 및 코드(직급, 휴가 타입 등) 설정
- 전체 통계 대시보드

## 🧪 테스트

- JUnit5 + Mockito 기반 단위 테스트
- 통합 테스트 (SpringBootTest)

## 💢 트러블 슈팅


### 테스트 코드 설계 시 불필요한 케이스 작성 문제

#### 🚨 문제상황
- 비즈니스 로직의 핵심이 아닌 단순 증감 테스트를 중복으로 작성
- 실제 검증이 필요한 성공/실패 조건보다 세부적인 값 변화에 집중
- 테스트 코드 유지보수 비용 증가 및 핵심 테스트 케이스 파악 어려움
  ```
    // Before - 불필요한 증감 테스트 중복
    @Test
    @DisplayName("총 휴가일수 증가 업데이트")
    void updateTotalCountIncrease() {
        VacationInfo vacationInfo = new VacationInfo(15.0, 5.0, "01", 1L);
        double newTotalCount = 20.0;
        
        VacationInfoLog log = vacationInfo.updateTotalCount(newTotalCount);
        
        assertThat(vacationInfo.getTotalCount()).isEqualTo(20.0);
        assertThat(log.getTotalCount()).isEqualTo(20.0);
    }

    @Test
    @DisplayName("총 휴가일수 감소 업데이트")
    void updateTotalCountDecrease() {
        VacationInfo vacationInfo = new VacationInfo(15.0, 5.0, "01", 1L);
        double newTotalCount = 12.0;
        
        VacationInfoLog log = vacationInfo.updateTotalCount(newTotalCount);
        
        assertThat(vacationInfo.getTotalCount()).isEqualTo(12.0);
        assertThat(log.getTotalCount()).isEqualTo(12.0);
    }
    ```
#### 🔧 해결과정
1. 테스트 목적 재정립: 업데이트 성공/실패가 핵심, 값의 증감은 부차적
2. 엣지 케이스 중심의 테스트 설계로 전환:
    ```
    // After - 핵심 비즈니스 로직 검증
    @Test
    @DisplayName("총 휴가일수 업데이트 성공")
    fun update_total_count_success() {
        val newTotalCount = 5.0
        val vacationInfo = VacationInfo(15.0, 5.0, "01", 1L)
        
        val log = vacationInfo.updateTotalCount(newTotalCount)
        
        assertThat(vacationInfo.totalCount).isEqualTo(newTotalCount)
        assertThat(log.totalCount).isEqualTo(newTotalCount)
    }

    @Test
    @DisplayName("사용일수보다 적은 총일수로 업데이트하면 예외 발생")
    fun update_total_count_failure() {
        val newTotalCount = 9.0
        val vacationInfo = VacationInfo(15.0, 10.0, "01", 1L)
        
        assertThatThrownBy { vacationInfo.updateTotalCount(newTotalCount) }
            .isInstanceOf(BadRequestException::class.java)
    }
    ```

#### ✅ 결과
- 테스트 케이스 수 감소로 유지보수 비용 절약
- 비즈니스 로직의 핵심 검증에 집중한 의미 있는 테스트 코드 작성
- 테스트 실행 시간 단축 및 실패 시 원인 파악 용이성 향상