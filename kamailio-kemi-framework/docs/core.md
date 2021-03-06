## Core KEMI Functions ##

The exports to KEMI framework from the core of Kamailio:

  * several functions directly to `KSR` module (like `KSR.function(params)`), which are mostly
  the main function from the core and for writing log messages (some being part of xlog module for
  native kamailio.cfg language)
  * the `KSR.hdr` submodule, which are the most used functions for managing SIP message headers

Example of using KEMI functions exported to Lua interpreter:

```Lua
KSR.dbg("a debug message from Lua script\n");
KSR.hdr.remove("Route");
```
Exported functions from core directly to KSR module or KSR.hdr submodule are listed in the next sections.

### KSR.add_local_rport() ###

`bool add_local_rport()`

Set the internal flag to add rport parameter to local generated Via header.

### KSR.add_tcp_alias() ###

`add_tcp_alias(int port)`

Adds a tcp port alias for the current connection (if tcp). Useful if you want to send all the trafic to port_alias through the same connection this request came from (it could help for firewall or nat traversal). When the `aliased` connection is closed (e.g. it is idle for too much time), all the port aliases are removed.

### KSR.add_tcp_alias_via() ###

`int add_tcp_alias_via()`

Adds the port from the message via as an alias to TCP connection. See `add_tcp_alias(int port)` for more details.

### void KSR.dbg(...) ###

`void KSR.dbg(str "msg")`

Write a log message to DEBUG level.

Example:

```
KSR.dbg("message from embedded interpreter\n");
```

### void KSR.err(...) ###

`void KSR.err(str "msg")`

Write a log message to ERROR level.

### KSR.force_rport() ###

`bool force_rport()`

Add rport parameter to the top Via of the incoming request and sent the
SIP response to source port.

### void KSR.info(...) ###

`void KSR.info(str "msg")`

Write a log message to INFO level.

### KSR.is_method() ###

`bool is_method(str "vmethod")`

Return true if the value of the parameter matches the method type of the SIP
message.

```JavaScript
if(KSR.is_method("INVITE")) {
  ...
}
```

### KSR.is_method_in() ###

`bool is_method_in(str "vmethod")`

Return true if SIP method of the currently processed message is matching one
of the corresponding characters given as parameter.

Matching the method is done based on corresponding characters:

  * `I` - INVITE
  * `A` - ACK
  * `B` - BYE
  * `C` - CANCEL
  * `R` - REGISTER
  * `M` - MESSAGE
  * `O` - OPTIONS
  * `S` - SUBSCRIBE
  * `P` - PUBLISH
  * `N` - NOTIFY
  * `U` - UPDATE
  * `K` - KDMQ
  * `G` - GET
  * `T` - POST
  * `V` - PUT
  * `D` - DELETE

Example:

```Lua
if KSR.is_method_in("IABC") then
  -- the method is INVITE, ACK, BYE or CANCEL
  ...
end
```

### KSR.is_INVITE() ###

`bool is_INVITE()`

Return true if the method type of the SIP message is `INVITE`.

```Lua
if KSR.is_INVITE() then
  ...
end
```

### KSR.is_ACK() ###

`bool is_ACK()`

Return true if the method type of the SIP message is `ACK`.

### KSR.is_BYE() ###

`bool is_BYE()`

Return true if the method type of the SIP message is `BYE`.

### KSR.is_CANCEL() ###

`bool is_CANCEL()`

Return true if the method type of the SIP message is `CANCEL`.

### KSR.is_REGISTER() ###

`bool is_REGISTER()`

Return true if the method type of the SIP message is `REGISTER`.

### KSR.is_MESSAGE() ###

`bool is_MESSAGE()`

Return true if the method type of the SIP message is `MESSAGE`.

### KSR.is_SUBSCRIBE() ###

`bool is_SUBSCRIBE()`

Return true if the method type of the SIP message is `SUBSCRIBE`.

### KSR.is_PUBLISH() ###

`bool is_PUBLISH()`

Return true if the method type of the SIP message is `PUBLISH`.

### KSR.is_NOTIFY() ###

`bool is_NOTIFY()`

Return true if the method type of the SIP message is `NOTIFY`.

