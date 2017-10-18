원문: https://engineering.heroku.com/blogs/2015-02-04-incremental-gc/


## 1. Mark and Sweep (M&S)
첫번째 버전은 M&S 알고리즘이었습니다.  가장 단순한 GC 알고리즘 중 하나로 두 단계로 되어있습니다.
  1. Mark - 모든 살아있는 객체에 "living object" Mark를 합니다.
  2. Sweep - Mark가 되어있지 않은 모든 객체를 수거합니다.

모든 "living object"는 실제로 사용 중이라는 것을 전제합니다.

M&S는 매우 간단했기 때문에 C extension 라이브러리를 작성하기 쉽다는 장점이 있었습니다. 하지만 지금은 FFI (foreign function interface)을 쓰기 때문에 더 이상 이런 장점이 중요하지 않습니다.

단점
  - Throughput  
  - Pause time

## 2. Lazy Sweep (1.9.3부터 나옴)
Pause time을 개선합니다. 한번에 Mark하지 않고 나눠서 Mark를 합니다.
  - white object - 마크되어있지 않음
  - grey object - 마크됨 하지만 black object를 레퍼런스로 가지고 있음
  - black object - 마크 됨 하지만 white object를 포인트하고 있지 않음

모든 존재하는 객체는 white로 마트되어있다가 living object인 경우에 grey object가 됩니다. grey object가 레퍼런스로 가지고 있는 다른 객체들을 방문해서 grey로 표현합니다. 그리고 original object는 black object로 바꿉니다. (leaf node에 있는 객체부터 black이 된다는 의미인듯) grey 오브젝트가 남지 않을 때까지 반복합니다. 모든 살아있는 객체가 black object가 되어있으면 다른 객체들을 수집(Sweep)합니다.

루비가 실행되는 동안 black object가 white object를 레퍼런스하는 문제가 있을 수 있습니다. 이를 방지하기 위해 write-barrier를 사용합니다. write-barrier는 black object로부터 white object로 레퍼런스가 생성되는 것을 감시합니다. black object는 grey object로 다시 바꿉니다.

## 3. 2세대 GC (2.1버전부터 나옴)
throughput을 해결하기 위해서 나왔습니다. 일반적인 OOP에서 대부분 living object가 빨리 죽는 것에 착안해서 Young space에 최신 객체를 두고 삭제합니다.
heap을 두개의 공간으로 나눔 → Young space(GC by Minor or Major), Old space(GC by Major)

## 4. 3세대 GC (2.2버전부터 나옴)
  1. 모든 객체를 white object로 바꿈
  2. 분명한 living object를 grey로 바꿈, stack에 포함된 객체들도 포함한다
  3. 한개의 grey object를 뽑아서 레퍼런스하고 있는 객체들을  grey로, original object는 black object로 바꿈. 모든 객체가 white 또는 black이 될때까지 반복함
  4. unprotected black object들을 한번에 스캔함, 왜냐하면 white object를 레퍼런스할 수 있기 때문
  5. 모든 white object들을 수집

black object가 white object를 레퍼런스하는 문제는 생기지 않았지만 pause time이 길어지는 문제가 되지만 대부분의 ruby 객체들은 write barrier protected object입니다. 이는 모두 Major GC에 대한 설명입니다. minor GC에 대한 pause time이슈는 없기 때문입니다.
