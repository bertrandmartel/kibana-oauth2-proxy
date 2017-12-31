# Oauth2.0 proxy config for ELK

Docker configuration for using [oauth2_proxy](https://github.com/bitly/oauth2_proxy) as Oauth2.0 proxy for ELK 6.1.1 using either : 

* [Search Guard](http://docs.search-guard.com/latest/main-concepts)
* [X-Pack](https://www.elastic.co/products/x-pack)

Edit `searchguard/docker-compose.yml` or `xpack/docker-compose.yml` with correct oauth config (container `oauth2-proxy`) : 

```yaml
- GITHUB_ORG=<Your Org>
- GITHUB_TEAM=<Your Team>
- CLIENT_ID=<Your Github Client ID>
- CLIENT_SECRET=<Your Github Client Secret>
```

The sample configuration here uses Github authentication for a single Team inside an organization, You can use any supported Oauth provider available [here](https://github.com/bitly/oauth2_proxy#oauth-provider-configuration). You will need to update `oauth-proxy/start.sh` with the correct variables if they are not already there

## SearchGuard

```bash
cd searchguard
docker-compose up
```

Then go to http://locahost:4180

## X-Pack

Complete tutorial for xpack can be found [here](https://www.elastic.co/blog/user-impersonation-with-x-pack-integrating-third-party-auth-with-kibana)

```bash
cd xpack
docker-compose up
```

Then go to http://locahost:4180

## Note for searchguard

The `searchguard/docker-compose.yml` uses two custom images with built-in proxy configuration :

* [`bertrandmartel/docker-elasticsearch`](https://github.com/bertrandmartel/docker-elasticsearch) forked from [`khezen/docker-elasticsearch`](https://github.com/khezen/docker-elasticsearch)
* [`bertrandmartel/docker-kibana`](https://github.com/bertrandmartel/docker-kibana) forked from [`khezen/docker-kibana`](https://github.com/khezen/docker-kibana)

For already existing configuration, check [Using Kibana with proxy authentication](http://docs.search-guard.com/latest/kibana-authentication-search-guard#using-kibana-with-proxy-authentication)

## Using a base path

Only when using Search Guard config, if you want to use a base path, for instance "/kibana" : 

```yaml
nginx-proxy:
  environment:
    - BASE_PATH=/kibana/
```
and 

```yaml
kibana:
  environment:
    SERVER_BASE_PATH: "/kibana"
```
and

```yaml
oauth2-proxy:
  environment:
    - UPSTREAM=http://nginx-proxy:8080/kibana/
```

## License

    The MIT License (MIT) Copyright (c) 2017 Bertrand Martel