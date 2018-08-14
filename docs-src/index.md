# RediSearch-PHP

RediSearch-PHP is a PHP client library for the [RediSearch](http://redisearch.io/) module which adds full-text search to Redis.

## Requirements

* Redis running with the [RediSearch module loaded](http://redisearch.io/Quick_Start/).
* PHP >=7
* [PhpRedis](https://github.com/phpredis/phpredis), [Predis](https://github.com/nrk/predis), or [php-redis-client](https://github.com/cheprasov/php-redis-client).

## Install

```bash
composer install ethanhann/redisearch-php
```

## Load

```php-inline
require_once 'vendor/autoload.php';
```


## Create a Redis Client

```php-inline
use Ehann\RediSearch\Redis\PredisAdapter;
use Ehann\RediSearch\Redis\PhpRedisAdapter;
use Ehann\RediSearch\Redis\RedisClientAdapter;

$redis = (new PredisAdapter())->connect('127.0.0.1', 6379);
// or
$redis = (new PhpRedisAdapter())->connect('127.0.0.1', 6379);
// or
$redis = (new RedisClientAdapter())->connect('127.0.0.1', 6379);

```

## Create the Schema

```php-inline
use Ehann\RediSearch\Index;

$bookIndex = new Index($redis);
$bookIndex->setIndexName('book_index')

$bookIndex->addTextField('title')
    ->addTextField('author')
    ->addNumericField('price')
    ->addNumericField('stock')
    ->create();
```

## Add a Document

```php-inline
$bookIndex->add([
    new TextField('title', 'Tale of Two Cities'),
    new TextField('author', 'Charles Dickens'),
    new NumericField('price', 9.99),
    new NumericField('stock', 231),
]);
```

## Search the Index

```php-inline
//in somewhere else you might need to search
$bookIndex=new Index($your_redis_instance);
$bookIndex->setIndexName('book_index');

$result = $bookIndex->search('two cities');

$result->getCount();     // Number of documents.
$result->getDocuments(); // Array of matches.

// Documents are returned as objects by default.
$firstResult = $result->getDocuments()[0];
$firstResult->title;
$firstResult->author;
```

