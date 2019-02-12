# 컨벤션 & 타입

## 코딩 컨벤션

다음은 ROOT 패키지 안에서 사용하는 코딩 컨벤션이다. ROOT를 사용하면서 도움이 되므로 알아 두자.
- 클래스는 **T** 로 시작한다. `TObject`
- 클래스를 제외한 타입은 **_t** 로 끝난다. `Int_t`
- 데이터 멤버는 **f** 로 시작한다. `fName`
- 멤버 함수는 대문자로 시작한다. `SetName`
- 상수는 **k** 로 시작한다. `kRed`
- 글로벌 변수는 **g** 로 시작한다. `gPad`

## 타입

`int`와 같은 타입은 컴퓨터에 따라서 그 크기가 다를 수 있다.  이를 위해서 ROOT에서는 다음과 같이 타입을 정의해 두었다.

| 타입 | 설명 | 최소 | 최대 |
| --- | --- | --- | --- |
|`Char_t`    | 1 byte, Character | -128 | 127 |
|`Short_t`   | 2 byte, Short     | -32,768 | 32,767 |
|`Int_t`     | 4 byte, Integer   | -2,147,583,648 | 2,147,483,647 |
|`Long64_t`  | 8 byte, Integer   |
|`Float_t`   | 4 byte, Float     | 3.4e-38 | 3.4e+38, 7 digits |
|`Double_t`  | 8 byte, Float     | 1.79e-308 | 1.79e+308, 15 digits |
|`Bool_t`    | 1 bit,  Boolean   | 0 | 1 |

Unsinged 타입은 앞에 **U** 를 붙인다: `UChar_t`, `UShort_t`, `UInt_t`, `ULong64_t`.

---

[이전](root.md)
[다음](root1.md)
