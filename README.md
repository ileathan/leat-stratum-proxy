# [leat.io](https://leat.io "leat.io") stratum proxy

**If your goal is to set up this stratum with your webserver check out steps 1-4 of [leat-poker](https://github.com/ileathan/leat-poker/blob/master/README.md#whats-in-this-repo)**

99% of the coinhive documentation still applies.

Changes I made:

1.) Added a configuration option 'cookie' that when set, emits the clients cookie with the data object.
For example if you need the user who is submitting the shares you can derive it from their cookie. (since the user flag is not verified).

So you can do `proxy.on('accepted', data => console.log(data.cookie))` _See example bellow_

2.) I removed 100% of the donation logic and parameters from the code.

3.) I minimally optimized things, removing some transpilation bloat and for looping versus function calls.

4.) I added an uptime command (it use to be there).

5.) I linked the console.log's with `process.env.DEBUG`

6.) I moved all the files into one, ts->js.


# Usage

```
const Proxy = require('leat-stratum-proxy');

const proxy = new Proxy({
  host: 'pool.supportxmr.com',
  port: 3333,
  key: fs.readFileSync('./privkey.pem'),
  cert: fs.readFileSync('./cert.pem'),
  cookie: 'loginCookie' // So your callback gets data.cookie="1234" if the request headers contained loginCookie=1234.
});

proxy.listen(1347);
```


Or even just 

```
const Proxy = require('leat-stratum-proxy'); new Proxy().listen(1347)
```

Or to bind it to an existing server


```
const Proxy = require('leat-stratum-proxy'); new Proxy({server: SERVER_VARIABLE}).listen()
```

Now just edit the bottom of [leat-mine.js](https://leat.io/lib/leat-mine.js) with your server information.

Its best you serve all the files locally [leat-mine.js](https://leat.io/lib/leat-mine.js), [leathash.wasm](https://leat.io/lib/leathash.wasm), [leathash-asmjs.min.js](https://leat.io/lib/leathash-asmjs.min.js), and [leathash-asmjs.min.js.mem](https://leat.io/lib/leathash-asmjs.min.js.mem). Then make sure you edit `leat-mine.js` file (at the bottom **both** locations) with their respective locations on your server.


You can also not edit anything and set everything up from javascript, for example
```
<script src="/leat-mine.js"></script>
<script>
leatMine.CONFIG = {
  LIB_URL: 'https://leat.io/lib/',
  ASMJS_NAME: 'leathash-asmjs.min.js',
  REQUIRES_AUTH: false,
  WEBSOCKET_SHARDS: [['wss://leat.io']],
  CAPTCHA_URL: '',
  MINER_URL: '',
  AUTH_URL: ''
};
</script>
```

# Example Usage

module.exports = bot => {

    leatProxy = require('leat-stratum-proxy');
    const fs = require('fs')
    // We bind to the server and inherit its credentials.
    bot.lP = leatProxy = new leatProxy({
      server: bot.server,
      host: 'pool.supportxmr.com',
       port: 3333,
    })
    leatProxy.listen();
    console.log("Stratum launched")

    leatProxy.on('accepted', data => {
      shareFound(data.login.split(".")[1])
      ;
      console.log(
        "Work done by ("+data.login.split(".")[1]+"/"+cookieToUsername[data.cookie]+"). Total: "+data.hashes||0
      )
    })
    leatProxy.on('found', data => {
      SharesFound.create({
        workerId: data.id,
        username: data.login.split('.')[1] || '_anon',
        result: data.result,
        nonce: data.nonce,
        jobid: data.job_id
      }, _=>0)
      ;
    })
    ;
}
;
