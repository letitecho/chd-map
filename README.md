# Let it ECHO : CHD Map (Proof-of-Concept)
Visualizing CHD cases in the Philippines. We start in the Philippines and if possible invite the CHD global community to help us map all CHD around the world.

## How to build docker image and run a container, then view on local machine
* Clone the repo
```bash
	λ git clone https://github.com/letitecho/chd-map.git
	
		Cloning into 'chd-map'...
		remote: Enumerating objects: 3, done.
		remote: Counting objects: 100% (3/3), done.
		remote: Compressing objects: 100% (3/3), done.
		remote: Total 41 (delta 0), reused 3 (delta 0), pack-reused 38
		Unpacking objects: 100% (41/41), done.
```
* Go to the chd-map directory
```bash
	λ cd chd-map\
```
```bash
	D:\repo\chd-map (master -> origin)
	λ git status
	
	On branch master
	Your branch is up to date with 'origin/master'.

	nothing to commit, working tree clean

	D:\repo\chd-map (master -> origin)
	λ git branch -a
	
	* master
	  remotes/origin/HEAD -> origin/master
	  remotes/origin/dev_branch
	  remotes/origin/master

	D:\repo\chd-map (master -> origin)
	λ ls -al
	
	total 10
	drwxr-xr-x 1 Kristoff 197121    0 Jan 31 22:31 ./
	drwxr-xr-x 1 Kristoff 197121    0 Jan 31 22:28 ../
	drwxr-xr-x 1 Kristoff 197121    0 Jan 31 22:31 .git/
	-rw-r--r-- 1 Kristoff 197121   54 Jan 31 22:31 Dockerfile
	-rw-r--r-- 1 Kristoff 197121 1095 Jan 31 22:28 LICENSE
	-rw-r--r-- 1 Kristoff 197121  171 Jan 31 22:28 README.md
	drwxr-xr-x 1 Kristoff 197121    0 Jan 31 22:28 src/
```
* Build the docker image 
```bash
	D:\repo\chd-map (master -> origin)
	λ docker images
	
	REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
	nginx               latest              42b4762643dc        8 days ago          109MB
	hello-world         latest              fce289e99eb9        4 weeks ago         1.84kB

	D:\repo\chd-map (master -> origin)
	λ ls
	Dockerfile  LICENSE  README.md  src/

	D:\repo\chd-map (master -> origin)
	λ docker build -t letitecho-chd-map .
	
	Sending build context to Docker daemon  1.074MB
	Step 1/3 : FROM nginx
	 ---> 42b4762643dc
	Step 2/3 : COPY src/ /usr/share/nginx/html
	 ---> ee5bc2261b85
	Step 3/3 : EXPOSE 80
	 ---> Running in 51171e1b3e4a
	Removing intermediate container 51171e1b3e4a
	 ---> 8f7a4e2d8930
	Successfully built 8f7a4e2d8930
	Successfully tagged letitecho-chd-map:latest
	SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
```
* Run a container instance with host port forwarding between the host and the container.
```bash
	D:\repo\chd-map (master -> origin)
	λ docker run -ti --rm -p 80:80 letitecho-chd-map
```
* Launch your browser and go to http://localhost. The request will be forwarded to the container. Sample output below.
```bash
	D:\repo\chd-map (master -> origin)
	λ docker run -ti --rm -p 80:80 letitecho-chd-map
	
	172.17.0.1 - - [31/Jan/2019:14:41:32 +0000] "GET / HTTP/1.1" 200 2882 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
	172.17.0.1 - - [31/Jan/2019:14:41:32 +0000] "GET /maps/chd-markers.json HTTP/1.1" 200 4761 "http://localhost/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
	172.17.0.1 - - [31/Jan/2019:14:41:32 +0000] "GET /maps/chd-leaf.js HTTP/1.1" 200 940 "http://localhost/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
	172.17.0.1 - - [31/Jan/2019:14:41:32 +0000] "GET /maps/images/trianglify-header-2560-400-2.png HTTP/1.1" 200 107199 "http://localhost/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
	172.17.0.1 - - [31/Jan/2019:14:41:32 +0000] "GET /maps/images/letitecho.png HTTP/1.1" 200 4417 "http://localhost/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
```
* Stop the container when done.
```bash
	D:\repo\chd-map (master -> origin)
	
	λ docker container list -all
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS
			   PORTS                NAMES
	4a666be14b4f        letitecho-chd-map   "nginx -g 'daemon of…"   46 seconds ago      Up 44 seconds       0.0.0.0:80->80/tcp   affectionate_elgamal

	D:\repo\chd-map (master -> origin)
	λ docker stop affectionate_elgamal
	affectionate_elgamal
```
