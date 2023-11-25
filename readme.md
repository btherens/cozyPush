# cozyPush: Web Push for PHP

A bare-bones web push implementation written in PHP. Create Web Push push notifications with cozyPush and send them to client browsers with cozyCurl.

### Installing

* copy either/both cozyPush.php and cozyCurl.php into your existing project
* create your web server's vapid keys once by running `vapid.generate` in a bash shell:
```
./vapid.generate;
```

### Example

```
/* website properties */
$basedns = 'your-url.com';

/* web push subscription */
$endpoint = 'https://pushservice.com/endpoint';
$key      = 'userPublicKey-here';
$token    = 'authtoken-here';
$encoding = 'aes128gcm';


/* create a new push message object */
$message = new cozyPush( 'message', $basedns, $endpoint, $key, $token, $encoding );

/* create curl object with callback for 410 status code */
$curl = new cozyCurl( function( $response, $info, $request ) { if ( 410 === $info[ 'http_code' ] ) { ( $request->callback )(); } } );

/* 410 status code callback - implement function to disable subscriptions */
$callback = fn() => dropSubscription( $id );

/* add request to queue */
$curl->request( $endpoint, 'POST', $message->cipherText(), $message->headers(), null, $callback );

/* execute pending curl requests */
$curl->execute();
```

## Acknowledgments

Learn more about Web Push:
* [World Wide Web Consortium](https://www.w3.org/TR/push-api/)
* [web.dev](https://web.dev/articles/push-notifications-web-push-protocol)
