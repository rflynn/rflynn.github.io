---
layout: post
title: "Docker 'Hello World' on Ubuntu"
date: 2014-10-05
summary: Docker seems very promising but I haven't seen enough any self-contained scripts that show off its capabilities. So I made one.
---

<p>
Docker seems very promising but I haven't seen enough any self-contained scripts that show off its capabilities. So I made one.
</p>

<p>
Given an Ubuntu machine this script installs Docker, pulls down a pre-made image, launches it in a container, hits the running web aplication, then shuts it down and deletes the container and then prints how much network traffic the whole operation took.
</p>

<code style="white-space:pre; font-size:smaller">(
<span title="verbose">set -v</span>
<span title="fail on error">set -e</span>
<span title="define function to read number of bytes rx/tx on our default network interface eth0">eth0_bytes(){ grep eth0 /proc/net/dev | awk -F' ' '{print $2}'; }</span>
<span title="read number of bytes currently">bytes_before=$(eth0_bytes)</span>
<span title="add docker's apt source to the system so apt-get can find where to get it">sudo /bin/sh -c "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"</span>
<span title="add docker's gpg key so we can use its packages with apt-get">sudo /bin/sh -c "wget -qO- https://get.docker.io/gpg | apt-key add -"</span>
<span title="fetch apt package list from everywhere, including docker">sudo apt-get update</span>
<span title="install docker via apt-get">sudo apt-get install -y lxc-docker</span>
<span title="run the docker image named 'training/webapp' daemonized with port forwarding. this will download the image from docker's hub. it's kinda big but manageable.">sudo docker run --name=sweet -d -p 5000:5000 training/webapp python app.py</span>
<span title="wait until the web app comes up, should only take a few seconds">until curl http://localhost:5000/ 2>/dev/null; do sleep 1; done</span>
<span title="show output from docker container's running web app">curl http://localhost:5000/</span>
<span title="stop container">sudo docker stop sweet</span>
<span title="delete container (not image)">sudo docker rm sweet</span>
<span title="read number of current bytes on eth0 for comparison">bytes_after=$(eth0_bytes)</span>
<span title="display number of bytes used total. i get 92.3MB">printf "%.1fMB\n" $(echo "($bytes_after - $bytes_before) / (1024*1024)" | bc -l)</span>
)
</code>

<h3>References</h3>
<ul>
    <li><a href="https://docs.docker.com/userguide/dockerizing/">Dockerizing Applications</a>
    <li><a href="https://wiki.debian.org/SourcesList">SourcesList</a>
    <li><a href="http://flask.pocoo.org/">Flask</a>
</ul>

<!--h3>Comments</h3>
<div>
<fb:comments href="http://www.parseerror.com/blog/docker-hello-world" num_posts="10"></fb:comments>
<script>
  FB.init({ 
    appId:'251363198227326',
    cookie:true, 
    status:true,
    xfbml:true 
  });
</script>
</div-->
