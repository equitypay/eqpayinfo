# How to Deploy eqpayinfo

eqpayinfo is splitted into 3 repos:
* [https://github.com/eqpayproject/eqpayinfo](https://github.com/eqpayproject/eqpayinfo)
* [https://github.com/eqpayproject/eqpayinfo-api](https://github.com/eqpayproject/eqpayinfo-api)
* [https://github.com/eqpayproject/eqpayinfo-ui](https://github.com/eqpayproject/eqpayinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy eqpay core
1. `git clone --recursive https://github.com/eqpayproject/eqpay.git --branch=eqpayinfo`
2. Follow the instructions of [https://github.com/eqpayproject/eqpay/blob/master/README.md#building-eqpay-core](https://github.com/eqpayproject/eqpay/blob/master/README.md#building-eqpay-core) to build eqpay
3. Run `eqpayd` with `-logevents=1` enabled

## Deploy eqpayinfo
1. `git clone https://github.com/eqpayproject/eqpayinfo.git`
2. `cd eqpayinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `eqpayinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `eqpayinfo` under a process manager (like `pm2`), to restart the process when `eqpayinfo` crashes.

## Deploy eqpayinfo-api
1. `git clone https://github.com/eqpayproject/eqpayinfo-api.git`
2. `cd eqpayinfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy eqpayinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/eqpayproject/eqpayinfo-ui.git`
2. `cd eqpayinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "eqpayINFO_API_BASE_CLIENT=/api/ eqpayINFO_API_BASE_SERVER=http://localhost:3001/ eqpayINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `eqpayinfo-ui` on port 3000
4. `npm run build`
5. `npm start`
