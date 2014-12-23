---
layout: post

title: The Docker Book
subtitle: "My review"

excerpt: "My review of the excellent book by James Turnbull."

---

I take an interest in the idea of building [SOA](http://en.wikipedia.org/wiki/Service-oriented_architecture)
applications and the idea of [microservices](http://martinfowler.com/articles/microservices.html) for a long time.
I share an opinion that SOA is the key to building scalable services from both technical and managerial (organize development process within big teams) aspects.

At the same time I have been sceptical about putting these ideas into action
(they look like yet another dreams of developers and operations about happy future),
because I had no idea how to develop and manage multitude services:

* how to solve problems of shared installation on one server?
* how to do continuous integration / continuous delivery / monitoring / logging for these services in a one manner in order to keep rational development cost?
* how to segregate of duties of developers and operations [properly](http://en.wikipedia.org/wiki/DevOps)?
* how to make [The Twelve-Factor App](http://12factor.net/) real?

But we are living in a wonderful time, so future becomes true faster than we might expect :)

[Docker](https://www.docker.com/) and its ecosystem gives answers for those questions via an implementation
of idea of application’s [containers](http://en.wikipedia.org/wiki/Operating_system%E2%80%93level_virtualization)
on top of [LXC](http://en.wikipedia.org/wiki/LXC) (for now).

[The Docker Book](http://www.dockerbook.com/) gives you understanding what Docker is and how to use it in practice.

I recommend to read this book to everyone who deals with application's backend on Linux servers.
Note: it is important for better understanding to have access to Linux shell and try to execute commands from the book.

By chapters:

* **Chapters 1-4**. General chapters about Docker – useful to all.
* **Chapters 5-6**. You definitely should read these chapters if you are excited about what Docker is and you want to know how to build real services with it.
* **Chapter 7**. This chapter gives understanding how to solve orchestration and service discovery problems for containers, so it is mostly useful for operations than developers. Personally I was more interested in [Kubernetes](http://kubernetes.io/) (an open source orchestration system for Docker containers by Google, which is more relevant for usage in [OWOX](http://www.owox.com/) where we are building services with Google Cloud), but the mentioned chapter was interesting as initial point for me.
* **Chapter 8-9**. These chapters are relevant to you if you become a Docker hacker (in a good meaning).

**P.S.** If you feel that Docker is a [silver bullet](http://en.wikipedia.org/wiki/Silver_bullet) after reading – of course it is not.
It is important to understand that Docker is just the concrete implementation of great ideas, but it is very powerful and popular now. I realize that [other players](https://github.com/coreos/rocket) may be involved in the game later, so there is no guarantee that Docker is the future. Tools are changing goals remain the same.

**P.P.S.** Thanks to [James Turnbull](https://twitter.com/kartar) for the excellent book and to [OWOX](http://www.owox.com/) for the given opportunity :)