## 3.5 ROOT 파일과 TFile 클래스

### TFile 열고 닫기

[TFile](https://root.cern.ch/doc/master/classTFile.html)은 `TObject`의 파생클래스를 (아마도 `TDirectoryFile` 파생클래스를 제외하고) 모두 저장할 수 있는 파일 클래스이다.  `TFile`을 통해서 읽고 쓰는 파일을 루트 파일이라고 하며 `.root` 확장자를 사용하여 구분한다.  `TFile`의 정의는 다음과 같다.

### 읽기 위해서 파일 열기
```c++
TFile *file = new TFile("file.root","read");
```

### 쓰기 위해서 파일 열기
```c++
TFile *file = new TFile("file.root","recreate");
```
첫번째 매개변수는 루트 파일의 이름, 두번째는 옵션으로, 파일을 쓸때는 "recreate", 읽을때는 "read"를 사용한다.  파일을 닫을 때는 `Close()` 함수를 사용한다.

### 파일 닫기

```c++
file.Close();
```

### TFile에 TObject 쓰기

생성된 모든 TObject를 저장할 때는 `TFile` 클래스의 Write() 함수를 사용한다. 예를 들어서 히스토그램을 두개 만들고 두 히스토그램 모두를 파일에 저장할 때는 다음과 같다.

```c++
void writeRootFile() {
    TFile* file = new TFile("file.root","recreate");

    TH1D* hist1 = new TH1D("histogram1","first histogram",10,0,10);
    TH1D* hist2 = new TH1D("histogram2","second histogram",20,-10,10);

    //...

    file -> Write();
    file -> Close();
}
```

개별적으로 `TObject`를 저장하고자 할 때는 `TObject의 Write()` 함수를 사용한다.  위 예제에서 hist1만을 파일에 저장할 때는 다음과 같다.

```c++
void writeRootFile2() {
    TFile* file = new TFile("file.root","recreate");

    TH1D* hist1 = new TH1D("histogram1","first histogram",10,0,10);
    TH1D* hist2 = new TH1D("histogram2","second histogram",20,-10,10);

    //...

    hist1 -> Write();
    file -> Close();
}
```

일을 하다보면 `TFile`을 두개 이상 열 때도 있다.  이때 `TObject`를 저장할 파일을 지정하기 위해서 `TFile` 의 `cd()` 함수를 사용한다.

```c++
void writeRootFile3() {
    TFile* file1 = new TFile("file1.root","recreate");
    TFile* file2 = new TFile("file2.root","recreate");

    TH1D* hist1 = new TH1D("histogram1","first histogram",10,0,10);
    TH1D* hist2 = new TH1D("histogram2","second histogram",20,-10,10);

    //...

    file1 -> cd();
    hist1 -> Write();

    file2 -> cd();
    hist2 -> Write();

    file1 -> Close();
    file2 -> Close();
}
```

### TFile에서 TObject 읽기

ROOT 커맨드 라인에서 파일을 읽을 때는 매크로를 읽을 때와 마찬가지로 'root' 명령어 후에 파일 이름을 쓴다.

```bash
> root file.root
root [0]
Attaching file file.root as _file0...
(TFile *) 0x7f953a32a440
root [1] .ls
TFile **                   file.root
  TFile*                   file.root
    KEY: TH1D         histogram1;1     first histogram
    KEY: TH1D         histogram2;1     second histogram
root [2]
```
이후에 `.ls` 명령어를 사용하면 파일안의 내용을 볼 수 있다.  자세한 내용은 생략하도록 하고 파일에 저장된 `TObject`는 **KEY:** 라는 문구로 시작하는 줄에서 확인할 수 있다.  그 이후에 따라오는 정보는 - [클래스] [이름];[싸이클 번호] [타이틀] - 순이다.  [싸이클 번호]는 몇번을 저장했는지에 따라서 결정되는 숫자다.  커맨드 라인에서는 `TObject`의 [이름]을 c++에서 하나의 변수처럼 그대로 사용할 수 있다.

```bash
root [2] histogram1 -> Draw()
Info in <TCanvas::MakeDefCanvas>:  created default TCanvas with name c1
root [3]
```

그래픽 인터페이스인 `TBrowser`를 이용하여 탐색하는 것도 가능하다.

```bash
root [4] TBrowser b
(TBrowser &) Name: Browser Title: ROOT Object Browser
root [5]
```
`TBrowser`는 상당히 직관적이므로 설명은 생략한다.

매크로를 이용해서 파일을 읽고 `TObject`를 가져올때는 `TFile`의 `Get([이름])` 함수를 이용한다.  이때 주의할 점은 `Get()` 함수가 객체를 `TObject`의 형태로 반환하므로 형변환을 해 주어야 한다는 점이다.  `TH1D`를 가져오는 경우 다음과 같다.

```c++
void readFile() {
    TFile *file = new TFile("file.root","read");
    TH1D *hist = (TH1D*) file -> Get("histogram1");
    hist -> Draw();
}
```
`Get()` 함수에 매개변수로 들어가는 이름은 저장할 때의 TH1D 이름과 같다.

---

[이전](root3.4.md)
[다음](root3.6.md)
