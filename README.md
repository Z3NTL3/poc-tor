# poc-tor
Proxy connection through tor nodes.

#### Docker image used
``docker run --name='tor-privoxy' -d -p 9050:9050 -p 9051:9051 -p 8118:8118 dockage/tor-privoxy:latest``

# Example usage
```ts
/**
 *        Solidarity Freedom Unit
 *           Derived from Pix4
 *  
 *         Proof Of Concept project
 *              @z3ntl3
 */
import Undici, { ProxyAgent, setGlobalDispatcher } from "undici";
import { Writable } from "stream";
const {XMLParser} = require('fast-xml-parser');
const options = {
  ignoreAttributes : false
};

const parser = new XMLParser(options);
const agent = new ProxyAgent('http://127.0.0.1:8118') // default docker image env port
setGlobalDispatcher(agent);

(async () => {
  let buffer = ""
  await Undici.stream('https://pool.proxyspace.pro/judge.php', 
  {method: 'GET', headers: {'cache-control':'no-cache'}},(data)=>{ 
    return new Writable({
        write: (chunk, encoding, callback) => {
            buffer += chunk
            callback()
        }
    })
})
  let json = parser.parse(buffer);
  console.log(`\x1b[1m${json.body.link.pre}\x1b[0m`)
})()
```
