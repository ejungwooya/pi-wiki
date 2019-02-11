## 2.4 Namespace TMath

Namespace인 `TMath`는 물리를 하면서 흔히 쓰이는 상수와 함수를 모아두었다.  c++의 cmath 함수들과 겹치는 부분도 있고 겹치지 않는 부분도 있다.

### 상수의 예

- 원주율, π = `TMath::Pi()`
- 빛의 속도 = `TMath::C()`

### 함수의 예

- 가우시안 함수 = `TMath::Gaus(x, mean, sigma)`
- arctangent(y/x) = `TMath::aTan2(y,x)` (범위: -π ~ π)

여기서 몇가지 기능을 소개하는 것 보다 직접 목록을 들여다 보는 것이 쉽고 빠르다.

* [TMath](https://root.cern.ch/root/html524/TMath.html)
