# yii2-gii-rest

Yii2 Code generator for REST api 

## Usage

Config gii modules in your application like this in `backend\config\main-local.php`

```php
<?php

$config = [
    'components' => [
        'request' => [
            // !!! insert a secret key in the following (if it is empty) - this is required by cookie validation
            'cookieValidationKey' => '7Zahf41C4AMjxPsFuKs7m7rDwXrb-vNL',
        ],
    ],
];

if (!YII_ENV_TEST) {
    // configuration adjustments for 'dev' environment
    $config['bootstrap'][] = 'debug';
    $config['modules']['debug'] = [
        'class' => 'yii\debug\Module',
        'allowedIPs' => ['*'],
    ];

    $config['bootstrap'][] = 'gii';
    $config['modules']['gii'] = [
        'class' => 'yii\gii\Module',
        'allowedIPs' => ['*'],
        'generators' => [
            'api-rest' => [
                'class' => 'graychen\yii2\gii\rest\Generator',
                'templates' => [
                    'rest' => 'graychen\yii2\gii\rest\default'
                ]
            ]
        ]
    ];
}

return $config;
```