### KSR.is_OPTIONS() ###

`bool is_OPTIONS()`

Return true if the method type of the SIP message is `OPTIONS`.

### KSR.is_INFO() ###

`bool is_INFO()`

Return true if the method type of the SIP message is `INFO`.

### KSR.is_UPDATE() ###

`bool is_UPDATE()`

Return true if the method type of the SIP message is `UPDATE`.

### KSR.is_PRACK() ###

`bool is_PRACK()`

Return true if the method type of the SIP message is `PRACK`.

### KSR.is_myself(...) ###

`bool KSR.is_myself(str "uri")`

Return true of the URI address provided as parameter matches a local socket (IP)
or local domain.

### KSR.is_myself_furi() ###

`bool is_myself_furi()`

Return true if the URI in From header matches a local socket (IP) or local
domain.

### KSR.is_myself_ruri() ###

`bool is_myself_ruri()`

Return true if the R-URI matches a local socket (IP) or local
domain.

### KSR.is_myself_turi() ###

`bool is_myself_turi()`

Return true if the URI in To header matches a local socket (IP) or local
domain.

### void KSR.log(...) ###

`void KSR.log(str "level", str "msg")`

Write a log message specifying the level value. The level parameter can be:

  * "dbg"
  * "info"
  * "warn"
  * "crit"
  * "err"

If level value is not matched, then "err" log level is used.

Example:

```
KSR.log("dbg", "message from: " + KSR.pv.getw("$si") + "\r\n");
```

### KSR.setflag(...) ###

`bool KSR.setflag(int flag)`

Set the SIP message/transaction flag at the index provided by the parameter. The flag parameter
has to be a number from 0 to 31.

```
KSR.setflag(10);
```

### KSR.resetflag(...) ###

`bool KSR.resetflag(int flag)`

Reset the SIP message/transaction flag at the index provided by the parameter. The flag parameter
has to be a number from 0 to 31.

```
KSR.resetflag(10);
```

### KSR.isflagset(...) ###

`bool KSR.isflagset(int flag)`

Return true if the message/transaction flag at the index provided by the parameter is set
(the bit has value 1).

```
if ( KSR.isflagset(10) ) ...
```

### KSR.setbflag(...) ###

`bool KSR.setbflag(int flag)`

Set the branch flag at the index provided by the parameter. The flag parameter
has to be a number from 0 to 31.

```
KSR.setbflag(10);
```

### KSR.resetbflag(...) ###

`bool KSR.resetbflag(int flag)`

Reset the branch flag at the index provided by the parameter. The flag parameter
has to be a number from 0 to 31.

```
KSR.resetbflag(10);
```

### KSR.isbflagset(...) ###

`bool KSR.isbflagset(int flag)`

Return true if the branch flag at the index provided by the parameter is set
(the bit has value 1).

```
if ( KSR.isbflagset(10) ) ...
```

### KSR.setbiflag(...) ###

`bool KSR.setbiflag(int flag, int branch)`

Set the flag at the index provided by the first parameter to the branch number
specified by the second parameter. The flag parameter has to be a number from
0 to 31. The branch parameter should be between 0 and 12 (a matter of
`max_branches` global parameter).

```
KSR.setbiflag(10, 2);
```

### KSR.resetbiflag(...) ###

`bool KSR.resetbiflag(int flag, int branch)`

Reset a branch flag by position and branch index.

### KSR.isbiflagset(...) ###

`bool KSR.isbiflagset(int flag, int branch)`

Test if a branch flag is set by position and branch index.

### KSR.setsflag(...) ###

`bool KSR.setsflag(int flag)`

Set a script flag.

### KSR.resetsflag(...) ###

`bool KSR.resetsflag(int flag)`

Reset a script flag.

### KSR.issflagset(...) ###

`bool KSR.issflagset(int flag)`

Test if a script flag is set.

### KSR.seturi(...) ###

`bool KSR.seturi(str "uri")`

Set the request URI (R-URI).

Example:

```
KSR.seturi("sip:alice@voip.com");
```

### KSR.setuser(...) ###

`bool KSR.setuser(str "user")`

Example:

```
KSR.setuser("alice");
```

### KSR.sethost(...) ###

