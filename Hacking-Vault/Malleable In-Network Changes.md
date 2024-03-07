
Beacon can communicate over HTTP(S), we can manipulate the URLs, GET & POST requests, headers, cookies, body, and other elements of the network communication. 

#### 🖊️ Elements

`http-get` : this block defines indicators fot HTTP GET requests

`set uri` : specifies the URI that the client and server will use. Usually, if the server receives a transaction on a URI that does not match its profile, it will automatically return a 404.

`client` : we can add additional parameters that appear after the URI, these are simple key and values. `parameter "utmac" "UA-2202604-2";` would be added to the URI like: `/__utm.gif?utmac=UA-2202604-2`.

`metadata` : When Beacon talks to the Team Server, it has to be identified in some way. This metadata can be transformed and hidden within the HTTP request. As an example, in this [profile](https://github.com/Cobalt-Strike/Malleable-C2-Profiles/blob/master/normal/webbug.profile) we specify the transform - possible values inclue `netbios`, `base64` and `mask` (which is a XOR mask). Then we can append and/or prepend string data. Finally, we specify where in the transaction it will be - this can be a URI `parameter`, a `header` or in the body.
In the webbug profile, the metadata would look something like this: `_utma=GMGLGGGKGKGEHDGGGMGKGMHDGEGHGGGI`.

`server` : this block defines how the response from the team server will look. Any headers are provided first.

`output` : this block dictates how the data being sent by the Team Server will be transformed. Arbitrary data can be appended or prepended before being terminated by the `print` statement.

`http-post`  : this block is exactly the same as `http-get` but for HTTP POST transactions.



### Properties
---
📆 created   {{06-03-2024}} 20:28
🏷️ tags: #redteam #crto 

---

