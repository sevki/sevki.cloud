---
title: lon-northwest-1
layout: page
---


sevki.cloud is a domain I registered a while back, I finally repurposed it to actually be useful. sevki.cloud domain is now used for the cluster of machines I maintain at my homelab. My current setup looks a little bit like this.

![](https://docs.google.com/drawings/d/e/2PACX-1vSa2q7xA3Prrr1DLZcT5lALfUcRUHDAdNqYimS_kzS9WOKwZMQlsOwtxS23Hf06_ppGTjoxiAjUXG6W/pub?w=800)

Currently I have 3 node proxmox cluster that runns a bunch of plan9 vms. Each plan9 VM that boots mounts camdvax.


fleet management
----------------

Currently `lon-northwest-1` is made of two thinkpads and an intel NUC.  
All these boxes have the same software running on them and can be very easily replaced. The operating system is [Proxmox Virtual Environment](https://proxmox.com/en/downloads/category/proxmox-virtual-environment)  
On top of Proxmox VE, I also install nomad and consul clients on every machine. The reason I call this entire setup sevki.cloud is because `*.lon-northwest-1.sevki.cloud` is an alias to consul. So every thing I run in my homelab I can reach with zero config.  
I am currently in the process of trying to understand how to turn this entire setup in to an overlay network for the virtualized environments so I can tie them together with wireguard. If you have experience with networking please do reach out to help out.

## software catalouge

| software     	| version 	|
|--------------	|---------	|
| proxmox      	| 6.2     	|
| pve-kernel   	| 5.4.34  	|
| ceph         	| 14.2    	|
| consul       	| 1.8.5   	|
| nomad        	| 1.12.7  	|
| foundationdb 	| 6.2.27  	|
| grafana      	| 7.2.1   	|


plan9
-----

When plan9 vms boot they mount camdvax and start vampria and cloudflared
```rc
srv tcp!192.168.162.243 camdvax
mount -q /srv/camdvax /n/camdvax
bind -a /n/camdvax/bin /bin
vampira/start
cloudflared/start        
```     

Vampira
-------

Both `sevki.io` and `fistfulofbytes.org` are served from plan9 boxes. Both are served from [vampira](https://github.com/sevki/vampira)  
`/n/camdvax/lib/namespace.httpd`

```rc
mount /srv/camdvax /n/camdvax
bind -c /n/camdvax/usr/sevki/blog /tmp/vampira/blog
bind -c /n/camdvax/usr/sevki/www /tmp/vampira/www

cd /tmp/vampira
```    

`vampira/start`
```rc
#!/bin/rc

fn start\_vampira {
    rfork
    vampira/vampira -n /n/camdvax/lib/namespace.httpd \\
    -map sevki.io/tmp/vampira/www \\
    -map fistfulofbytes.com/tmp/vampira/blog \\
    -map ffbyt.es/tmp/vampira/blog/qrs 
}

start\_vampira &
```    


cloudflared
-----------

I have a fork of [cloudflared](https://github.com/sevki/cloudflared/tree/plan9) that runs on plan9. So all my websites are running out of my office. 
![](/assets/IMG_4465.jpg)