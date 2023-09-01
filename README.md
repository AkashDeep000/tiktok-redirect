###Open
```
itms-apps://itunes.apple.com
```

###Open account settings
```
itms-ui://
```

###Open specific app
```
itms-apps://itunes.apple.com/app/id
```

###Search
```
itms-apps://search.itunes.apple.com/WebObjects/MZSearch.woa/wa/search?media=software&term=YourQuery
```

###Top charts
```
itms-apps://itunes.apple.com/WebObjects/MZStore.woa/wa/viewTop?genreId=36&popId=30
```
###Results in 'Cannot Connect' error when used alone
```
itms-apps://
itms-appss://
macappstore://
macappstores://
itms-watch://
```

# create-svelte

Everything you need to build a Svelte project, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/master/packages/create-svelte).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm create svelte@latest

# create a new project in my-app
npm create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
