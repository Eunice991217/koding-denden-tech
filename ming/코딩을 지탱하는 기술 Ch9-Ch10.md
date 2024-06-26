# 9장. 컨테이너와 문자열

1. 다양한 종류의 컨테이너

컨테이너란? 

- 무언가를 넣는 상자

ex) C언어에서는 배열, Python 에서는 튜플

1. 왜 다양한 컨테이너가 존재할까?
- 각각의 컨테이너는 장단점이 있기 때문에

***배열***

- 값을 순서대로 넣음
- 중간에 값을 넣으려면 다 복사해야해서 속도가 느림
- 항목 삽입시 계산량이 O(n)

***연결 리스트*** 

- 값을 순서대로 안넣음 (다음 값을 저장)
- 중간에 값을 넣으려고 해도 처리 시간에 변화가 없음

⇒ 요소 개수가 많으면 연결 리스트가 적합

- 항목 삽입시 계산량이 O(1)
1. 사전, 해쉬, 연상 배열
- 배열은 **정수와 값 대응**, 사전은 **문자열과 값 대응 (key-value)**
- 방법? → 해쉬 테이블, 트리
    - 해쉬 테이블 : 문자열을 인수로 받아서 정수를 반환
    - 트리 : 뿌리(root)를 기준으로 왼쪽 자식과 오른쪽 자식에 두 개의 화살표로 연결

## 만능 컨테이너란 없다.

어떤 것을 사용하면 좋을까? 라는 의문을 품을 수 있지만, 사용하는 목적에 따라 선택해야 하는 컨테이너도 달라진다. 자신의 상황에 맞게 적합한 균형을 찾는게 중요하다! 

1. 문자란?
- ‘이것은 문자라고 부르자’고 정한 기호 집합
- 이건 문화에 따라 다름
- 발전 과정
    
    : 모스부호 → 보 코드 → EDSAC → ASCII와 EBCDID → Unicode
    
1. 문자열이란?
- 문자가 정해져 있는 것.
- 표현하는 방법이 제각각 (언어별로 다름)
- C언어에서는 NULL 문자가 문자열의 끝을 표현함 (애는 주소만 기록함)
- 반면, Pascal은 문자열 제일 앞부분에 문자열 길이 기록해둠.

## 정리

- 어떻게 메모리에 저장하느냐에 따라 성능 차이가 난다.
- 일방적으로 뛰어난 건 없고, 각각 장단점이 존재
- 문자열과 값을 대응하는 것도 각각 구현 방법에 따라 장단점이 다름 (해쉬 테이블이 가장 많이 사용)
- 문자열의 여러가지 차이 (무엇이 문자인지, 어떻게 문자를 비트열로 표현하는지, 어떤 정보를 어떤 메모리에 저장하는지) ⇒ 문자 집합, 문자 부호화, 문자열 구현

# 10장. 병행 처리

1. 병행 처리란?
- 동시에 수행하는 작업. 즉, 복수의 처리를 시간축 상에 오버랩해서 실행한다.
- 편리한 병행 처리를 위해 프로세스나 스레드 등의 개념이 만들어진 것
- 하지만, 병행 처리가 원이이 되어 새로운 문제를 일으케기 되었음
- 이에 락이나, 파이버 등의 개념이 나옴
1. 잘게 분할해서 실행한다.
- 병행 처리의 가장 중요한 개념
- 사람의 눈으로 보면 프로그램이 계속 동작하고 있는것처럼 보이지만, 실제로는 잘게 분할해서 실행되고 있는 것
1. 처리를 변경하는 2가지 방법

**1) 협력적 멀티태스크**

- 타이밍이 좋은 시점에서 교대
- ‘교대해도 좋아’ 라고 말하지않으면 다른 처리를 계속 기다려야함…ㅠㅠ (독점해버릴 수도 있음)

**2) 선점적 멀티태스크**

- 일정 시간에 교대
- 개별 프로그램과 다른 프로그램(태스크 스캐줄러)가 존재
- 일정 시간마다 지금 실행되고 있는 처리를 강제적으로 중단시켜 다른 프로그램이 실행할 수 있게.
1. 경합 상태 방지법
- 선점적 멀티태크스는 사용자한테는 매우 좋음
- 근데 프로그램 만드는 사람은 언제 ‘교대해!’ 라는 명령이 떨어질지 모르는 상황에서 제대로 동작하는 프로그램 만드는게 어려움 ㅠㅠ

