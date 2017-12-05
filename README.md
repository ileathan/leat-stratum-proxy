# [leat.io](https://leat.io "leat.io") stratum proxy

99% of the coinhive documentation still applies.

Changes I made:

1.) Added a configuration option 'cookie' that when set, emits the clients cookie with the data object.
For example if you need to user who is submitting the shares you can derive it from their cookie.

So you can do `proxy.on('accepted', data => console.log(data.cookie))`

2.) I removed 100% of the donation logic and parameters from the code.

3.) I minimally optimized things but removing some transpilation bloat and for looping versus function calls.

4.) I moved all the files into one, and then minified it for convenience.


# Usage

```
Proxy = require('leat-stratum-proxy.js');

proxy = new Proxy({
  host: "pool.supportxmr.com",
  port: 3333,
  key: fs.readFileSync("./privkey.pem"),
  cert: fs.readFileSync("./cert.pem")
});

proxy.listen(3000);
```


Or even just 

````
Proxy = require('./leat-stratum-proxy'); new Proxy().listen(3000)
```


Now just point your regular coinhive.min.js or the modified [leatMine.js](https://leat.io/leatMine.js "leatMine.js") front end miner to the proxy.


For example if your external IP Address is 84.34.112.12 then you would set 

```
leatMine.WEBSOCKET_SHARDS=[["84.34.112.12:3000"]]
```


