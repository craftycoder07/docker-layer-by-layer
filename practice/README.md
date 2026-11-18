# Practice with real Docker

This Codespace has real Docker installed. Open a terminal (Ctrl+`) and work through the same challenges as the website's practice area.
Port 8080 is forwarded automatically: when something listens on it, Codespaces offers to open it in your browser.

| # | Challenge | Goal |
|---|-----------|------|
| 1 | Hello, container | Run the `hello-world` image. |
| 2 | Background web server | Run `nginx` in the background, named `web`. |
| 3 | Open the front door | Make nginx answer on `localhost:8080`, then `curl` it. |
| 4 | Clean sweep | Stop and remove `web`. |
| 5 | Data that survives | Run `postgres:16` named `db` with the named volume `pgdata` at `/var/lib/postgresql/data`. |
| 6 | Talk by name | Put `db` and a container named `api` on a network `appnet`; resolve `db` from inside `api`. |
| 7 | Build your own | Build `practice/hello` and tag it `hello:1.0`. |
| 8 | Cache-friendly | Edit `practice/hello/index.html`, rebuild, and notice which steps say `CACHED`. |
| 9 | Ship it | Run `hello:1.0` with `-p 8080:80` and open the forwarded port. |
| 10 | Push-ready | Tag the image for a registry you own (e.g. `ghcr.io/<you>/hello:1.0`) and push it. |

<details>
<summary>Solutions</summary>

```sh
docker run hello-world
docker run -d --name web nginx
docker rm -f web && docker run -d --name web -p 8080:80 nginx && curl localhost:8080
docker stop web && docker rm web
docker run -d --name db -e POSTGRES_PASSWORD=pw -v pgdata:/var/lib/postgresql/data postgres:16
docker network create appnet && docker network connect appnet db
docker run -d --name api --network appnet alpine sleep infinity && docker exec api getent hosts db
docker build -t hello:1.0 practice/hello
docker build -t hello:1.0 practice/hello      # after editing index.html
docker run -d -p 8080:80 hello:1.0
echo $GITHUB_TOKEN | docker login ghcr.io -u <you> --password-stdin   # needs a token with write:packages
docker tag hello:1.0 ghcr.io/<you>/hello:1.0 && docker push ghcr.io/<you>/hello:1.0
```
</details>

Clean up when you're done: `docker rm -f $(docker ps -aq); docker system prune -a --volumes`
