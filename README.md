# leat.io stratum proxy

100% of the documentation of coinhive applies.

The changes I made are mostly just ripping out the donation logic.


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

```
Proxy = require('./leat-stratum-proxy'); new Proxy().listen(3000)
```


Now just point your regular coinhive.min.js or the modified leatmine.min.js front end miner to the proxy.


For example if your external IP Address is 84.34.112.12 then you would set 

```
leatMine.WEBSOCKET_SHARDS=[["84.34.112.12:3000"]]
```


