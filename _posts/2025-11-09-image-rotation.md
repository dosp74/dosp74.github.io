---
title: "[컴퓨터 비전] Image Rotation 구현하기"
date: 2025-11-09 02:50:00 +0900
last_mod: 2025-11-09
categories: [컴퓨터 비전]
tags: [컴퓨터 비전, opencv, Visual Studio, Image Rotation]
---

![Image](/assets/images/2025-11-09/2025-11-09-image-rotation.png)

이번 포스팅에서는 **Image Rotation** 에 대해 다루려고 한다.

Image Rotation 공식을 이용하여 영상을 회전시켰다.

Lena.png 파일을 각각 30도, 45도, 60도 회전시킨 결과이다.

[소스 코드]

사용 언어: C++

환경: Microsoft Windows 10 Home, Visual Studio 2022, C:\opencv\...

```cpp
#include <opencv2/opencv.hpp>
#include <iostream>
#include <cmath>
using namespace cv;
using namespace std;

Mat rotateImage(const Mat& src, double degree) {
    int W = src.cols;
    int H = src.rows;
    Mat result(H, W, CV_8UC1, Scalar(0)); // 모든 픽셀 값을 0으로 초기화하고 시작

    double rad = degree * CV_PI / 180.0; // 라디안 = 각도 x π x 180
    double cos_t = cos(rad);
    double sin_t = sin(rad);

    for (int y = 0; y < H; y++) {
        for (int x = 0; x < W; x++) {

            // 이미지의 중심을 원점으로 이동
            double nx = x - W / 2.0;
            double ny = y - H / 2.0;

            // Image Rotation 공식 적용
            double srcX = cos_t * nx + sin_t * ny + W / 2.0; // y축은 아래 방향으로 증가(중요)
            double srcY = -sin_t * nx + cos_t * ny + H / 2.0;

            // 범위 체크 후 보간(여기서는 Bilinear Interpolation) -> 변환한 회전 좌표는 정수가 아닌 실수이므로 어떤 픽셀 값을 쓸 지 결정해야 함
            if (srcX >= 0 && srcX < W - 1 && srcY >= 0 && srcY < H - 1) {
                int x0 = (int)floor(srcX);
                int y0 = (int)floor(srcY);
                int x1 = min(x0 + 1, W - 1);
                int y1 = min(y0 + 1, H - 1);

                double dx = srcX - x0;
                double dy = srcY - y0;

                double p1 = src.at<uchar>(y0, x0);
                double p2 = src.at<uchar>(y0, x1);
                double p3 = src.at<uchar>(y1, x0);
                double p4 = src.at<uchar>(y1, x1);

                double q1 = (1 - dx) * p1 + dx * p2;
                double q2 = (1 - dx) * p3 + dx * p4;
                double q = (1 - dy) * q1 + dy * q2;

                // 픽셀 밝기 값 범위 보정(0 ~ 255)
                if (q < 0) {
                    q = 0;
                }

                if (q > 255) {
                    q = 255;
                }

                result.at<uchar>(y, x) = static_cast<uchar>(q);
            }
        }
    }

    return result;
}

int main() {
    Mat src = imread("Lena.png", IMREAD_GRAYSCALE);

    if (src.empty()) {
        cout << "이미지를 열 수 없습니다." << endl;

        return -1;
    }

    double angles[3] = { 30.0, 45.0, 60.0 };

    for (double deg : angles) {
        Mat rotated_image = rotateImage(src, deg);

        imshow(to_string((int)deg) + "도", rotated_image);
    }

    waitKey(0);

    return 0;
}
```

8비트일 때, unsigned char 형 픽셀의 범위는 0부터 255까지이므로 데이터를 최종적으로 출력하기 전에 이 범위 안으로 한정시키는 과정이 필요하다.

**원점을 중심으로 회전하는** 다음 회전식을 이용하였다.

![Image](/assets/images/2025-11-09/2025-11-09-image-rotation-formula.png)

따라서, 이미지의 중심을 원점으로 이동시킨 후 공식을 적용한다.

다만, 영상 좌표계에서 y축은 아래로 증가하기 때문에 sin Θ 값은 -를 붙여서 사용해야 한다.

회전 후에는 `+ W / 2.0, + H / 2.0`으로 다시 원래 중심으로 이동시켜준다.

또한, 변환한 회전 좌표는 정수가 아닌 실수이기 때문에 어떤 픽셀 값을 쓸지 결정해야 한다. 필자는 Bilinear Interpolation 기법을 사용하여 픽셀 값을 결정하였다.
