---

title: "Another Guest Post: Monitoring, Comments, and a Much Better VPS"
date: 2026-06-29T14:30:00+08:00
draft: false
image: "cover.png"
categories:
- Homelab

tags:
- Linux
- Homelab
- Prometheus
- Grafana
- Remark42
- Docker
- Nginx
- ZeroTier

---

Today turned into one of those unexpectedly productive infrastructure days.

I started with a monitoring setup that was only partially finished and ended with the entire environment visible in Prometheus and Grafana, a working comment system on the blog, a larger VPS, and a proper update script tying everything together.

## Monitoring is finally complete

Prometheus is now scraping all the machines that matter:

* the monitoring VM
* the files VM
* the utility VM
* the ThinkPad server
* the VPS
* the cloud VM
* the archive VM

Every target reports as healthy, and Grafana can switch cleanly between them.

The dashboard now gives me one place to check CPU usage, memory, storage, load, uptime, and network activity across the entire setup.

This had been sitting on the roadmap for a while, so finally being able to remove it felt good.

## ZeroTier is cleaned up too

The network separation was finished as well.

The private systems and public-facing services now live on separate ZeroTier networks, with only the machines that genuinely need access connected to both.

There is still a longer-term plan to simplify the public side further, but the current setup is already much cleaner than before.

Eventually the VPS and files server will probably share a small service network of their own, while the rest of the infrastructure remains private.

That can wait.

## The VPS got an upgrade

The VPS was still running with only 1 GB of RAM, which had become increasingly uncomfortable.

GoAccess already consumed a noticeable amount of memory, and adding Docker and another public service would have left very little room.

I upgraded it to:

* 2 GB RAM
* 50 GB disk

The improvement was immediate. Swap usage disappeared, there is now comfortable free memory, and the disk has plenty of space for future work.

This should also make the eventual migration of the archived WordPress site much less painful.

Not painless, unfortunately. Just less painful.

## Remark42 is now live

The main goal for the day was to add comments to the new blog.

I chose Remark42 because it is small, self-hosted, open source, and does not require handing the entire comment section over to an external platform.

It now runs in Docker on the VPS and is exposed through nginx at:

`https://sleepymario.com/comments/`

The container itself only listens on localhost, so nginx is the only public entry point.

The setup includes:

* persistent comment storage
* automatic backups
* GitHub login
* anonymous commenting
* administrator access through my GitHub account
* nginx reverse proxying
* a health check
* log rotation

The comment section has been added to all current blog posts.

## The theme now follows the site

There was one visual problem.

Remark42 initially stayed in light mode while the blog defaulted to dark mode, which looked fairly terrible.

That was fixed by connecting Remark42 to the Stack theme’s existing color-scheme event.

Now, when the site switches between light and dark mode, the comment section changes with it immediately.

That small detail made the integration feel much more complete.

## A proper VPS update command

The final piece was a reusable VPS update script.

Future maintenance now requires only:

`sudo update-vps`

The script:

* updates Ubuntu packages
* updates Docker and Docker Compose
* pulls the latest Remark42 image
* creates a backup before replacing the container
* retains the eight newest backups
* waits for the Remark42 health check
* validates nginx
* tests the main website
* tests the public comment endpoint
* reports whether a reboot is required

The first full run completed successfully.

No pending packages, no reboot required, and all services came back healthy.

## What is left

The infrastructure roadmap is much shorter now.

The next actual project will probably be setting up the TCL NXTPAPER A1 as a dedicated notetaking device.

Everything else can move slowly:

* Matrix and Element cleanup
* i3 or Sway configuration
* rebuilding the Gentoo control center
* migrating the archive site
* eventually simplifying the remaining public network layout
* several very low-priority services

For once, nothing urgent is left unfinished.

That is a good place to stop for the day.

---

This is quite a technical author, isn't it. Now please don't hack me 哭哭. 
