## 2.5 무작위 숫자 생성 클래스 TRandom

`TRandom`은 무작위 숫자를 생성하는 클래스이며 종류에는 여러가지가 있다: TRandom1, TRandom2, TRandom3 등.  자세한 이야기는 다음을 보자: [TRandom 클래스 레퍼런스](https://root.cern.ch/doc/master/classTRandom.html)

`TRandom`은 ROOT를 실행할 때 `gRandom` 이라는 글로벌 포인터를 자동으로 생성한다.  기본설정으로 `TRandom3`를 사용하며 숫자 생성 방법에 대해서 크게 신경쓰지 않는다면 그냥 `gRandom`을 사용하면 편하다.

### Seed

무작위 숫자를 생성할 시 주의할 점은 이 숫자들이 런타임에 관계없이 같은 숫자가 반복된다는 점이다.  예를 들어서 10개의 무작위 숫자를 생성하는 같은 코드를 두번 돌렸을 때, 두 코드가 무작위하게 다른 숫자를 사용하는 것이 아니라 같은 10개의 숫자를 사용한다.  이를 방지하기 위해서 seed 라는 바꿔가면서 코드를 돌려야 하는데 이를 설정하는 방법은 `SetSeed(ULong_t seed)` 함수를 불러오는 것이다.  그리고 seed 또한 무작위하게 입력해야 하기때문에 사람이 하는 일에 한해서 가장 좋은 방법은 시간을 seed 로 설정하는 것이다.  간단하게 말해서 코드의 가장 윗줄에 다음과 같이 쓰자.  
```c++
gRandom -> SetSeed(time(0));
```

### 숫자 생성

`Uniform(Int_t x)` 함수는 0부터 x사이의 무작위 숫자를 균일한 함수로 생성한다.
`Gaus(Double_t mean, Double_t sigma)` 함수는 mean, sigma 값을 가지는 가우시안 분포에서 무작위 숫자를 생성한다.

```c++
Double_t random_number1 = gRandom -> Uniform(1);​
Double_t random_number2 = gRandom -> Gaus(0,2);
```
추가로 나중에 함수 클래스 TF1을 배우면 `TF1::GetRandom()` 함수를 이용하여 함수 분포에서 무작위 숫자를 생성하는 것도 가능하다.
