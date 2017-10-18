On-Call Rotation은 개발자가 돌아가면서 일정한 주기(파티에 참여하지 않은 사람/N)로 Production의 문제에 반응하거나 비개발자들의 업무 요청을 처리하는 것을 뜻합니다. 하지만 *On-Call 개발자는 즉시 기능을 구현하기 위한 개발자가 아닙니다.* On-Call 개발자는 제한 된 경우에만 요청을 처리합니다. 클라이언트를 포함한 모든 개발자가 대상이 됩니다. 입사한지 얼마 안되거나 파악이 덜 된 사람은 On-Call에서 배제합니다.

## 장점
* 매니저로 몰리는 커뮤니케이션 집중 문제를 해결할 수 있습니다.
* On-Call이 아닌 개발자들이 현재 Schedule에만 집중할 수 있습니다.
* 한 가지 문제 때문에 여러사람이 중복으로 문제를 탐색하는 비용을 줄일 수 있습니다.
* 비 개발팀에서 누구에게 연락해야하는지 바로 알 수 있습니다.
* 개발자가 시스템에 대해서 더 잘 이해 할 수 있습니다.
* 문제가 생긴 경우 개발자에게 주저없이 연락할 수 있습니다.(임시적인 업무기 때문에 감당 가능함.)

## 요청이 가능한 경우
버그 또는 장애의 경우 : 장애는 장애등급 및 대응 법칙 참고

## 요청 방식
1. 버그 또는 장애의 경우
  - Jira에 버그 티켓을 만듭니다.
  - #emergency 채널에 알립니다.

## On-Call 개발자가 되면 해야하는 준비
  - 휴가 계획이 있는 경우 다른 사람과 순서를 바꾸거나 휴가 계획을 수정합니다.
  - #emergency 채널의 알림에 대응할 수 있도록 slack앱을 받고 알림을 켜둡니다.
  - 모니터링 툴 사용법을 익힙니다.
  - Production에 접근 권환을 얻습니다.
  - 집에서도 문제를 해결할 수 있도록 VPN을 세팅해둡니다.
  - 개발자들의 연락처를 알아둡니다.
  - 배포 방법을 익혀두고 배포 과정을 알아둡니다.

## Rotation
Rotation은 개발자 중 일주일씩 번갈아가면서 담당하게 됩니다. On-Call Rotation 스프레드 시트에서 담당자를 알 수 있습니다.
순서의 변경이 필요한 경우 On-Call 관리자 또는 부관리자에게 문의하여 변경합니다. 사전 변경은 자유롭게 할 수 있습니다.

## On-Call 개발자가 할일
1. 문제 정의
문제가 맞는지 확인합니다. 그리고 문제가 정말 시급한지도 판단합니다.(Production에 무언가를 가하는 것 자체가 더 큰 문제를 야기할 수 있음) 문제를 혼자 파악하기 어려운 경우, 도움이 될 사람에게 연락하거나 매니저에게 연락하여 누가 이런 경우에 도움을 줄 수 있는지 연락합니다.

2. 격리(Containment)
문제가 팀원에 의해서 다시 발생하지 않도록 주의 조치 또는 접근 제한 조치를 합니다. 더 나빠지지 않도록 조치를 취합니다.

3. 근절(Eradication)
문제의 원인을 파악하고 다시 일어나지 않도록 문제의 원인을 제거합니다.

4. 회복
Production이 정상적으로 동작하도록 합니다. 배포과정이 자동화 되어있지 않으면 배포가 가능한 개발자의 도움을 받습니다.

5. 회고
문제가 왜 발생했는지 팀원에게 원인을 알립니다. 처리하는 방식에서의 어려움이 있으면 다음 On-Call 개발자를 위해 처리 방식이 개선되도록 합니다. 재발을 방지할 수 있는 방법을 찾아냅니다.