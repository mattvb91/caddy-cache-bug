This is a test repository to recreate what seems like a cache / memory corruption issue.

To get started:

```
cd nextjs && npm i && npm run build
cd ../ && docker-compose up -d
```

The config is available at: http://localhost:2019/config
the cache endpoint is available at: http://localhost/__cache/souin

You should be able to get a valid starter response from nextjs container at http://localhost

On the console everything should be ok.

Now we can start adding hosts:

```bash
curl --location --request PUT 'localhost:2019/config/apps/http/servers/srv0/routes/0/match/0/host/0' \
--header 'Content-Type: application/json' \
--data-raw '"host.localhost"'
```

Change `host.localhost` in the `--data-raw` part to a number of subdomains for example

```
host1.localhost
host2.localhost
host3.localhost
host4.localhost
```

Now visit each host you have added + the original localhost 

Sometimes it takes me up to 3 hosts before corruption takes place. You need to switch between each host and refresh each host (hard refresh and normal refresh) and then
corruption will take place.

You should then be able to click on the script in dev tools & open in new tab to see the corrupted file. If you hard refresh on it
it goes back to normal.