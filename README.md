二维码生成问题修正

问题描述：
    原endroid/qr-code二维码生成组件与phar包不兼容,  参考的错误信息："Invalid logo path: "

    from: endroid/qr-code:3.5.8

解决步骤：

    1. 所有使用endroid/qr-code并且以phar启动的项目请修改项目根目录composer.json, 将原有的"endroid/qr-code": "^1.0"修改为"uniondrug/qr-code": "^1.0"
        "require" : {
               ...
               "uniondrug/qr-code": "^1.0"
        }

    2. 在项目根目录app目录下查找setErrorCorrectionLevel, 将$qrCode->setErrorCorrectionLevel(ErrorCorrectionLevel::HIGH); 修改为 $qrCode->setErrorCorrectionLevel(new ErrorCorrectionLevel(ErrorCorrectionLevel::HIGH));

    3. 上传代码，更新composer，重新生成phar包，并重启服务

---------------------------------------------------------------------------------------

以下内容为：endroid/qr-code:3.5.8原README

QR Code
By endroid

Latest Stable Version Build Status Total Downloads Monthly Downloads License

This library helps you generate QR codes in a jiffy. Makes use of bacon/bacon-qr-code to generate the matrix and khanamiryan/qrcode-detector-decoder for validating generated QR codes. Further extended with Twig extensions, generation routes, a factory and a Symfony bundle for easy installation and configuration.

Installation
Use Composer to install the library.

$ composer require endroid/qr-code
Basic usage
use Endroid\QrCode\QrCode;

$qrCode = new QrCode('Life is too short to be generating QR codes');

header('Content-Type: '.$qrCode->getContentType());
echo $qrCode->writeString();
Advanced usage
use Endroid\QrCode\ErrorCorrectionLevel;
use Endroid\QrCode\LabelAlignment;
use Endroid\QrCode\QrCode;
use Endroid\QrCode\Response\QrCodeResponse;

// Create a basic QR code
$qrCode = new QrCode('Life is too short to be generating QR codes');
$qrCode->setSize(300);

// Set advanced options
$qrCode->setWriterByName('png');
$qrCode->setMargin(10);
$qrCode->setEncoding('UTF-8');
$qrCode->setErrorCorrectionLevel(new ErrorCorrectionLevel(ErrorCorrectionLevel::HIGH));
$qrCode->setForegroundColor(['r' => 0, 'g' => 0, 'b' => 0, 'a' => 0]);
$qrCode->setBackgroundColor(['r' => 255, 'g' => 255, 'b' => 255, 'a' => 0]);
$qrCode->setLabel('Scan the code', 16, __DIR__.'/../assets/fonts/noto_sans.otf', LabelAlignment::CENTER);
$qrCode->setLogoPath(__DIR__.'/../assets/images/symfony.png');
$qrCode->setLogoSize(150, 200);
$qrCode->setRoundBlockSize(true);
$qrCode->setValidateResult(false);
$qrCode->setWriterOptions(['exclude_xml_declaration' => true]);

// Directly output the QR code
header('Content-Type: '.$qrCode->getContentType());
echo $qrCode->writeString();

// Save it to a file
$qrCode->writeFile(__DIR__.'/qrcode.png');

// Create a response object
$response = new QrCodeResponse($qrCode);
QR Code

Built-in validation reader
You can enable the built-in validation reader (disabled by default) by calling setValidateResult(true). This validation reader does not guarantee that the QR code will be readable by all readers but it helps you provide a minimum level of quality.

The readability of a QR code is primarily determined by the size, the input length, the error correction level and any possible logo over the image so you can tweak these parameters if you are looking for optimal results. You can also check $qrCode->getRoundBlockSize() value to see if block dimensions are rounded so that the image is more sharp and readable.

Take note that the validator can consume quite amount of additional resources.

Symfony integration
The endroid/qr-code-bundle integrates the QR code library in Symfony for an even better experience.

Configure your defaults (like image size, default writer etc.)
Generate QR codes quickly from anywhere via the factory service
Generate QR codes directly by typing an URL like /qr-code/<text>.png?size=300
Generate QR codes or URLs directly from Twig using dedicated functions
Read the bundle documentation for more information.

Versioning
Version numbers follow the MAJOR.MINOR.PATCH scheme. Backwards compatibility breaking changes will be kept to a minimum but be aware that these can occur. Lock your dependencies for production and test your code when upgrading.

License
This bundle is under the MIT license. For the full copyright and license information please view the LICENSE file that was distributed with this source code.