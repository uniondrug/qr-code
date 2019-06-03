# QR Code
*From: [endroid/qr-code:3.5.8](https://github.com/endroid/qr-code/blob/3.5.8/README.md)*


## 二维码生成问题修正

### 问题描述：
    原endroid/qr-code二维码生成组件与phar包不兼容,  参考的错误信息："Invalid logo path: "

### 解决步骤：

    1. 所有使用endroid/qr-code并且以phar启动的项目请修改项目根目录composer.json, 将原有的"endroid/qr-code": "^1.0"修改为"uniondrug/qr-code": "^1.0"
        "require" : {
               ...
               "uniondrug/qr-code": "^1.0"
        }

    2. 在项目根目录app目录下查找setErrorCorrectionLevel, 将$qrCode->setErrorCorrectionLevel(ErrorCorrectionLevel::HIGH); 修改为 $qrCode->setErrorCorrectionLevel(new ErrorCorrectionLevel(ErrorCorrectionLevel::HIGH));

    3. 上传代码，更新composer，重新生成phar包，并重启服务
    