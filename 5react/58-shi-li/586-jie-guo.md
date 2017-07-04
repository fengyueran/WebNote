运行npm start(babel-node server.js) 开启开发环境，启动html
server.js:
```
/* eslint-disable */
import webpack from 'webpack';
import config from './webpack.config';
import browserSync from 'browser-sync';
import webpackHotMiddleware from 'webpack-hot-middleware';
import webpackDevMiddleware from 'webpack-dev-middleware';

const bundler = webpack(config);

// Run Browsersync and use middleware for Hot Module Replacement
browserSync({
  startPath: 'index.html',
  server: {
    baseDir: ['./src', './'],//服务路径，也就是页面资源存放的路径
    middleware: [
      webpackDevMiddleware(bundler, {
        // Dev middleware can't access config, so we provide publicPath
        publicPath: config.output.publicPath,

        // pretty colored output
        stats: { colors: true },

        // Set to false to display a list of each file that is being bundled.
        noInfo: true,

        // for other settings see
        // http://webpack.github.io/docs/webpack-dev-middleware.html
      }),

      // bundler should be the same as above
      webpackHotMiddleware(bundler),

      // historyApiFallback(),
    ],
  },

  // no need to watch '*.js' here, webpack will take care of it for us,
  // including full page reloads if HMR won't work
  files: [

  ],
});

```
