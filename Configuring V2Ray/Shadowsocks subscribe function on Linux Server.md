Subscribe function has been supported by many anti-censorship clients like Shadowrocket, V2rayN etc. Users are only required subscribe certain URL to import batch of configurations and update it at any time. Here will introduce how to implement subscribe function at your server (it even ok if you don't have a server, you can also upload it onto other websites to implement the function)

1: Configuration format
---
The subscribed URL could be http or https. It returns a text string encoded in **base64**, with a lists of configurations again encoded in **base64**. 


### The format of v2ray:
V2ray configuration is written in JSON format.
```json
{  
"ps": "some name",  
"add": "111.111.111.111",  
"port": "32000",  
"id": "1386f85e-657b-4d6e-9d56-78badb75e1fd",  
"aid": "100",  
"net": "tcp",  
"type": "none",  
"host": "www.bbb.com",
"tls": "none"
}
```
Field Name 	| Meaning
------				|-----
ps					| given name of server
add					| server address
net					| network protocol (tcp/kcp/ws)
type				| camouflage type (none\http\srtp\utp\wechat-video)
host					| camouflaged host
Some fields above like type and host can be omitted

### The format of shadowsocks
Shadowsocks and ShadowsocksR configuration is written in specific format:
shadowsocks:
```
# format: 
method:password@server:port
# example:
aes-256-cfb:123pass@123.123.123.123:37123
```
shadowsocksR:
```server:port:protocol:method:obfs:password_base64/?suffix_base64```

Note that `suffix_base64` part is again encoded in base64. It contains parts like `obfuscation parameters`, `protocol parameters`, `remarks` etc. Before encoded, it is like this:
```
obfsparam=obfsparam_base64&protoparam=protoparam_base64&remarks=remarks_base64&group=group_base64
```

2: Encoding mechanism
---
First we encode all your configurations into base64
```
cat [your config] | base64 >> config64.txt
```
Now the config64.txt file is like something below, this contains two configurations separated by newline symbol `\n` (of course your configuration should be much longer than mine):
```
ewoicHMiOiAibXkgZXhhbXBsZSIKfQo=
ewoicHMiOiAieW9vIiwKImJhbGFiYWxhIjogInNlYW4iCn0K
```

Then We add protocol type of them at the start of each line:
```
vmess://ewoicHMiOiAibXkgZXhhbXBsZSIKfQo=
ss://ewoicHMiOiAieW9vIiwKImJhbGFiYWxhIjogInNlYW4iCn0K
```

And encode it with base64 again:
```
cat config64.txt | base64 > final.txt
```

Now we have finally finished generating the sharing text. 

3: Put the sharing text on web
---
There are number of ways you can upload this text onto internet. You can use [netlify](https://app.netlify.com), [Github Gist](https://gist.github.com) or you own linux server.  After posting the sharing text onto internet, you can type the URL with http:// or https:// protocol to access the subscription. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM1NTIzNzYzNF19
-->