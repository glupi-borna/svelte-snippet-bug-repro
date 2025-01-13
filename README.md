# Snippet alias bug repro

Creating and exporting a snippet in one file, and then importing it in another
file and assigning it to a variable causes the dev server to break on first run
after `npm install`.

- node v20.16.0
- npm v10.8.1

This bug only reproduces after a clean npm install, so, to reproduce the bug:

1. Delete the node modules folder if it exists (`rm -rf node_modules`)
2. Run `npm i` to install the dependencies
3. Run `npm run dev`
4. Observe that, after the dev server starts, the following output is shown:
```
Forced re-optimization of dependencies

  VITE v6.0.7  ready in 213 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
Error:   Failed to scan for dependencies from entries:
  /home/borna/projects/svelte/repro/index.html

  ✘ [ERROR] No matching export in "html:/home/borna/projects/svelte/repro/src/Snippet.svelte" for import "foo"

    script:/home/borna/projects/svelte/repro/src/App.svelte?id=0:2:12:
      2 │    import { foo } from "./Snippet.svelte";
        ╵             ~~~


    at failureErrorWithLog (/home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:1476:15)
    at /home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:945:25
    at runOnEndCallbacks (/home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:1316:45)
    at buildResponseToResult (/home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:943:7)
    at /home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:955:9
    at new Promise (<anonymous>)
    at requestCallbacks.on-end (/home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:954:54)
    at handleRequest (/home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:647:17)
    at handleIncomingPacket (/home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:672:7)
    at Socket.readFromStdout (/home/borna/projects/svelte/repro/node_modules/esbuild/lib/main.js:600:7)
```
5. If you now kill the server (via `CTRL+C`) and run `npm run dev` again, the dev server will start normally.
