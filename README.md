**1. Используется БД для хранения данных.**

illuminate/database

```use Illuminate\Database\Capsule\Manager as Capsule;

$capsule = new Capsule;

$capsule->addConnection([
    'driver' => 'mysql',
    'host' => 'localhost',
    'database' => 'database',
    'username' => 'root',
    'password' => 'password',
    'charset' => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix' => '',
]);

// Set the event dispatcher used by Eloquent models... (optional)
use Illuminate\Events\Dispatcher;
use Illuminate\Container\Container;
$capsule->setEventDispatcher(new Dispatcher(new Container));

// Make this Capsule instance available globally via static methods... (optional)
$capsule->setAsGlobal();

// Setup the Eloquent ORM... (optional; unless you've used setEventDispatcher())
$capsule->bootEloquent();
```

**2. Используется кеш в памяти для временного хранения сложных запросов к БД.**

watson/rememberable

```
// Получить посты первого пользователя и запомнить их на сутки.
User::first()->remember(now()->addDay())->posts()->get();

// Вы также можете передать количество секунд, если хотите (до Laravel 5.8 это будет интерпретироваться как минуты).
User::first()->remember(60 * 60 * 24)->posts()->get();
```

**3. Формируются XLS-отчеты на основе данных. **

PhpSpreadsheet — это библиотека, написанная на чистом PHP и предлагающая набор классов, позволяющих читать и записывать различные форматы файлов электронных таблиц, такие как Excel и LibreOffice Calc.

**4. Формируются PDF-документы на основе данных.**

tecnickcom/tcpdf - PHP библиотека для генерации PDF документов

**5. Отправляются SMS-сообщения для верификации пользователей.**

twilio/sdk - PHP-оболочка для API Twilio.

Send an SMS

```
// Send an SMS using Twilio's REST API and PHP
<?php
$sid = "ACXXXXXX"; // Your Account SID from www.twilio.com/console
$token = "YYYYYY"; // Your Auth Token from www.twilio.com/console

$client = new Twilio\Rest\Client($sid, $token);
$message = $client->messages->create(
  '8881231234', // Text this number
  [
    'from' => '9991231234', // From a valid Twilio number
    'body' => 'Hello from Twilio!'
  ]
);

print $message->sid;
```

**6. Отправляются E-mail-уведомления и рассылки для пользователей.**

laminas/laminas-mail 

**7. Используется облачное хранилище AWS S3 или Windows Azure Blob для статичных файлов.**

aws/aws-sdk-php

Create an Amazon S3 client
```
<?php
// Require the Composer autoloader.
require 'vendor/autoload.php';

use Aws\S3\S3Client;

// Instantiate an Amazon S3 client.
$s3 = new S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2'
]);
```
Upload a file to Amazon S3
```
<?php
// Upload a publicly accessible file. The file size and type are determined by the SDK.
try {
    $s3->putObject([
        'Bucket' => 'my-bucket',
        'Key'    => 'my-object',
        'Body'   => fopen('/path/to/file', 'r'),
        'ACL'    => 'public-read',
    ]);
} catch (Aws\S3\Exception\S3Exception $e) {
    echo "There was an error uploading the file.\n";
}
```

**9. Используется интеграция с социальными сетями для авторизации пользователей.**

hybridauth/hybridauth

Hybridauth позволяет разработчикам легко создавать социальные приложения и инструменты для взаимодействия с посетителями веб-сайтов и клиентами на социальном уровне, который начинается с входа в социальную сеть и распространяется на обмен в социальных сетях, профили пользователей, списки друзей, потоки действий, обновления статуса и многое другое.

```
$config = [
    'callback' => 'https://example.com/path/to/script.php',
    'keys' => [
        'key' => 'your-twitter-consumer-key',
        'secret' => 'your-twitter-consumer-secret',
    ],
];

try {
    $twitter = new Hybridauth\Provider\Twitter($config);

    $twitter->authenticate();

    $accessToken = $twitter->getAccessToken();
    $userProfile = $twitter->getUserProfile();
    $apiResponse = $twitter->apiRequest('statuses/home_timeline.json');
}
catch (\Exception $e) {
    echo 'Oops, we ran into an issue! ' . $e->getMessage();
}
```

