---
layout: post
title:  "OCT 4th outage"

---


On 4th of october, I sent a friend, the link to sevki.cloud and went to get coffee. I got a reply back pretty quick...

![](/assets/oct-4-incident.png)

### What happened?

One of the thinkpads fan stopped working.This lead to it over heating and shutting down. But I hadn 't even noticed it went down. The problems began when I rebooted another node.  

<table>
    <tr>
        <td>
            <img src="/assets/fan.jpg" width="320px" />
        </td>
        <td>
            <video width="320" controls>
            <source  type="video/mp4" src="/assets/fan.mp4" width="300px" />
        <source  type="video/webm" src="/assets/fan.webm" width="300px"/>
        </video>
        </td>
    </tr>
</table>

  
The thing with highly available and fault tolerant systems is that for them to be able to do anything, they need quorum. But when you start them, they don't have quorum, so most fault tolerant systems have a bootstrapping stage where they sit and wait to reach a quorum. But that stage is manual, so when you 're recovering from the loss of a single node,

I have learned that running infra at capacity is a horrible idea.