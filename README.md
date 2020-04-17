# Omnipay: Ameria

**Ameria bank payment gateway driver**

[Omnipay](https://github.com/thephpleague/omnipay) is a framework agnostic, multi-gateway payment
processing library for PHP 5.5+. This package implements Ameria support for Omnipay.

## Installation

Omnipay installation is done using [Composer](http://getcomposer.org/). To install, simply add it
to your `composer.json` file:

```json
{
    "require": {
        "tobelyan/omnipay-ameria": "dev-master"
    }
}
```

And run composer to update your dependencies:

    composer update

Or you can simply run

    composer require tobelyan/omnipay-ameria

## Basic Usage

1. Call Omnipay gateway class:

```php
    use Omnipay\Omnipay;
```

1.1 Set your credentials in app.php (located in your conf directory) use code below

```php
    'ameria' => [
        'clientId' => 'YourClientIdHere',
        'username' => 'YourUsernameHere',
        'password' => 'YourPasswordHere',
    ],
```


2. Initialize Ameria gateway:

```php
     $gateway = Omnipay::create('Ameria');
     $gateway->setClientId(config('app.ameria.clientId'));
     $gateway->setUsername(config('app.ameria.username'));
     $gateway->setPassword(config('app.ameria.password'));
     $gateway->setCurrency('AMD'); //or USD, RUB
     $gateway->setTestMode('bool'); // set true or false
     $gateway->setParameter('returnUrl','return url here'); // e.g route('ameria.order.check')
     $gateway->setParameter('lang',\App::getLocale()); //or set am, ru
     $gateway->setParameter('OrderID',"123456"); // orderID from Bank
     $gateway->setParameter('amount',$order_amount); // price of product
     $gateway->setParameter('PaymentId',$order_id); // random generated numbers type int
     $gateway->setParameter('description',$description); // Description for your purchase
     $gateway->setParameter('transactionId',$order_id); // random generated numbers type int
     $purchase = $gateway->purchase()->send();
     if ($purchase->isRedirect()) {
         $purchase->redirect();
     }

```

3. Create a webhook to handle the callback request at your `returnUrl` and catch the webhook

```php

    $gateway = Omnipay::create('Ameria');
    $gateway->setClientId(config('app.ameria.clientId'));
    $gateway->setUsername(config('app.ameria.username'));
    $gateway->setPassword(config('app.ameria.password'));
    
    $purchase = $gateway->completePurchase('paymentID')->send();
    
    if ($purchase->isSuccessful()) {       
        // Your logic     
    }
    
    return new Response('OK');

```
## Information

In this package implemented AmeriaBank integration vPOS 3.0. 

API interaction is performed via data exchange through Rest (except administration page: SOAP) .

For more information read ameria Bank documentation.

##Developed by [DINEURON](https://dineuron.com.com/)

## Support

If you are having general issues with Omnipay, we suggest posting on
[Stack Overflow](http://stackoverflow.com/). Be sure to add the
[omnipay tag](http://stackoverflow.com/questions/tagged/omnipay) so it can be easily found.

If you believe you have found a bug, please report it using the [GitHub issue tracker](https://github.com/tobelyan/omnipay-ameria/issues),
or better yet, fork the library and submit a pull request.