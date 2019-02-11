## 3.6 데이터 저장을 위한 클래스 TTree

### 데이터 저장

[TTree](https://root.cern.ch/doc/master/classTTree.html) 클래스는 정해진 데이터 세트의 배열이라고 할 수 있다.
`TTree`는 실제 나무처럼 브랜치(`TBranch`)와 리프(`TLeaf`)를
가지고 있는데 성질을 다르지만 이해하는데 도움은 된다.

먼저 `TTree`에는 한개 혹은 그 이상의 브랜치를 가지고 있을 수 있다.
각 브랜치는 저장되는 변수의 데이터 타입을 결정하고 그 타입을 가진 변수만을 연속적으로 저장한다.
이렇게 브랜치에 저장되는 변수는 리프라고 한다.
같은 `TTree` 아래 서로 다른 브랜치는 모두 같은 개수의 리프를 저장한다.
말로만 들으면 어려우므로 실제로 어떻게 사용하는지 알아보자.

`TTree`를 생성할 때는 반드시 사전에 `TFile`을 생성해야 한다.

```c++
TFile *file = new TFile("file.root","recreate");
```
`TTree`는 `TTree([이름], [타이틀])` constructor로 생성한다.

```c++
TTree *tree = new TTree("data","tree title")
```
먼저 `TTree`는 브랜치라는 것을 등록해서 내가 어떤 타입의 데이터를 저장할 지 알려주어야 한다.
브랜치는 여러개 등록할 수 있다. 브랜치를 등록하려면 1)이름과 2)데이터 전달 역할을 하는 변수가 필요하다.
가령 실험에서 관측한 입자의 종류와 운동량의 크기를 저장한다고 하자.
입자의 종류를 integer 형태로 저장한다고 할 때 다음과 다음과 같이
`TTree::Branch([이름], &[전달변수])` 함수를 이용해서 브랜치를 생성할 수 있다.

```c++
Int_t pid;
tree -> Branch("particleID",&pid);
```
위  `Branch()` 함수에서 첫번째 변수는 브랜치의 이름이며 두번째는 전달변수다. 이때 전달변수 앞에 '&'가 붙어있는데 이는 `TTree`에 전달변수의 주소를 알려주기 위함이다. 주소를 전달해 주어야 하는 것을 잊고 프로그래밍을 하다가 프로그램이 멈추거나 제대로 동작하지 않는 실수를 아주 많이 하는데 잘 기억해 두어야 할 점이다.

두번째 브랜치는 운동량의 크기이므로 double형의 데이터를 사용한다.

```c++
Double_t mom;
tree -> Branch("momentum",&mom);
```
이렇게 트리에 브랜치 두개가 등록이 되면 앞으로 데이터를 저장할 때 "particleID" 브랜치와
"momentum" 브랜치가 하나의 세트 {`pid`, `mom`}의 형태로 저장이 된다.
저장은 `TTree::Fill()` 함수로 하게 되는데 `Fill()` 함수를 실행하면
"particleID" 브랜치는 전달변수 `pid`에 들어있던 값을 저장하고
"momentum" 브랜치는 전달변수 `mom`에 들어있던 값을 저장한다.
다음을 보자.

```c++
pid = 1;
mom = 5.192;
tree -> Fill();
```
위 코드를 실행하게 되면 트리에 들어있는 두 브랜치는 전달변수 `pid`와 `mom`에 들어있던 값을 참고하여
{1, 5.192}를 저장하게 된다. 표로 그려보면 다음과 같다.

|particleID (Int_t)	|momentum (Double_t)|
| --- | --- |
| 1	| 5.192 |
이렇게 `Fill()`을 통하여 데이터를 채우는 작업을 엔트리(entry)를 채운다고 한다.

마지막으로 프로그램을 종료하기 전에 트리를 파일에 쓰는 것을 잊지 말자.
다른 `TObject`와 마찬가지로 `Write()` 함수를 사용한다.

```c++
void writeTree() {
    TFile* file = new TFile("file.root","recreate");
    TTree* tree = new TTree("data","particles");

    Int_t pid;
    tree -> Branch("particleID",&pid);
    Double_t mom;
    tree -> Branch("momentum",&mom);

    pid = 1;
    mom = 5.192;
    tree -> Fill();

    pid = 1;
    mom = 3.004;
    tree -> Fill();

    pid = 2;
    mom = 12.559;
    tree -> Fill();

    tree -> Write();
    file -> Close();
}
```

|particleID (Int_t)	| momentum (Double_t) |
| --- | --- |
|1	| 5.192 |
|1	| 3.004 |
|2	| 12.559 |

**힘들게 tree를 만들고서 저장하지 않고 종료하게 된다면 절대 데이터를 복구할 수 없으니 주의하도록 하자.**


### 데이터 읽기

`TTree`를 파일에서 꺼내온 후 데이터를 읽으려면 브랜치를 다시 연결해야 한다.
이미 존재하는 브랜치를 연결하는 것은 `SetBranchAddress([이름], &[전달변수])`
함수를 이용하는데 `Branch()` 함수에서의 전달변수와는 다르게 이번에는 값을 받아오는 역할을 한다.
브랜치의 이름은 저장할 때 설정했던 이름을 적는다.

```c++
Int_t pid;
tree -> SetBranchAddress("particleID",&pid);
Double_t mom;
tree -> SetBranchAddress("momemtum",&mom);
```
데이터는 엔트리 별로 꺼내올 수 있는데 엔트리 번호는 0번부터 시작하며 `GetEntry([번호])`` 함수를 이용한다.

```c++
void readTree() {
    TFile *file = new TFile("file.root","read");
    TTree *tree = (TTree *) file -> Get("data");

    Int_t pid;
    tree -> SetBranchAddress("particleID",&pid);
    Double_t mom;
    tree -> SetBranchAddress("momentum",&mom);

    tree -> GetEntry(0);
    cout << pid << " " << mom << endl;
    tree -> GetEntry(1);
    cout << pid << " " << mom << endl;
    tree -> GetEntry(2);
    cout << pid << " " << mom << endl;
}
```
```bash
> root readTree.C
root [0]
```
```
Processing readTree.C...
1 5.192
1 3.004
2 12.559
root [1]
```
