## 2.2 문자열 클래스 TString

문자 혹은 문자열을 사용하고 싶은데 char, string 을 사용하다가 사고회로가 멈추기 직전이라면 ROOT의 [TString 클래스](https://root.cern.ch/doc/master/classTString.html)를 사용해 보는 것을 추천한다.  `TString`은 문자 혹은 문자열에 관한, 우리가 생각 할 수 있는 거의 모든 귀찮은 일을 대신 해주며 직관적으로 사용할 수 있는 유용한 클래스이다. 간단한 사용법을 알아보자.

### 정의
``` c++
TString s = "apple";
```

### 크기 Sizeof
문자열은 항상 문자열의 끝을 나타내는 `null(0x00)`을 포함한다.  따라서 아무것도 들어있지 않은 문자열의 크기는 1이며 "apple"의 경우 6이 된다.  `Sizeof` 함수를 이용하여 확인할 수 있다.
```c++
TString s = "apple";
Int_t length = s.Sizeof();
cout << length << endl;
```
```
6
```

### 문자의 위치
문자열의 위치는 0부터 시작하며 `[]` 오퍼레이터를 사용하여 `char`의 형태로 꺼내올 수 있다.
```c++
TString s = "apple";
cout << s[0] << endl;
cout << s[1] << endl;
cout << s[2] << endl;
cout << s[3] << endl;
cout << s[4] << endl;
```
```
a
p
p
l
e
```

### 출력
```c++
TString s = "apple";
cout << s << endl;
```
```
apple
```

### const char *
`(const char *)`의 형태로 치환은 `Data` 함수를 사용한다.
```c++
TString s = "apple";
const char *c = s.Data();
```

### 더하기 +
문자열을 더해서 새로운 TString을 만들때는 (당연하지만) 첫 시작을 TString으로 해야한다. 즉, 아래 코드는 가능하지만,
```c++
TString s0 = "apple"
TString s1 = "juice"
TString s2 = s0 + " " + s1;
cout << s2 << endl;
```
```
apple juice
```
아래는 가능하지 않다.
```c++
TString s1 = "juice"
TString s2 = "apple " + s1;
```
```
에러
```
### 문자열을 숫자로 Atoi, Atof
`Atoi (ASCII to integer)`와 `Atof (ASCII to float)` 함수는 TString이 숫자일 경우 정수와 실수로 바꿔서 리턴한다.
```c++
TString si = "12";
Int_t value_i = s0.Atoi();

TString sf = "3.45";
Double_t value_f = s1.Atof();
```

### 숫자를 문자열로 Itoa
숫자를 문자열로 바꾸고 싶을 때 `Itoa([정수],10)` 함수를 사용한다.  [정수]는 문자열로 바꾸려 하는 숫자, 10은 10진수를 사용한다는 의미이다.
```c++
Int_t value = 123;
TString s = TString::Itoa(value, 10);
```

### 찾기 Index
예를 들어 "too much apple juice"라는 문자열에 "to"라는 문자열이 포함되어 있는지, 들어있다면 몇번째 위치에 있는지 알고 싶을 때는 Index("[찾고싶은 문자열]") 함수를 사용할 수 있다.  출력 하는 정수는 그 문자열이 시작하는 위치를 알려준다.  위치는 0부터 시작하므로 "to"를 찾으면 0을 리턴한다.  만약에 찾고자 하는 문자가 없다면 `-1` 을 리턴한다.

```c++
TString s = "too much apple juice";

Int_t idx0 = s.Index("to");
Int_t idx1 = s.Index("apple");
Int_t idx2 = s.Index("water");

cout << "to is at " << idx0 << endl;
cout << "apple is at " << idx1 << endl;
cout << "water is at " << idx2 << endl;
```
```
to is at 0
apple is at 9
water is at -1
```
