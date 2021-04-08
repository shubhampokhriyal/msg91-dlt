- [Installation](#installation)
    - [Supported Framework](#supportedframeworks)
- [Config file setup](#configfilesetup)
- [ SMS API](#smsapi)
    - [SendTransactional & SendPromotional](#1.sendTransactionalsendPromotionalusingget)
    - [SendBulkSms](#2.sendbulksmsusingpost)
- [ OTP API](#otpapi)
    - [SEND OTP](#sendotp)
    - [RESEND OTP](#resendotp)
    - [VERIFY OTP](#verifyotp)

### Installation

Run the following command to install the latest applicable version of the package:

```sh
composer require shubhampokhriyal/msg91-dlt
```
```sh
"require": {
        "shubhampokhriyal/msg91-dlt": "dev-master"
        }
```
## Supported FrameWorks

- `Laravel5.3`, `Laravel5.4`, `laravel5.5` we are suggested Laravel frame work
- `Symfony 3.1`, `Slim 3.8 `, `Zend 3.0`, `Codeigniter 3.1`

## Config file setup

- Config file Now, using only for send Authkey.

`Three ways send Authkey`

Tips 1: `Using Config`

```sh
'msg91' => [
    'common' => [
        'transAuthKey' =>  "17086...........9a87a1",
        'promoAuthKey' =>  "17086...........9a87a1",
        'otpAuthKey'   =>  "17043...........59969531",
    ],
]
``` 
Tips 2: `Using Class parameter`

```sh
use Sender\PromotionalSms;
use Sender\TransactionalSms;

$sms = new PromotionalSms("17043...........59969531");
$sms->PromotionalSms("919******541,919******728",$sample);

$sms = new TransactionalSms("17043...........59969531");
$sms->sendTransactional("919******541,919******728",$sample);

```
Tips 3: `Dot Env File`

```sh
TRANSAUTHKEY=170***************a87a1
OTPAUTHKEY=1704***************531
```
### Coding Standards

The entire library is intended to be PSR-1, PSR-2 compliant.

### SMS
 [Msg91 Send SMS](http://api.msg91.com/apidoc/textsms/send-sms.php) 
- `GET` Method
- `POST` Method

```sh
 GET
http://api.msg91.com/api/sendhttp.php?authkey=YourAuthKey&mobiles=919999999990,919999999999&message=message&sender=ABCDEF&route=4&country=0
```

| Parameter name | Data type   | Description
| -------------- | ---------   | -----------
| authkey *   | alphanumeric  |Login authentication key (this key is unique for every user)
| mobiles *   | integer   | Keep numbers in international format (with country code), multiple numbers should be | separated by comma (,)
| message *   | varchar   | Message content to send
| sender *    | varchar   | Receiver will see this as sender's ID.
| DLT_TE_ID *   | varchar   | DLT Template ID of a message.
| route *   |   varchar   | If your operator supports multiple routes then give one route name. Eg: route=1 for promotional, route=4 for transactional SMS.
| country   |   numeric   | 0 for international,1 for USA, 91 for India.
| flash     | integer     | (0/1) flash=1 (for flash SMS)
| unicode   |   integer   |   (0/1) unicode=1 (for unicode SMS)
| schtime   | date and time |When you want to schedule the SMS to be sent. Time format could be of your choice you can use Y-m-d h:i:s (2020-01-01 10:10:00) Or Y/m/d h:i:s (2020/01/01 10:10:00) Or you can send unix timestamp (1577873400)
| afterminutes  | integer   | Time in minutes after which you want to send sms.
| response    |   varchar   | By default you will get response in string format but you want to receive in other format (json,xml) then set this parameter. for example: &response=json or &response=xml
| campaign    | varchar   |   Campaign name you wish to create.

operator supports.

  - route=1 for promotional   
  - route=4 for transactional

# SMS API

## 1. SendTransactional & SendPromotional Using GET
- `GET` Method

### Input Data
- `$mobileNumber` 
   "919******541,919******728"  String 
   9195********3                Integer
- `$data`                       Array

### Sample Input Data

```sh
Tips 1:
$sample = [ 
    'message'      => 'WELCOME TO Msg91',
    'sender'       => 'UTOOWE',
    'DLT_TE_ID'    => '1307161443204186459',
    'country'      => 91,
    'flash'        => 1,
    'unicode'      => 1,
    'schtime'      => "2020-01-01 10:10:00",
    'response'     => "json",
    'afterminutes' => 10,
    'campaign'     => "SHUBHA"
];
use Sender\PromotionalSms;
use Sender\TransactionalSms;

$sms = new PromotionalSms();
$sms->PromotionalSms("919******541,919******728",$sample);

$sms = new TransactionalSms();
$sms->sendTransactional("919******541,919******728",$sample);

Tips 2:
$sample = [ 
    'message'      => 'WELCOME TO Msg91',
    'sender'       => 'TOOME',
    'DLT_TE_ID'    => '1307161443204186459'
    'country'      => 91,
    'flash'        => 1,
    'unicode'      => 1,
    'schtime'      => "2020-01-01 10:10:00",
    'response'     => "json",
    'afterminutes' => 10,
    'campaign'     => "SHUBHA"
];

use Sender\PromotionalSms;
use Sender\TransactionalSms;

$sms = new PromotionalSms();
$sms = new TransactionalSms();

$sms->sendTransactional(919******541,$sample);
$sms->PromotionalSms(919******541,$sample);

```
### API
```sh
use Sender\TransactionalSms;
$sms = new TransactionalSms();
$sms->sendTransactional($mobileNumber, $data);
```
```sh
use Sender\PromotionalSms;
$sms = new PromotionalSms();
$sms->sendPromotional($mobileNumber, $data);
```
## 2. SendBulkSms Using POST
- `POST` Method

### Input Data
- `$data`  Array

### Sample Input Data

```sh
Tips 1: 
$sample = [
    [
        'authkey' => '17086************k599a87a1',
        'sender'  => 'MULSMS',
        'DLT_TE_ID'    => '1307161443204186459'
        'schtime'=> '2016-03-31 11:17:39',
        'campaign'=> 'SHUBHA',
        'DLT_TE_ID' => 'DLT_TEMPLATE_ID',
        'country'=> '91',
        'flash'=> 1,
        'unicode'=> 1,
        'content' =>[ 
           [
           'message'    => 'welcome multi sms',
           'mobile' => '91951******1,91880******4,917******972'
           ],
           [
              'message'    => 'shubham world',
              'mobile' => '9195******41,918******824,917******972'
           ]
        ]
    ]  
];        
Tips 2:
$sample = [
    [
       'authkey' => '17086************k599a87a1',
       'sender'  => 'MULSMS',
       'DLT_TE_ID'    => '1307161443204186459'
       'content' =>[ 
            [
                'message'    => 'welcome multi sms',
                'mobile' => '919******541,918******824,917******972'
            ],
            [
                'message'    => 'shubham world',
                'mobile' => '9195******41,91880******4,9170******72'
            ]
        ]
    ],
    [
       'authkey' => '17086************k599a87a1',
       'sender'  => 'SUNSMS',
       'DLT_TE_ID'    => '1307161443204186459'
       'content' =>[ 
            [
                'message'    => 'hi how are u',
                'mobile' => '9195******41,918******824,9******2972'
            ],
            [
                'message'    => 'hai welcome',
                'mobile' => '9195******41,918******824,9******42972'
            ]
        ]
    ]
];
```
 
### API

```sh
use Sender\PromotionalSms;
$sms = new PromotionalSms();
$sms->sendBulkSms($data);
```

# Sample XML

```sh
<?xml version="1.0"?>
<MESSAGE>
  <AUTHKEY>17086************k599a87a1</AUTHKEY>
  <SENDER>MULSMS</SENDER>
  <DLT_TE_ID>12312312312</DLT_TE_ID>
  <SMS TEXT="welcome multi sms">
    <ADDRESS TO="919******541"/>
  </SMS>
  <SMS TEXT="shubham world">
    <ADDRESS TO="919******541"/>
  </SMS>
</MESSAGE>
```

# Sample Output
```sh
5134842646923e183d000075
```
>Note : Output will be request Id which is alphanumeric and contains 24 character like mentioned above. With this request id, delivery report can be viewed. If request not sent successfully, you will get the appropriate error message. View error codes

# OTP API

[Msg91 Send OTP](http://api.msg91.com/apidoc/sendotp/send-otp.php)


## SEND OTP
- `GET` Method
```sh
GET
http://api.msg91.com/api/sendotp.php?authkey=YourAuthKey&mobile=919999999990&message=Your%20otp%20is%202786&sender=senderid&otp=2786
```

|  Parameter name |   Data type|    Description|
|------------- |-----------------|-----------------|
|  authkey *  |  alphanumeric|    Login authentication key (this key is unique for every user)
|  mobile *   |  integer    |  Keep number in international format (with country code)
|  message    |  varchar    |  Message content to send. (default : Your verification code is ##OTP##.)
|  sender   |  varchar    |  Receiver will see this as sender's ID. (default : OTPSMS)
|  otp      |   integer   |  OTP to send and verify. If not sent, OTP will be generated.
|  otp_expiry |  integer    |  Expiry of OTP to verify, in minutes (default : 1 day, min : 1 minute)
|  otp_length |  integer    |  Number of digits in OTP (default : 4, min : 4, max : 9)

### Input Data
 - `authkey` *     alphanumeric 
 - `mobile` *    Integer 
 - `message`     varchar  
 - `sender`      varchar
 - `DLT_TE_ID`   varchar
 - `otp`       Integer 
 - `otp_expiry`    Integer
 - `otp_length`    Integer

### Sample Input Data
```sh
$data = [
    'message'       => "Your verification code is ##5421##",
    'sender'        => "SHUBHA",
    'DLT_TE_ID'    => '1307161443204186459'
    'otp'           => 5421,
    'otp_expiry'    => 20,
    'otp_length'    => 4
];
```
### API

```sh
use Sender\Otp;

$otp = new Otp();
$otp->sendOtp($mobile,$data);
```
## Sample Output

```sh
{"message":"3763646c3058373530393938","type":"success"}
```
## RESEND OTP
- `GET` Method
```sh
http://api.msg91.com/api/retryotp.php?authkey=YourAuthKey&mobile=919999999990&retrytype=voice
```
 | Parameter name  | Data type |  Description|
 | --------------    | --------- | ------------|
 | authkey *   | alphanumeric |   Login authentication key (this key is unique for every user)
 | mobile *    | integer    |   Keep number in international format (with country code)
 | retrytype   | varchar    |   Method to retry otp : voice or text. Default is voice.

### Input Data
- `$mobile`   Integer
- `$retrytype` String

### Sample Input Data
```sh
use Sender\Otp;

$otp = new Otp();

$otp->resendOtp($mobile,"voice")
$otp->resendOtp($mobile,"text")
$otp->resendOtp($mobile)
```

### API
```sh
use Sender\Otp;

$otp = new Otp();
$otp->resendOtp($mobile,$retrytype)
```

Sample Output
```sh
{"message":"otp_sent_successfully","type":"success"}
```
## VERIFY OTP
- `GET` Method

```sh
http://api.msg91.com/api/verifyRequestOTP.php?authkey=YourAuthKey&mobile=919999999990&otp=2786
```
 | Parameter name | Data type | Description|
 | -------------- | ----------| ------------|
 | authkey *   | alphanumeric |   Login authentication key (this key is unique for every user)
 | mobile *    | integer    | Keep number in international format (with country code)
 | otp *     | varchar    | OTP to verify

### Input Data
- `$mobile` Integer
- `$otp`     string

### Sample Input Data
OtpSend::verifyOtp(919*******41,9195****421);

### API
```sh
use Sender\Otp;

$otp = new Otp();
$otp->verifyOtp($mobile,$otp);
```

Sample Output
```sh
{"message":"number_verified_successfully","type":"success"}
```


### NOTE:

```sh
- Mobile number not attached `+` sign like this `+9195*****41`
```

### Get in touch

If you have any suggestions, feel free to email me at shubhampokhriyal@hotmail.com.
