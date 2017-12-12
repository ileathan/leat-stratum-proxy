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
Proxy = require('leat-stratum-proxy.js');

proxy = new Proxy({
  host: 'pool.supportxmr.com',
  port: 3333,
  key: fs.readFileSync('./privkey.pem'),
  cert: fs.readFileSync('./cert.pem'),
  cookie: 'loginCookie' // So your callback gets data.cookie="1234" if the request headers contained loginCookie=1234.
});

proxy.listen(3000);
```


Or even just 

```
Proxy = require('./leat-stratum-proxy'); new Proxy().listen(3000)
```


Now just point your regular coinhive.min.js or the modified [leat-mine.js](https://leat.io/leat-mine.js "leat-mine.js") front end miner to the proxy.


For example if your external IP Address is 84.34.112.12 then you would set 

```
leatMine.WEBSOCKET_SHARDS=[["84.34.112.12:3000"]]
```


