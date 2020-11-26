# nextjs-vercel-pnpm-issue

Demo of an issue deploying a next@10.0.3 project to Vercel using pnpm.

### The issue

After switching to pnpm by running `pnpm install` to generate [pnpm-lock.yaml](pnpm-lock.yaml) and setting the Vercel project's **install command** to `npx pnpm install`, the Vercel build will fail with:

```
17:33:50.191  	Installing build runtime...
17:33:50.824  	Build runtime installed: 632.776ms
17:33:51.434  	Looking up build cache...
17:33:51.563  	Build cache not found
17:33:52.198  	Running "install" command: `npx pnpm install`...
17:33:53.880  	npx: installed 1 in 1.6s
17:33:54.454  	Lockfile is up-to-date, resolution step is skipped
17:33:54.489  	Progress: resolved 1, reused 0, downloaded 0, added 0
17:33:54.655  	Packages: +534
17:33:54.655  	++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
17:33:55.215  	Packages are hard linked from the content-addressable store to the virtual store.
17:33:55.216  	  Content-addressable store is at: /vercel/.pnpm-store/v3
17:33:55.216  	  Virtual store is at:             node_modules/.pnpm
17:33:55.490  	Progress: resolved 534, reused 0, downloaded 25, added 23
17:33:55.497  	Progress: resolved 534, reused 0, downloaded 26, added 23
17:33:56.499  	Progress: resolved 534, reused 0, downloaded 85, added 79
17:33:56.535  	Progress: resolved 534, reused 0, downloaded 85, added 80
17:33:57.539  	Progress: resolved 534, reused 0, downloaded 167, added 163
17:33:57.542  	Progress: resolved 534, reused 0, downloaded 167, added 164
17:33:58.554  	Progress: resolved 534, reused 0, downloaded 241, added 234
17:33:58.561  	Progress: resolved 534, reused 0, downloaded 241, added 235
17:33:59.580  	Progress: resolved 534, reused 0, downloaded 345, added 333
17:33:59.584  	Progress: resolved 534, reused 0, downloaded 346, added 333
17:34:00.585  	Progress: resolved 534, reused 0, downloaded 532, added 531
17:34:00.587  	Progress: resolved 534, reused 0, downloaded 532, added 532
17:34:00.793  	Progress: resolved 534, reused 0, downloaded 534, added 534, done
17:34:01.061  	.../sharp@0.26.2/node_modules/sharp install$ (node install/libvips && node install/dll-copy && prebuild-install) || (node-gyp rebuild && node install/dll-copy)
17:34:01.063  	.../@ampproject/toolbox-optimizer postinstall$ node lib/warmup.js
17:34:01.279  	.../sharp@0.26.2/node_modules/sharp install: info sharp Using cached /vercel/.npm/_libvips/libvips-8.10.0-linux-x64.tar.br
17:34:01.909  	.../@ampproject/toolbox-optimizer postinstall: AMP OPTIMIZER Downloaded latest AMP runtime data.
17:34:01.950  	.../@ampproject/toolbox-optimizer postinstall: Done
17:34:02.708  	.../sharp@0.26.2/node_modules/sharp install: Done
17:34:03.007  	dependencies:
17:34:03.007  	+ next 10.0.3
17:34:03.007  	+ react 17.0.1
17:34:03.007  	+ react-dom 17.0.1
17:34:03.420  	Running "npm run build"
17:34:03.696  	> nextjs-vercel-pnpm-issue@0.1.0 build /vercel/workpath0
17:34:03.696  	> next build
17:34:04.685  	info  - Creating an optimized production build...
17:34:04.707  	Attention: Next.js now collects completely anonymous telemetry regarding usage.
17:34:04.707  	This information is used to shape Next.js' roadmap and prioritize features.
17:34:04.707  	You can learn more, including how to opt-out if you'd not like to participate in this anonymous program, by visiting the following URL:
17:34:04.707  	https://nextjs.org/telemetry
17:34:12.641  	internal/fs/utils.js:269
17:34:12.641  	    throw err;
17:34:12.641  	    ^
17:34:12.642  	Error: ENOENT: no such file or directory, lstat '/vercel/workpath0/url'
17:34:12.642  	    at realpathSync (fs.js:1643:7)
17:34:12.642  	    at handleExternals (/vercel/workpath0/node_modules/.pnpm/next@10.0.3_react-dom@17.0.1+react@17.0.1/node_modules/next/dist/build/webpack-config.js:68:21)
17:34:12.642  	    at webpackConfig.externals (/vercel/workpath0/node_modules/.pnpm/next@10.0.3_react-dom@17.0.1+react@17.0.1/node_modules/next/dist/build/webpack-config.js:80:136)
17:34:12.642  	    at handleExternals (/vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/ExternalModuleFactoryPlugin.js:78:17)
17:34:12.642  	    at next (/vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/ExternalModuleFactoryPlugin.js:66:9)
17:34:12.642  	    at handleExternals (/vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/ExternalModuleFactoryPlugin.js:71:7)
17:34:12.642  	    at /vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/ExternalModuleFactoryPlugin.js:101:5
17:34:12.642  	    at /vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/NormalModuleFactory.js:400:5
17:34:12.642  	    at AsyncSeriesWaterfallHook.eval [as callAsync] (eval at create (/vercel/workpath0/node_modules/.pnpm/tapable@1.1.3/node_modules/tapable/lib/HookCodeFactory.js:33:10), <anonymous>:16:1)
17:34:12.642  	    at NormalModuleFactory.create (/vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/NormalModuleFactory.js:381:28)
17:34:12.642  	    at /vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/Compilation.js:897:14
17:34:12.642  	    at Semaphore.acquire (/vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/util/Semaphore.js:29:4)
17:34:12.642  	    at asyncLib.forEach.err.stack (/vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/Compilation.js:895:15)
17:34:12.642  	    at arrayEach (/vercel/workpath0/node_modules/.pnpm/neo-async@2.6.2/node_modules/neo-async/async.js:2405:9)
17:34:12.642  	    at Object.each (/vercel/workpath0/node_modules/.pnpm/neo-async@2.6.2/node_modules/neo-async/async.js:2846:9)
17:34:12.642  	    at Compilation.addModuleDependencies (/vercel/workpath0/node_modules/.pnpm/webpack@4.44.1/node_modules/webpack/lib/Compilation.js:873:12) {
17:34:12.642  	  errno: -2,
17:34:12.642  	  syscall: 'lstat',
17:34:12.642  	  code: 'ENOENT',
17:34:12.642  	  path: '/vercel/workpath0/url'
17:34:12.642  	}
17:34:12.657  	npm ERR! code ELIFECYCLE
17:34:12.657  	npm ERR! errno 1
17:34:12.660  	npm ERR! nextjs-vercel-pnpm-issue@0.1.0 build: `next build`
17:34:12.661  	npm ERR! Exit status 1
17:34:12.661  	npm ERR! 
17:34:12.661  	npm ERR! Failed at the nextjs-vercel-pnpm-issue@0.1.0 build script.
17:34:12.661  	npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
17:34:12.667  	npm ERR! A complete log of this run can be found in:
17:34:12.667  	npm ERR!     /vercel/.npm/_logs/2020-11-26T22_34_12_661Z-debug.log
17:34:12.675  	Error: Command "npm run build" exited with 1
17:34:14.465  	Done with "package.json"
```

### Repro

The exact repro steps were:

1. Create this project with `pnpx create-next-app`. Name it `nextjs-vercel-pnpm-issue`.
2. Create the repo on GitHub and `git push` the Next.js project.
3. Import the project to Vercel via https://vercel.com/import.
4. Push each commit invididually (see the commit history of this repo) and observe each Vercel deployment.
5. After pushing commit 804c122157193afa6fd8de9386782aca1e14cf36, observe the build logs above.
