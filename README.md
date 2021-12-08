# simple-nginx-caching
To deploy the code you need to have docker installed. Then run the below
```
docker swarm init
docker config create default.conf default.conf
docker config create nginx.conf nginx.conf
docker stack deploy -c compose.yaml service
```

## Testing the caching
Open in browser http://localhost/echo
Look for the string `X-Proxy-Cache: MISS` - This indicates response is from backend
 
 ## Bring down the echo service
docker service scale service_echo=0
Open in browser http://localhost/echo
Look for the string `X-Proxy-Cache: EXPIRED` - This indicates we are serving the request from cache, even though the backend is down.
You can verify the backend is down by running `localhost:4000`

- This is a simple example to achive caching using nginx
- We can also increase the duration of caching in order to reduce the load on backend for static contents. For one hour we can serve contents from cache, post which we will auto update the cache when the next request for backend comes.
