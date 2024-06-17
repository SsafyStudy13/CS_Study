블로그 링크 : https://no-delay.tistory.com/91

## 2. 데드락이란 무엇인지, 방지할 수 있는 방법에는 어떤 것들이 있는지 설명해주세요.


![image](https://github.com/SsafyStudy13/CS_Study/assets/57094856/00dec57f-76b4-4409-ab4b-38ef544bd920)

### **DeadLock이란?**

두 개이상의 프로세스 혹은 스레드가 서로가 가진 리소스를 기다리는 상태  
교착 상태 → 무한 대기

교착 상태는 아래의 사진을 보면 이해가 쉬울거다. 일을 하고 싶은데 할 수 없는 것이다...

![image](https://github.com/SsafyStudy13/CS_Study/assets/57094856/ca579080-4781-4b07-a5f2-2aa87aac7925)

### **Deadlock을 발생시키는 4가지 조건**

-   Mutual exclusion(상호 배제)  
    \- 리소스(critial session or lock ,cpu, 메모리, ssd 등)를 공유해서 사용할 수 없음
-   Hold and wait(점유와 대기)  
    \- 프로세스가 이미 하나 이상의 리소스를 취득한(hold)한 상태에서 다른 프로세스가 사용하고 있는 리소스를 추가로 기다림(wait)
-   No preemption(비선점)  
    \- 리소스 반환은 오직 그 리소스를 취득한 프로세스만 할 수 있음  
    \- 지난 발표에서 다른 사람이 lock을 해제하는 경우는 안됨
-   Circular wait(환형 대기)  
    \- 프로세스들이 순환 형태로 서로의 리소스를 기다림

**💡그러면 이걸 해결하는 방법은 뭐가 있을까?**

### **해결 방법**

#### 1\. Deadlock prevention(데드락 방지)

시스템 레벨에서 4가지 조건 중 하나가 충족되지 않게 디자인  
  
**Mutual exclusion** → 불가능  
**Hold and wait**  
\- 사용할 리소스들을 모두 획득한 뒤에 시작  
\- 리소스를 전혀 가지지 않은 상태에서만 리소스 요청  
\- but 리소스 사용 효율이 떨어질 수 있음 → 놀고있는 리소스가 있을 수 있기 때문  
\- but 사용할 리소스가 수요가 많다면 계속 기다려야됨 → stravation(기아 상태)  
**No preemption**  
\- 추가적인 리소스를 기다려야 한다면 이미 획득한 리소스를 다른 프로세스가 선점 가능하도록 함 → 양보  
**Circular wait**  
\- 모든 리소스에 순서 체계를 부여해서 오름차순으로 리소스를 요청(가장 많이 사용됨)

#### 2\. Deadlock avoidance(데드락 회피)

실행 환경에서 추가적인 정보를 활용해서 데드락이 발생할 것 같은 상황을 회피하는 것

**Banker algorithm**  
리소스 요청을 허락해줬을 때 데드락이 발생할 가능성이 있으면 리소스를 할당해도 안전할 때까지 계속 요청을 거절하는 알고리즘

참고 : [https://wpaud16.tistory.com/271](https://wpaud16.tistory.com/271)

![image](https://github.com/SsafyStudy13/CS_Study/assets/57094856/157530ae-fb61-47a0-b631-62dc7881dfad)

#### 3\. Deadlock detection & recovery(데드락 감지와 복구)

**감지 방법**

-   일정 시간(가벼운 데드락 검출)
    -   일정 시간동안 프로세스의 작업이 진행되지 않으면 데드락에 빠진걸로 간주
-   자원 할당 그래프 사용(무거운 데드락 검출)
    -   프로세스가 어떤 자원을 사용 중이고, 어떤 자원을 "기다리는지" 방향성 있는 그래프로 표시한 것
    -   프로세스는 원, 자원을 사각형 으로 표시

**복구 전략**

-   프로세스 종료(진행 작업을 잃을 수 있으므로 리스크 큼)
    -   모든 프로세스 종료
    -   순차적으로(우선 순위기준) 프로세스 종료 → 복구될 때까지
-   리소스의 일시적인 선점을 허용

### 4. Deadlock Ignorance(데드락 무시)

deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않음  
  
\- 데드락이 매우 드물게 발생하므로 조치를 취하지 않음  
\- 데드락이 발생한 경우 사용자가 직접 다루도록 함  
\- Unix, window등 현재 대부분의 시스템이 이러한 방식을 채택하고 있음 → 데드락을 처리하는 오버헤드가 크기 때문

노션 링크

[https://spurious-astronomy-3a1.notion.site/Deadlock-beb69b2e314b4bc1a8e180715044a150?pvs=4](https://spurious-astronomy-3a1.notion.site/Deadlock-beb69b2e314b4bc1a8e180715044a150?pvs=4)
