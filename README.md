# Prerequisites

* Node >= 6.7.0
* Docker >= 1.12.1

And `server/settings.js` should look like:

```js
export const cookieSecret = '';

// Google oauth
export const clientId = '';
export const clientSecret = '';
export const redirectOrigin = '';
```

…but with the correct values.

# To install

```sh
npm install # or yarn
npm run install-mongo
```

# To run

```sh
npm run serve
```


Then up the connection limits in `/etc/nginx/nginx.conf`.

```
worker_rlimit_nofile 90000;

events {
        worker_connections 90000;
        # multi_accept on;
}
```

…then restart nginx.

Your Nginx config is correct, you just miss few lines.

Here is a "magic trio" making EventSource working through Nginx:
```
proxy_set_header Connection '';
proxy_http_version 1.1;
chunked_transfer_encoding off;
```
Place them into location section and it should work.

You may also need to add
```
proxy_buffering off;
proxy_cache off;
```
That's not an official way of doing it.