**경합상태 (스레드 세이프)**

- 명령어들이 뒤섞여 수행되면 수행 결과를 예측할 수 없는 상태

ex) 은행 상황 

- 경합 상태의 3가지 조건

1) 2가지 처리가 변수를 공유하고 있다.

2) 적어도 하나의 처리가 그 변수를 변경한다.

3) 한쪽 처리가 한 단락 마무리 되기 전에, 다른 한쪽의 처리가 끼어들 가능성이 있다.

⇒ 3가지 조건 중 하나라도 제거할 수 있으면 병행 실행 시에도 안정된 프로그램을 만들 수 있다.

*공유하지 않는다*

- 프로세스와 엑터 모델
- 처음부터 아무것도 공유하지 않으면 경합 상태를 신경 쓸 필요가 없음.
- 프로세스
    - 하나의 프로세스 안에서 병행해서 실행되는 처리는 하나
    - 병행 실행되는 처리는 메모리를 공유하지 않는다.
- 엑터 모델
    - 정보를 교환하는 방법
    - 메세지를 보낸다.
    - A가 일하고 있으면 B가 A 서류 상자에 서류 넣고 가~
    - B가 준 일을 A가 하면 A는 B한테 했다고 말만하면 됨~

*변경하지 않는다.*

- 메모리를 공유해도 변경하지 않으면 문제 없다.
- 일부 변수를 변경할 수 없게 한다.

cf. Java에서는 Immutable 패턴이 자주 사용 

*끼어들지 않는다.*

- 협력적 스레드 사용
- 끼어들면 곤란해지는 처리에 표식을 붙임 (0이면 끼어들지마~)

⇒ 여러가지 기법이 있음

- 락, 뮤텍스, 세마포어 등
- 핵심 개념은 화장실 문의 “사용중” 표식과 같은 것.
- 처리 흐름의 일부만 양보하는 구조.

`여기서 생기는 문제점은?`

- 교착 상태가 방샐함 → A가 x, y 순서로 락 걸어두고, B가 y, x 순서로 락 걸어두면 서로가 상대방 락 풀리는거 기다려야함.
- 합성할 수 없음 → x 에서 y 로 데이터를 옮길 때, 옮기고 있는 그 어중간한 상태에서 변경이 되어버리면 원하는 결과가 안나옴.
    
    : x나 y 를 읽고 쓰는 모든 모든 코드를 synchronized 블록 등으로 감싸야함. 
    
    → 근데 이건 불편함. ‘편하게 하자’ 라는 목적에 안맞음
    

⇒ 그래서 나온게 트랜잭션 메모리

- 일단 해보고 실패하면 처음부터 다시하셈
- 별도 버전을 만들어서 변경하고 그 묶음 처리가 끝나면 반영하는거지
- 근데 이건 쓰는 처리 빈도가 높으면 계속 다시 고쳐서 하기 때문에 성능 나빠짐 ㅠㅠ

## 정리

병행 처리는 아직까지도 많은 사람 고민하게 만들고 있음. 

- 공유 → 비공유 → 공유
- 협력 → 비협력 → 협력
- 하드웨어 → 소프트웨어 → 하드웨어

2가지 방법 사이에서 방황중… ⇒ 한쪽만 배우지말고 양쪽 모두 익혀서 균형 있게 사용해야함!!

# 그럼 오늘 스터디에서는 뭘 다뤄볼까?

*생각해보기*

Q1. 어떤 상황에서 배열과 연결 리스트 중 어떤 것을 선택해야 할까? 

각각의 장단점을 고려할 때, 어떤 상황에서 배열을 사용하는 것이 적합하고, 어떤 상황에서는 연결 리스트를 사용하는 것이 더 효율적일지. 

Q2. 병행 처리에서 경합 상태나 교착 상태를 방지하기 위한 3가지 중에서 어떤 방법이 가장 효과적이라고 생각하는지?

+ 이제 2 Chapter 남았는데, 지금까지 책 읽고 나서 소감, 혹은 궁금한 점 등…히히