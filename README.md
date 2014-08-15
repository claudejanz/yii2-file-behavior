File Behavior
=============
Adds file behavior to Active Records

Features
-----

* Upload handeled throuth behaviors
* Rewrite url with model variables
* After Save update url
* skipOnEmpty support 

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist claudejanz/yii2-file-behavior "*"
```

or add

```
"claudejanz/yii2-file-behavior": "*"
```

to the require section of your `composer.json` file.


Usage
-----

Once the extension is installed, simply use it in your code by  :

In model

```php
    public function rules() {
        return array_merge(parent::rules(), [
            [['image'], 'file', 'extensions' => 'jpg'],
            [['image2'], 'file', 'extensions' => 'jpg'],
        ]);
    }

    public function behaviors() {
        return [
            'image' => [
                'class' => \claudejanz\fileBehavior\FileBehavior::className(),
                'paths' => '@webroot/images/all/{id}/',
            ],
        ];
    }```

In Controller

```php
    public function actionCreate() {
        $model = new Vin;

        if ($model->loadWithFiles(Yii::$app->request->post()) && $model->save()) {

            return $this->redirect(['view', 'id' => $model->id]);
        } else {
            return $this->render('create', [
                        'model' => $model,
            ]);
        }
    }

    public function actionUpdate($id) {
        /* @var $model Vin */
        $model = $this->model;
        if ($model->loadWithFiles(Yii::$app->request->post()) && $model->save()) {

            return $this->redirect(['view', 'id' => $model->id]);
        } else {
            return $this->render('update', [
                        'model' => $model,
            ]);
        }
    }```