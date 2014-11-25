File Behavior
=============

[![Latest Stable Version](https://poser.pugx.org/claudejanz/yii2-file-behavior/v/stable.svg)](https://packagist.org/packages/claudejanz/yii2-file-behavior) [![Total Downloads](https://poser.pugx.org/claudejanz/yii2-file-behavior/downloads.svg)](https://packagist.org/packages/claudejanz/yii2-file-behavior) [![Latest Unstable Version](https://poser.pugx.org/claudejanz/yii2-file-behavior/v/unstable.svg)](https://packagist.org/packages/claudejanz/yii2-file-behavior) [![License](https://poser.pugx.org/claudejanz/yii2-file-behavior/license.svg)](https://packagist.org/packages/claudejanz/yii2-file-behavior)


Adds file behavior to Active Records

Features
-----

* Upload handeled throuth behaviors
* Rewrite url with model variables
* After Save update model for insert support
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

### In model

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
    }
```

### In Controller

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
    }
```

More about configuration in [FileUploade comments](https://github.com/claudejanz/yii2-file-behavior/blob/master/FileBehavior.php)

Futur developpment plan (when I need them)
-----

* Delete of old files
* Multiple files upload for one field