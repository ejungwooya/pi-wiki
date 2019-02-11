## 2.3 벡터(수학) 클래스 TVector3

3차원 위치, 혹은 3차원 벡터를 쉽게 다루고 싶다면 ROOT의 [TVector3](https://root.cern.ch/doc/master/classTVector3.html) 클래스를 사용할 수 있다.  업그레이드 된 버전으로 4벡터 클래스인 [TLorentzVector](https://root.cern.ch/doc/master/classTLorentzVector.html) 클래스도 존재한다.  `TVector2` 클래스도 있지만 사용법이 다르고 기능도 적어서 추천하지 않는다.  참고로 `TVector3`, `TLorentzVector`, `TVector2` 는 모두 `TObject`를 상속한다.

### 정의
`TVector3`는 기본적으로 Double_t 타입의 맴버 변수 3개(fX, fY, fZ)를 가지고 있는 벡터 클래스다.  생성자에 값을 써줄 수 도 있고 써주지 않았다면 (0,0,0)으로 초기화한다.
```c++
TVector3 v(1.,2.,3.);
```

### 값의 입출력

TVector3의 데이터 값 입력은 여러가지 방법이 있다.  생성자에서 초기화 하거나 이미 들어있는 값을 바꿀때는 SetXYZ() 혹은 SetX(), SetY(), SetZ() 함수를 이용하여 바꿀 수 있다.
```c++
TVector3 v;
v.SetX(1.);
v.SetY(2.);
v.SetZ(3.);
cout << v.X() << " " << v.Y() << " " << v.Z() << endl;
```
```
1 2 3
```

### 크기, 방향
크기는 `Mag()`, 유닛벡터는 `Unit()` 함수로 가져올 수 있다.
```c++
Double_t magnitude = v.Mag();
TVector3 unitV3 = v.Unit();
```

### 기타 함수 목록
기타 여러가지 기능이 있으므로 간단한 목록으로 설명한다.  참고로 각도의 단위는 radian이며 π의 값은 TMath::Pi() 로 가져올 수 있다.

- `Double_t Mag()` : 벡터 크기를 출력 = sqrt(x*x+y*y+z*z)
- `Double_t Mag2()` : 벡터 크기의 제곱을 출력 = x*x+y*y+z*z
- `TVector3 Unit()` : 벡터의 유닛벡터를 출력
- `Double_t Perp()` : 벡터의 xy 평면상 크기를 출력 = sqrt(x*x+y*y)
- `Double_t Perp2()` : 벡터의 xy 평면상 크기의 제곱을 출력 = x*x+y*y
- `Double_t Theta()` : Perp() 성분과 z 성분 사이의 각도
- `Double_t CosTheta()` = cos(theta) = z/Mag()
- `Double_t Phi()` : xy 평면상의 polar angle = artan(y/x)
- `void Print()` : 정보를 화면에 출력

크기, 혹은 각도를 지정해서 값을 변경하는 방법도 있다.

- `void SetMag(Double_t mag)​`
- `void SetPerp(Double_t perp)`
- `void SetPhi(Double_t phi)`
- `void SetTheta(Double_t theta)`

이외의 함수는 [클래스 레퍼런스](https://root.cern.ch/doc/master/classTVector3.html)를 참조하자.
