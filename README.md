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


## Screen Shoot

![](http://ww1.sinaimg.cn/large/675eb504gy1fte8s7yw7sj21sq0x2n2i.jpg)


## Code Demo

Here is the Controller generated via the User model: 

```php
<?php

namespace api\modules\v1\client;

use Yii;
use common\models\User;
use yii\data\ActiveDataProvider;
use yii\rest\ActiveController;
use yii\web\NotFoundHttpException;
use yii\filters\VerbFilter;

/**
 * UserController implements the CRUD actions for User model.
 */
class UserController extends ActiveController
{
    public $modelClass = 'common\models\User';

    public function actions()
    {
       return [];
    }

    /**
     * Lists all User models.
     * @return mixed
     */
    public function actionIndex()
    {
        $dataProvider = new ActiveDataProvider([
            'query' => User::find(),
        ]);

        return $dataProvider;
    }

    /**
     * Displays a single User model.
     * @param integer $id
     * @return mixed
     * @throws NotFoundHttpException if the model cannot be found
     */
    public function actionView($id)
    {
        return $this->findModel($id),
    }

    /**
     * Creates a new User model.
     * @return mixed
     */
    public function actionCreate()
    {
        $model = new User();

        if ($model->load(Yii::$app->getRequest()->getBodyParams(), '') && $model->save()) {
          $response = Yii::$app->getResponse();
          $response->setStatusCode(201);
        } elseif (!$model->hasErrors()) {
          throw new yii\web\ServerErrorHttpException('Failed to create the object for unknown reason.');
        }
        return $model;
    }

    /**
     * Updates an existing User model.
     * @param integer $id
     * @return mixed
     * @throws NotFoundHttpException if the model cannot be found
     */
    public function actionUpdate($id)
    {
        $model = $this->findModel($id);

        if ($model->load(Yii::$app->request->post()) && $model->save()) {
            Yii::$app->response->setStatusCode(200);
        } elseif (!$model->hasErrors()) {
            throw new ServerErrorHttpException('Failed to update the object for unknown reason.');
        }
            return $model;
     }

    /**
     * Deletes an existing User model.
     * @param integer $id
     * @return mixed
     * @throws NotFoundHttpException if the model cannot be found
     */
    public function actionDelete($id)
    {
        $model = new User();
        if ($this->findModel($id)->delete()) {
            Yii::$app->getResponse()->setStatusCode(204);
        } elseif (!$model->hasErrors()) {
            throw new ServerErrorHttpException('Failed to delete the object for unknown reason.');
        }
        return $model;
    }

    /**
     * Finds the User model based on its primary key value.
     * If the model is not found, a 404 HTTP exception will be thrown.
     * @param integer $id
     * @return User the loaded model
     * @throws NotFoundHttpException if the model cannot be found
     */
    protected function findModel($id)
    {
        if (($model = User::findOne($id)) !== null) {
            return $model;
        }

        throw new NotFoundHttpException('The requested patient does not exist.');
    }
}
```
