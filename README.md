# jQuery TreeGrid Extension for Yii 2

This is the [jQuery TreeGrid](https://github.com/maxazan/jquery-treegrid) extension for Yii 2. It encapsulates TreeGrid component in terms of Yii widgets,and thus makes using TreeGrid component in Yii applications extremely easy.The project was established because the original warehouse was unstable.

[![Latest Stable Version](http://poser.pugx.org/hjp1011/yii2-treegrid/v)](https://packagist.org/packages/hjp1011/yii2-treegrid) [![Total Downloads](http://poser.pugx.org/hjp1011/yii2-treegrid/downloads)](https://packagist.org/packages/hjp1011/yii2-treegrid) [![Latest Unstable Version](http://poser.pugx.org/hjp1011/yii2-treegrid/v/unstable)](https://packagist.org/packages/hjp1011/yii2-treegrid) [![License](http://poser.pugx.org/hjp1011/yii2-treegrid/license)](https://packagist.org/packages/hjp1011/yii2-treegrid) [![PHP Version Require](http://poser.pugx.org/hjp1011/yii2-treegrid/require/php)](https://packagist.org/packages/hjp1011/yii2-treegrid)
## Installation

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist hjp1011/yii2-treegrid "*"
```

or add

```
"hjp1011/yii2-treegrid": "*"
```

to the require section of your `composer.json` file.

## Usage

**Model**

```php

use yii\db\ActiveRecord;

/**
 * @property string $description
 * @property integer $parent_id
 */
class Tree extends ActiveRecord 
{

    /**
     * @inheritdoc
     */
    public static function tableName()
    {
        return 'tree';
    }  
    
    /**
     * @inheritdoc
     */
    public function rules()
    {
        return [
            [['description'], 'required'],
            [['description'], 'string'],
            [['parent_id'], 'integer']
        ];
    }
}
```

**Controller**

```php
use yii\web\Controller;
use Yii;
use yii\data\ActiveDataProvider;

class TreeController extends Controller
{

    /**
     * Lists all Tree models.
     * @return mixed
     */
    public function actionIndex()
    {
        $query = Tree::find();
        $dataProvider = new ActiveDataProvider([
            'query' => $query,
            'pagination' => false
        ]);

        return $this->render('index', [
            'dataProvider' => $dataProvider
        ]);
    }
```

**View**

```php
use yiiframe\treegrid\Auth;
  
<?= Auth::widget([
    'dataProvider' => $dataProvider,
    'keyColumnName' => 'id',
    'parentColumnName' => 'parent_id',
    'parentRootValue' => '0', //first parentId value
    'pluginOptions' => [
        'initialState' => 'collapsed',
    ],
    'columns' => [
        'name',
        'id',
        'parent_id',
        ['class' => 'yii\grid\ActionColumn']
    ]     
]); ?>
```

## Adding resources

When is necessary  to add other resource files, then should be used the [Dependency Injection](http://www.yiiframework.com/doc-2.0/guide-concept-di-container.html#registering-dependencies) concept.

To use the `saveState` option it's necessary to add `jquery.cookie.js`.

```php
//config/web.php
  
$config = [
  'id' => 'my-app',
  'components' => [
    ...
  ]
  ...
]

Yii::$container->set('yiiframe\treegrid\TreeGridAsset',[
    'js' => [
        'js/jquery.cookie.js',
        'js/jquery.treegrid.min.js',
    ]
]);

return $config;
```