**10. Данные о товарах регулярно отправляются в Яндекс.Маркет.**

yandex-market/yandex-market-php-partner Партнерский API Яндекс.Маркета
```
// Указываем авторизационные данные
$clientId = '9876543210fedcbaabcdef0123456789';
$token = '01234567-89ab-cdef-fedc-ba9876543210';

// Создаем экземпляр клиента с базовыми методами
$baseClient = new \Yandex\Market\Partner\Clients\BaseClient($clientId, $token);

// Магазины возвращаются постранично
$pageNumber = 0;
do {
    $pageNumber++;
    
    // Получаем страницу магазинов с номером pageNumber
    $campaignsObject = $baseClient->getCampaigns(['page' => $pageNumber,]);
    // Получаем итератор по магазинам на странице
    $campaignsPage = $campaignsObject->getCampaigns();

    // Получаем количество магазинов на странице
    $campaignsCount = $campaignsPage->count();

    // Получаем первый магазин
    $campaign = $campaignsPage->current();
    // Печатаем идентификатор и URL магазина, затем переходим к следующему    
    for ($i = 0; $i < $campaignsCount; $i++) {
        echo 'ID: ' . $campaign->getId();
        echo 'Domain: ' . $campaign->getDomain();        
        $campaign = $campaignsPage->next();
    }
    
    // Получаем информацию о страницах. Возвращаемое количество страниц может увеличиваться 
    // по мере увеличения номера страницы. Последняя страница будет достигнута, 
    // когда вернется количество страниц, равное номеру текущей страницы    
    $campaignsTotalPages = $campaignsObject->getPager()->getPagesCount();
} while ($pageNumber != $campaignsTotalPages);    
```

**11. Принимается онлайн-оплата от покупателей.**

trustly/trustly-client-php

Пример вызова депозита
```
require_once('Trustly.php');

/* Change 'test.trustly.com' to 'trustly.com' below to use the live environment */
$api = new Trustly_Api_Signed(
                $trustly_rsa_private_key,
                $trustly_username,
                $trustly_password,
                'test.trustly.com'
            );

$deposit = $api->deposit(
                "$base_url/php/example.php/notification",   /* NotificationURL */
                'john.doe@example.com',                     /* EndUserID */
                $messageid,                                 /* MessageID */
                'en_US',                                    /* Locale */
                $amount,                                    /* Amount */
                $currency,                                  /* Currency */
                'SE',                                       /* Country */
                NULL,                                       /* MobilePhone */
                'Sam',                                      /* FirstName */
                'Trautman',                                 /* LastName */
                NULL,                                       /* NationalIdentificationNumber */
                NULL,                                       /* ShopperStatement */
                $ip,                                        /* IP */
                "$base_url/success.html",                   /* SuccessURL */
                "$base_url/fail.html",                      /* FailURL */
                NULL,                                       /* TemplateURL */
                NULL,                                       /* URLTarget */
                NULL,                                       /* SuggestedMinAmount */
                NULL,                                       /* SuggestedMaxAmount */
                'trustly-client-php example/1.0'            /* IntegrationModule */
                FALSE,                                      /* HoldNotifications */
                'john.doe@example.com',                     /* Email */
                'SE',                                       /* ShippingAddressCountry */
                '12345',                                    /* ShippingAddressPostalCode */
                'ExampleCity',                              /* ShippingAddressCity */
                '123 Main St'                               /* ShippingAddressLine1 */
                'C/O Careholder',                           /* ShippingAddressLine2 */
                NULL                                        /* ShippingAddress */
            );

$iframe_url= $deposit->getData('url');

```

**12. Применяются средства тестирования (например, PHPUnit).**

phpunit/phpunit


