
## 1 - Instalar las dependecias de babel 7

`npm install --save-dev @babel/core @babel/cli @babel/preset-env`

`npm install --save-dev @babel/preset-react  - opcional`

`npm install --save @babel/polyfill - opcional`

## 2 - Instalar las ultimas versiones Webpack y webpack-cli
Instalar las ultimas versiones de los loader usados si es necesario.

Si tiene un webpack inferior a su version 3 hay que configurar bien la estructura del objecto del config.

[Webpack configuration](https://webpack.js.org/configuration/)

- babel-loader
- sass loader
- style loader
- css loader
- svg loader
- etc...

	
## 3 - Cambiar los presets y plugins en el loader de babel dentro del config.

```js
{
	loader: 'babel-loader',
	options: {
		presets: [
			['@babel/preset-env', { modules: false }],
			'@babel/preset-react'
		],
		plugins: [
			'@babel/plugin-proposal-object-rest-spread',
			'@babel/plugin-proposal-class-properties'
		]
	}
}
```
	
## 4 - Cambiar el plugin de "extract-text-webpack-plugin" y su uso en el config.
Dejar de usar `extract-text-webpack-plugin` y usar el siguiente plugin que te da webpack.

[mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)

```js

	{
		module: {
			rules: [
				{
					test: /\.(scss|sass|css)$/,
					use: [
						ExtractTextPlugin.extract({
							fallback: 'style-loader',
							use: [
								'css-loader',
								{
									loader: 'postcss-loader',
									options: {
										plugins: () => [
											autoprefixer({
												browsers: '> 1%, last 2 versions, iOS >= 8'
											})
										]
									}
								},
								{
									loader: 'sass-loader',
									options: {
										data: `$ENV: "${environmentMap[STATE_ENV]}"; $isPROD: ${!/QA/.test(STATE_ENV)}; @import "general"; `,
										includePaths: [
											path.resolve(__dirname, './src/styles')
										]
									}
								}
							]
						})
					]
					
				}
			]
		},
		plugins: [
			new ExtractTextPlugin({ fileName : '...' })
		]
	}
```
↓

```js

	{
		module: {
			rules: [
				{
					test: /\.(scss|sass|css)$/,
					use: [
						MiniCssExtractPlugin.loader,
						'css-loader',
						'sass-loader'
					]
				}
			]
		},
		plugins: [
			new MiniCssExtractPlugin({ fileName : '...' })
		]
	};
```

## 5 - Cambiar plugin "UglifyJsPlugin" y cambiar su uso en la config de plugin a optimization
Dejar de usar `webpack.optimize.UglifyJsPlugin` y usar algunos de los siguientes que te da webpack.
	
[uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin)

[terser-webpack-plugin](https://github.com/webpack-contrib/terser-webpack-plugin)


```js
{
	plugins: [
		new webpack.optimize.UglifyJsPlugin()
	]
}
```
↓
```js
{
	optimization: {
		minimizer: [
			new TerserPlugin() || new UglifyJsPlugin()
		]
	}
}
```