`bool KSR.sethost(str "host")`

Example:

```
KSR.sethost("voip.com");
```

### KSR.setdsturi(...) ###

`bool KSR.setdsturi(str "uri")`

Example:

```
KSR.setdsturi("sip:voip.com:5061;transport=tls");
```

### KSR.resetdsturi(...) ###

`bool KSR.resetdsturi()`

Reset the destination URI (aka: outbound proxy address, dst_uri, $du).

### KSR.isdsturiset(...) ###

`bool KSR.isdsturiset()`

Test if destination URI is set.

### KSR.force_rport(...) ###

`bool KSR.force_rport()`

Set the flag for "rport" handling (send the reply based on source address
instead of Via header).

Example:

```
KSR.force_rport();
```

### KSR.set_drop(...) ###

`void KSR.set_drop()`

Set the DROP flag, so at the end of KEMI script execution, the SIP request branch or the SIP response is not forwarded.

Note: it doesn't not stop the execution of KEMI script, see KSR.x.drop().

Example:

```
KSR.set_drop();
```

### KSR.set_advertised_address() ###

`int set_advertised_address(str "addr")`

### KSR.set_advertised_port() ###

`int set_advertised_port(str "port")`

### KSR.set_forward_close(...) ###

`bool KSR.set_forward_close()`

### KSR.set_forward_no_connect(...) ###

`bool KSR.set_forward_no_connect()`

### KSR.set_reply_close(...) ###

`bool KSR.set_reply_close()`

### KSR.set_reply_no_connect(...) ###

`bool KSR.set_reply_no_connect()`

### KSR.forward(...) ###

`int KSR.forward()`

Forward the SIP request in stateless mode to the address set in destination
URI ($du), or, if this is not set, to the address in request URI ($ru).

### KSR.forward_uri(...) ###

`int KSR.forward_uri(str "uri")`

Forward the SIP request in stateless mode to the address provided in the SIP
URI parameter.

```
KSR.forward_uri("sip:127.0.0.1:5080;transport=tcp");
```

### KSR.hdr.append(...) ###

`int KSR.hdr.append(str "hdrval")`

Append header to current SIP message (request or reply). It will be added after
the last header.

Example:

```
KSR.hdr.append("X-My-Hdr: " + KSR.pv.getw("$si") + "\r\n");
```

### KSR.hdr.append_after(...) ###

`int KSR.hdr.append_after(str "hdrval", str "hdrname")`

Append header to current SIP message (request or reply). It will be added after
the first header matching the name `hdrname`.

Example:

```
KSR.hdr.append_after("X-My-Hdr: " + KSR.pv.getw("$si") + "\r\n", "Call-Id");
```

### KSR.hdr.insert(...) ###

`int KSR.hdr.insert(str "hdrval")`

Insert header to current SIP message (request or reply). It will be added before
the first header.

Example:

```
KSR.hdr.insert("X-My-Hdr: " + KSR.pv.getw("$si") + "\r\n");
```

### KSR.hdr.insert_before(...) ###

`int KSR.hdr.insert_before(str "hdrval", str "hdrname")`

Insert header to current SIP message (request or reply). It will be added before
the header matching the name `hdrname`.

Example:

```
KSR.hdr.insert_before("X-My-Hdr: " + KSR.pv.getw("$si") + "\r\n", "Call-Id");
```

### KSR.hdr.remove(...) ###

`int KSR.hdr.remove(str "hdrval")`

Remove all the headers with the name `hdrval`.

Example:

```
KSR.hdr.remove("X-My-Hdr");
```

### KSR.hdr.is_present(...) ###

`int KSR.hdr.is_present(str "hdrval")`

Return greater than 0 if a header with the name `hdrval` exists and less than
0 if there is no such header.

Example:

```
if(KSR.hdr.is_present("X-My-Hdr") > 0) {
  ...
}
```

### KSR.hdr.append_to_reply(...) ###

`int KSR.hdr.append_to_reply(str "hdrval")`

Add a header to the SIP response to be generated by Kamailio for the current
SIP request.

Example:

```
KSR.hdr.append_to_reply("X-My-Hdr: " + KSR.pv.getw("$si") + "\r\n");
```