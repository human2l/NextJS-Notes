## Start a new project

`npx create-next-app` or `yarn create-next-app`

`npx` is a node package runner to run node executable

## Upgrade Next.js version

1. `npm install react@latest react-dom@latest`

2. `npm audit`

3. Check the high vulnerabilities, then fix it

4. `npm audit fix`

5. keep doing step 2-4 till all of vulnerabilities fixed

Resource: https://nextjs.org/docs/upgrading

## Next.js project folder structure

<img src="Next.js.assets/Screen Shot 2022-03-17 at 10.52.47 AM.png" alt="Screen Shot 2022-03-17 at 10.52.47 AM" style="zoom:50%;" />

### Pages:

Next.js will create a router for files in `/pages` base on file name

`_app.js` is the entry of project, is a wrapper of our app, we may add global features here, i.e. footer

## CSS Module

`*.module.css`

Any file with surfix ".module.css" is a css module. each module have a scope base on file name. i.e. "Home.module.css" is scope to "home" component.

css module will not affect pages outside its scope. i.e. `.container` in one module will **not overwrite** `.container` in another one. This because each class will be given a unique name.

<img src="Next.js.assets/Screen Shot 2022-03-17 at 11.08.59 AM.png" alt="Screen Shot 2022-03-17 at 11.08.59 AM" style="zoom:50%;" />

all styles in `module.css` will be export as object. and be inported to the page.

```css
.container {
	color: red;
}
```

```react
import styles from "../styles/hello.module.css"
const hello = () => {
	return <div className={style.container}>Hello World</div>;
}
export default Hello
```





