# UIED - UI element detection, detecting UI elements from UI screenshots or drawnings

This project is still ongoing and this repo may be updated irregularly, I developed a web app for the UIED in http://uied.online

## Related Publications:

[1. UIED: a hybrid tool for GUI element detection](https://dl.acm.org/doi/10.1145/3368089.3417940)

[2. Object Detection for Graphical User Interface: Old Fashioned or Deep Learning or a Combination?](https://arxiv.org/abs/2008.05132)

> The repo has been **upgraded with Google OCR** for GUI text detection, to use the original version in our paper (using [EAST](https://github.com/argman/EAST) as text detector), check the relase [v2.3](https://github.com/MulongXie/UIED/releases/tag/v2.3) and download the pre-trained model in [this link](https://drive.google.com/drive/folders/1MK0Om7Lx0wRXGDfNcyj21B0FL1T461v5?usp=sharing).

## What is it?

UI Element Detection (UIED) is an old-fashioned computer vision (CV) based element detection approach for graphic user interface.

The input of UIED could be various UI image, such as mobile app or web page screenshot, UI design drawn by Photoshop or Sketch, and even some hand-drawn UI design. Then the approach detects and classifies text and graphic UI elements, and exports the detection result as JSON file for future application.

UIED comprises two parts to detect UI text and graphic elements, such as button, image and input bar.

-   For text, it leverages [Google OCR](https://cloud.google.com/vision/docs/ocr) to perfrom detection.

-   For graphical elements, it uses old-fashioned CV approaches to locate the elements and a CNN classifier to achieve classification.

> UIED is highly customizable, you can replace both parts by your choice (e.g. other text detection approaches). Unlike black-box end-to-end deep learning approach, you can revise the algorithms in the non-text detection and merging (partially or entirely) easily to fit your task.

![UIED Approach](https://github.com/MulongXie/UIED/blob/master/data/demo/approach.png)

## How to use?

### Dependency

-   **Python 3.5**
-   **Opencv 3.4.2**
-   **Pandas**
<!-- * **Tensorflow 1.10.0**
-   **Keras 2.2.4**
-   **Sklearn 0.22.2** -->

### Installation

<!-- Install the mentioned dependencies, and download two pre-trained models from [this link](https://drive.google.com/drive/folders/1MK0Om7Lx0wRXGDfNcyj21B0FL1T461v5?usp=sharing) for EAST text detection and GUI element classification. -->

<!-- Change ``CNN_PATH`` and ``EAST_PATH`` in *config/CONFIG.py* to your locations. -->

The new version of UIED equipped with Google OCR is easy to deploy and no pre-trained model is needed. Simply donwload the repo along with the dependencies.

> Please replace the Google OCR key at `detect_text/ocr.py line 28` with your own (apply in [Google website](https://cloud.google.com/vision)).

### Usage

To test your own image(s):

-   To test single image, change _input_path_img_ in `run_single.py` to your input image and the results will be output to _output_root_.
-   To test mutiple images, change _input_img_root_ in `run_batch.py` to your input directory and the results will be output to _output_root_.
-   To adjust the parameters lively, using `run_testing.py`

> Note: The best set of parameters vary for different types of GUI image (Mobile App, Web, PC). I highly recommend to first play with the `run_testing.py` to pick a good set of parameters for your data.

## Folder structure

`cnn/`

-   Used to train classifier for graphic UI elements
-   Set path of the CNN classification model

`config/`

-   Set data paths
-   Set parameters for graphic elements detection

`data/`

-   Input UI images and output detection results

`detect_compo/`

-   Non-text GUI component detection

`detect_text/`

-   GUI text detection using Google OCR

`detect_merge/`

-   Merge the detection results of non-text and text GUI elements

The major detection algorithms are in `detect_compo/`, `detect_text/` and `detect_merge/`

## Demo

GUI element detection result for web screenshot

![UI Components detection result](https://github.com/MulongXie/UIED/blob/master/data/demo/demo.png)

## How to use PaddleOCR (không cần Google Key)

UIED hỗ trợ sử dụng PaddleOCR để nhận diện text mà không cần Google OCR key. Để sử dụng PaddleOCR:

-   Khi chạy nhận diện text, hãy truyền tham số `method='paddle'` vào hàm `text_detection` trong file `detect_text/text_detection.py`.
-   Ví dụ, trong file `run_single.py` hoặc script của bạn:

```python
import detect_text.text_detection as text
text.text_detection(input_file, output_file, show=True, method='paddle')
```

-   PaddleOCR sẽ tự động tải model khi chạy lần đầu (cần internet lần đầu).
-   Không cần chỉnh sửa hay nhập Google Key.

> Nếu muốn dùng Google OCR, bạn cần đăng ký key và thay vào file `detect_text/ocr.py` như hướng dẫn cũ.

---
