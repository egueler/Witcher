

#command to run everything 
```cve=$(basename $(dirname $(pwd)));plus=$(basename $(pwd)); docker build -t puppeteer1337/$cve .. && docker kill $cve-$plus; sleep 1; docker run -id --rm --name $cve-$plus -v /p:/p -v /p/Witcher/seclab-targets/$cve/$plus/coverages:/tmp/coverages puppeteer1337/$cve && docker exec -it $cve-$plus bash -c "chown www-data:wc /tmp/coverages; sudo chmod 777 /tmp/coverages" && docker exec -it -w $(pwd) -u wc $cve-$plus bash -i -c 'touch /tmp/start_test.dat' && ipaddr=$(docker inspect $cve-$plus | jq '.[]|.NetworkSettings.Networks.bridge.IPAddress'|tr -d '"') && sed -r 's/(^.*form_url.*http:\/\/)[0-9\.]+(.*)/\1'"$ipaddr"'\2/' witcher_config.json > /tmp/tmpcfg.json && cp /tmp/tmpcfg.json witcher_config.json && timeout 4h node /p/Witcher/base/helpers/request_crawler/main.js http://$ipaddr $(pwd) ; docker exec -it -w $(pwd) -u wc $cve-$plus bash -i -c 'p'```