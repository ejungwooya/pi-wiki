## 2.1 기본 클래스 TObject와 TNamed

### TObject

ROOT의 거의 모든 클래스는 [TObject](https://root.cern.ch/doc/master/classTObject.html) 클래스로 부터 파생된다.  이러한 ROOT 클래스 구조의 장점은 파생클래스에 공동적인 동작을 제공하면서 일관성을 제공한다는 것이다.  아래 소개하는 TObject는 멤버 함수들은 자주 보게 될 함수들이므로 눈에 익혀두자.
- I/O: `Read()`, `Write()`​
- 프린팅: `Print()`​
- 그리기: `Draw(`)
- 클리어: `Clear()`​
- 정렬: `IsSortable()`, `Compare()`, `IsEqual()`

ROOT [메뉴얼](https://root.cern.ch/root/htmldoc/guides/users-guide/ROOTUsersGuide.html#tobject) 에서는 약 10가지 종류의 함수들을 소개 하였지만 일단은 위 함수정도만 알아두어도 도움이 될 것이다.  이 외에도 [TObject 클래스 레퍼런스](https://root.cern.ch/doc/master/classTObject.html) 에 나온 함수들은 모두 공통적으로 적용되며 파생 클래스에 따라서 그 기능이 달라질 수 있으니 **항상 클래스 레퍼런스를 잘 찾아보고 사용하도록 하자.**

큰 프로그램을 돌릴 때 TObject를 사용하면서 가지는 장점 중 하나는 메모리 관리가 유용하다는 점이다.  TObject는 메모리에 생성되는 즉시 ROOT에 의해서 관리되는데 프로그램 중간중간 마다 메모리에 존재하는 TObject의 종류와 크기를 체크할 수 있다. 방법은 다음과 같다.  프로그램을 돌리기 전에 홈(`$HOME`)에 `.rootrc`라는 파일을 생성한다.  이미 존재한다면 그 파일을 열고 다음과 같은 줄을 추가한다.
```
Root.MemStat:            1
Root.ObjectStat:         1
```
그리고 프로그램 중간중간마다 메모리 체크를 하고 싶은 지점에 다음과 같은 줄을 추가한다.
```c++
gObjectTable -> Print();
```
메모리 사용 내용은 화면에 출력된다. 컴파일을 하는 경우라면 `TObjectTable.h` 헤더를 추가해야 한다.  사실 메모리 관리는 초보자에게는 조금 과한 내용이지만 이런게 있다는 것만 알아둬도 나중에 크게 도움이 될 것이다.
* 참고 : [Tracking Memory Leaks](https://root.cern.ch/root/htmldoc/guides/users-guide/ROOTUsersGuide.html#tracking-memory-leaks)

### TNamed

[TNamed](https://root.cern.ch/doc/master/classTNamed.html)는
TObject의 파생 클래스인데 TObject에 ``이름``과 ``타이틀``이 붙은 클래스이다.
저장이 지원되는 클래스 중에서 서로를 구분할 필요가 있는 클래스들이 대부분
TNamed로 부터 파생되며 아마 우리가 사용하는 대부분의 클래스가 이에 속한다.
많은 경우에 TNamed 파생 클래스는 메모리상에 생성될 때 같은 이름이 있으면
메모리 충돌이 일어나므로 신경써야 할 부분이다.
또 나중에에 배우겠지만 데이터를 저장한 파일에서 TObject를 꺼내올 때 기본적으로 이름을 이용해서 데이터를 찾아온다.
따라서 같은 이름을 가진 두개이상의 데이터가 파일에 안에 존재한다면 매우 귀찮아 지므로 조심해야 한다.
이름을 부여하지 않는 경우도 마찬가지이다.
이름과 타이틀은 보통 클래스 constructor에서 부여하지만
그렇지 않은경우에는 `SetName(이름)`, `SetTitle(타이틀)` 혹은 `SetNameTitle(이름, 타이틀)` 함수를 이용하면 된다.
