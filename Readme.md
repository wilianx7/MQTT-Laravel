# Laravel MQTT Package

A simple Laravel 5 and 6 Library to connect/publish/subscribe to MQTT broker

Based on [bluerhinos/phpMQTT](https://github.com/bluerhinos/phpMQTT)

For Example see this [repo](https://github.com/salmanzafar949/Laravel-Mqtt-Example)

## Installation
```
composer require salmanzafar/laravel-mqtt
```
## Features

* Name and Password Authentication
* Certificate Protection for end to end encryption
* Enable Debug mode to make it easier for debugging 
* Now you can also set Client_id of your choice and if you don't want just simply don't use or set it to null
* Set QOS flag directly from config file
* Set Retain flag directly from config file

## Enable the package (Optional)

This package implements Laravel auto-discovery feature. After you install it the package provider and facade are added automatically for laravel >= 5.5.

__This step is only required if you are using laravel version <5.5__

To declare the provider and/or alias explicitly, then add the service provider to your config/app.php:

```
'providers' => [

        Salman\Mqtt\MqttServiceProvider::class,
];
```
And then add the alias to your config/app.php:
```
'aliases' => [

       'Mqtt' => \Salman\Mqtt\Facades\Mqtt::class,
];
```
## Configuration
Publish the configuration file
```
php artisan vendor:publish --provider="Salman\Mqtt\MqttServiceProvider"
```
## Config/mqtt.php
```
    'host'     => env('mqtt_host','127.0.0.1'),
    'password' => env('mqtt_password',''),
    'username' => env('mqtt_username',''),
    'certfile' => env('mqtt_cert_file',''),
    'port'     => env('mqtt_port','1883'),
    'debug'    => env('mqtt_debug',false) //Optional Parameter to enable debugging set it to True
    'qos'      => env('mqtt_qos', 0), // set quality of service here
    'retain'   => env('mqtt_retain', 0) // it should be 0 or 1 Whether the message should be retained.- Retain Flag
```
#### Publishing topic

```
use Salman\Mqtt\MqttClass\Mqtt;

public function SendMsgViaMqtt($topic, $message)
{
        $mqtt = new Mqtt();
        $client_id = Auth::user()->id;
        $output = $mqtt->ConnectAndPublish($topic, $message, $client_id);

        if ($output === true)
        {
            return true;
        }

        return false;
}
```
#### Publishing topic using Facade

```
use Mqtt;

public function SendMsgViaMqtt($topic, $message)
{
        $client_id = Auth::user()->id;
        
        $output = Mqtt::ConnectAndPublish($topic, $message, $client_id);

        if ($output === true)
        {
            return true;
        }

        return false;
}
```

#### Subscribing topic

```
use Salman\Mqtt\MqttClass\Mqtt;

public function SubscribetoTopic($topic)
    {
        $mqtt = new Mqtt();
        $client_id = Auth::user()->id;
        $mqtt->ConnectAndSubscribe($topic, function($topic, $msg){
            echo "Msg Received: \n";
            echo "Topic: {$topic}\n\n";
            echo "\t$msg\n\n";
        }, $client_id);


    }
```
#### Subscribing topic using Facade

```
use Mqtt;

public function SubscribetoTopic($topic)
    {
       Mqtt::ConnectAndSubscribe($topic, function($topic, $msg){
            echo "Msg Received: \n";
            echo "Topic: {$topic}\n\n";
            echo "\t$msg\n\n";
        },$client_id);


    }
```

### Tested on php 7.3 and laravel 5.7 and also laravel 5.8
#### Also supports php 7.4