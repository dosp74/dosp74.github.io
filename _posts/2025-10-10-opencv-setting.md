---
title: "[컴퓨터 비전] opencv 세팅하기"
date: 2025-10-10 18:28:00 +0900
last_mod: 2025-10-10
categories: [컴퓨터 비전]
tags: [컴퓨터 비전, opencv, Visual Studio]
---

![Image](/assets/images/2025-10-10/2025-10-10-opencv.png)

OpenCV(Open Source Computer Vision)는 실시간 컴퓨터 비전을 목적으로 한 프로그래밍 라이브러리로, 인텔이 개발하였다.

필자는 Windows 환경에서 opencv 프로그래밍 환경을 세팅해보고자 한다.

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-1.png)

`opencv releases` 검색

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-2.png)

`opencv.org/releases` 접속

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-3.png)

최신 버전의 Windows exe 파일 다운로드

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-4.png)

실행하면 이런 화면이 뜨는데 원하는 경로 설정 후 `Extract`

필자는 C 드라이브를 선택했다.

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-5.png)

그러면 opencv 폴더가 생긴다.

## 세팅

### 실행용 DLL 경로 추가

```plaintext
시작 -> 시스템 환경 변수 편집 -> 환경 변수 -> 시스템 변수에서 Path 선택 -> 편집 -> 새로 만들기 -> C:\opencv\build\x64\vc16\bin 입력 -> 확인
```

버전에 따라 `\vcxx\` 경로가 바뀔 수 있으니 해당 경로가 실제로 있는지 확인하자.

이후 Visual Studio에서 C++ 프로젝트를 만든다.

### 포함 디렉터리 설정

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-6.png)

```plaintext
솔루션 탐색기 -> 프로젝트 이름 우클릭 -> 속성
```

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-7.png)

```plaintext
VC++ 디렉터리 클릭 -> 오른쪽의 포함 디렉터리 선택 -> 드롭다운 -> 편집 -> 줄 추가 버튼 클릭 -> C:\opencv\build\include 입력 -> 확인 -> 적용
```

### 라이브러리 디렉터리 설정

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-8.png)

```plaintext
같은 창에서 라이브러리 디렉터리 선택 -> 드롭다운 -> 편집 -> 줄 추가 버튼 클릭 -> C:\opencv\build\x64\vc16\lib 입력 -> 확인 -> 적용
```

마찬가지로 압축해제 폴더 확인 후 `vc16`이 다르면 그 이름으로 변경

### 링커 추가 종속성 설정

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-9.png)

```plaintext
왼쪽 구성 속성에서 링커 -> 입력 -> 추가 종속성 선택 -> 드롭다운 -> 편집 -> opencv_world4xxd.lib 입력 -> 확인 -> 적용
```

`xx`는 설치한 버전에 맞게 바꾸어야 한다. 필자는 `opencv_world4120d.lib`으로 지정해주었다.

참고로 디버그 빌드일 때만 `d`가 붙고, 릴리스 빌드 시에는 `d`가 붙지 않는다.

### 테스트 빌드

```cpp
#include <opencv2/opencv.hpp>
using namespace cv;

int main() {
	Mat image = imread("Lena.png");
	imshow("test", image);
	waitKey(0);

	return 0;
}
```

소스 파일이 있는 경로에 출력할 이미지를 두고 위의 테스트 코드를 빌드해보자.

![Image](/assets/images/2025-10-10/2025-10-10-opencv-setting-10.png)

이미지가 로드되면 opencv 설정을 완료한 것이다.
