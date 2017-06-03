# 利用 tesseract 解析简单数字验证码图片

tesseract 是一个 OCR（Optical Character Recognition，光学字符识别）引擎，能够识别图片中字符，利用这个可以用来解析一些简单的图片验证码  
Github 地址：[https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract)  

Windows 平台 v3.05.01 版本下载地址：[http://digi.bib.uni-mannheim.de/tesseract/tesseract-ocr-setup-3.05.01.exe](http://digi.bib.uni-mannheim.de/tesseract/tesseract-ocr-setup-3.05.01.exe)  

一开始弄这个是因为学校网络要上网每次都要在网页验证，就想能不能写个程序自动验证免去手动验证过程。但这需要验证码，为了解决这个问题，就上网搜了一下，就看到有用 tesseract 的。  

有人用 Python 实现了一个工具：[https://github.com/madmaze/pytesseract](https://github.com/madmaze/pytesseract)  

拿来试了一下，Windows 上使用总是有问题  
我就把目光转向了 tesseract 本身，这是它的使用说明  
```
C:\Program Files\Tesseract-OCR>tesseract
Usage:
  tesseract --help | --help-psm | --version
  tesseract --list-langs [--tessdata-dir PATH]
  tesseract --print-parameters [options...] [configfile...]
  tesseract imagename|stdin outputbase|stdout [options...] [configfile...]

OCR options:
  --tessdata-dir PATH   Specify the location of tessdata path.
  --user-words PATH     Specify the location of user words file.
  --user-patterns PATH  Specify the location of user patterns file.
  -l LANG[+LANG]        Specify language(s) used for OCR.
  -c VAR=VALUE          Set value for config variables.
                        Multiple -c arguments are allowed.
  -psm NUM              Specify page segmentation mode.
NOTE: These options must occur before any configfile.

Page segmentation modes:
  0    Orientation and script detection (OSD) only.
  1    Automatic page segmentation with OSD.
  2    Automatic page segmentation, but no OSD, or OCR.
  3    Fully automatic page segmentation, but no OSD. (Default)
  4    Assume a single column of text of variable sizes.
  5    Assume a single uniform block of vertically aligned text.
  6    Assume a single uniform block of text.
  7    Treat the image as a single text line.
  8    Treat the image as a single word.
  9    Treat the image as a single word in a circle.
 10    Treat the image as a single character.

Single options:
  -h, --help            Show this help message.
  --help-psm            Show page segmentation modes.
  -v, --version         Show version information.
  --list-langs          List available languages for tesseract engine.
  --print-parameters    Print tesseract parameters to stdout.
```  

最后就决定自己实现一个简单的接口  

使用方法
```
ocr = Ocr(r'C:\Program Files\Tesseract-OCR', r"e:\Python\pyocr\images\1.png").exec()
print(ocr)
```
对参数解释一下  
```python
def __init__(self, ocr_path, img_path, out_path=None, mode=3, delete=True):
        """
        ocr_path: 
			tesseract 引擎的安装路径，例如我的 r'C:\Program Files\Tesseract-OCR'
        img_path: 
			要解析的图片路径，如 r"e:\Python\pyocr\images\1.png"
        out_path: 
			输出文件路径，如果只是简单为了获取解析出来的数字，可不管，默认地址为 r"D:\result.txt"
        mode: 
			图片的切割模式，参见 tesseract 使用方法，默认为 3
        delete: 
			是否保留生成的文本文件，默认不保存
        """
```  

至于为什么只是数字，是因为英文的总是不能完全解析出来，修改了 -l 参数也是没用，使用其自带的 tessdata 也没用，中文的话解析出来的内容完全看不懂...  （或许是我打开方式不对？）  

效果  

![1.png](https://github.com/chenjiandongx/pyocr/blob/master/images/1.png)
```
C:\Users\54186\Anaconda3\python.exe E:/Python/pyocr/ocr.py
Tesseract Open Source OCR Engine v3.05.00dev with Leptonica
5108
```

![2.jpg](https://github.com/chenjiandongx/pyocr/blob/master/images/2.jpg)
```
C:\Users\54186\Anaconda3\python.exe E:/Python/pyocr/ocr.py
Tesseract Open Source OCR Engine v3.05.00dev with Leptonica
4893
```

![3.png](https://github.com/chenjiandongx/pyocr/blob/master/images/3.png)
```
C:\Users\54186\Anaconda3\python.exe E:/Python/pyocr/ocr.py
Tesseract Open Source OCR Engine v3.05.00dev with Leptonica
130768
```  

**温馨提示：不能保证百分百正确，也不能保证百分百解析得出来。所以项目仅供参考！！！要有保证的话还是找打码平台吧**
