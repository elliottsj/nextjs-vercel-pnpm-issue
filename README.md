# nextjs-vercel-pnpm-issue

Demo of an issue deploying a next@10.0.3 project to Vercel using pnpm.

See the main [README](https://github.com/elliottsj/nextjs-vercel-pnpm-issue/blob/master/README.md) for more context.

## Proposed fix

(link PR) is proposed to fix this issue. By using [resolve](https://github.com/browserify/resolve)'s `preserveSymlinks: true` option instead of `fs.realpathSync`, we avoid the build error.

### How to repro this bug fix

1. Set up this project as described [here](https://github.com/elliottsj/nextjs-vercel-pnpm-issue/tree/fix#repro).
2. Set up a 3rd-party npm registry, e.g. [Bytesafe](https://bytesafe.dev/). This is needed to publish the patched Next.js packages so Vercel can download them.

    Make sure to configure the [`.npmrc`](.npmrc) file so that the correct registry is used. You may need to set an `NPM_TOKEN` environment variable on Vercel to authenticate.

3. Check out the branch of the proposed fix and publish its packages to your registry:

    ```shell
    git clone https://github.com/elliottsj/next.js
    git checkout resolve-request
    ```

    In `lerna.json`, set `"registry": "https://your-npm-registry.example.com/"`.

    Commit the change and merge it into the local `canary` branch:

    ```shell
    git add lerna.json
    git commit -m 'Configure private npm registry'

    git checkout canary
    git merge resolve-request
    ```

    Bump package versions:

    ```shell
    $ yarn run lerna version prerelease --preid canary --force-publish
    ...
    Changes:
    - create-next-app: 10.0.5 => 10.0.6-canary.0
    - @next/eslint-plugin-next: 10.0.5 => 10.0.6-canary.0
    - @next/bundle-analyzer: 10.0.5 => 10.0.6-canary.0
    - @next/codemod: 10.0.5 => 10.0.6-canary.0
    - @next/env: 10.0.5 => 10.0.6-canary.0
    - @next/mdx: 10.0.5 => 10.0.6-canary.0
    - @next/plugin-google-analytics: 10.0.5 => 10.0.6-canary.0
    - @next/plugin-sentry: 10.0.5 => 10.0.6-canary.0
    - @next/plugin-storybook: 10.0.5 => 10.0.6-canary.0
    - @next/polyfill-module: 10.0.5 => 10.0.6-canary.0
    - @next/polyfill-nomodule: 10.0.5 => 10.0.6-canary.0
    - next: 10.0.5 => 10.0.6-canary.0
    - @next/react-dev-overlay: 10.0.5 => 10.0.6-canary.0
    - @next/react-refresh-utils: 10.0.5 => 10.0.6-canary.0

    ? Are you sure you want to create these versions? Yes
    lerna info execute Skipping GitHub releases
    lerna info git Pushing tags...
    lerna success version finished
    ✨  Done in 16.13s.
    ```

    Publish the packages:

    ```shell
    $ yarn run lerna publish from-git --npm-tag canary --yes
    ...
    Successfully published:
    - create-next-app@10.0.6-canary.0
    - @next/eslint-plugin-next@10.0.6-canary.0
    - @next/bundle-analyzer@10.0.6-canary.0
    - @next/codemod@10.0.6-canary.0
    - @next/env@10.0.6-canary.0
    - @next/mdx@10.0.6-canary.0
    - @next/plugin-google-analytics@10.0.6-canary.0
    - @next/plugin-sentry@10.0.6-canary.0
    - @next/plugin-storybook@10.0.6-canary.0
    - @next/polyfill-module@10.0.6-canary.0
    - @next/polyfill-nomodule@10.0.6-canary.0
    - next@10.0.6-canary.0
    - @next/react-dev-overlay@10.0.6-canary.0
    - @next/react-refresh-utils@10.0.6-canary.0
    lerna success published 14 packages
    ✨  Done in 236.69s.
    ```

4. Install the newly-published "next" package in this project:

    ```shell
    pnpm add -E next@10.0.6-canary.0
    ```

5. Push the project to Vercel and observe the build. It should pass.
