## About OpenPower

- [OpenPOWER Foundation](https://openpowerfoundation.org)
- [University of Campinas - Unicamp Minicloud](https://openpower.ic.unicamp.br)

[xet7](https://github.com/xet7), as maintainer of [Wekan](https://wekan.github.io), got access to ppc64le at
at University of Campinas - Unicamp Minicloud. Unicamp is member of [OpenPOWER Foundation](https://openpowerfoundation.org). At Minicloud OpenStack, xet7 created Ubuntu 20.10 VM, and at 2020-12-22 ported Wekan to ppc64le very similarly like previously for [s390x](https://github.com/wekan/wekan/wiki/s390x).

## About MongoDB for OpenPower

Working:
- Official MongoDB binaries for OpenPower are available as part of MongoDB Enterprise subscription.

Other options:
- There is no Community version of MongoDB binaries for OpenPower available, like there is for other CPUs like x64/arm64/s390x.
- xet7 did not yet get MongoDB successfully built from source for OpenPower.
- There are some unofficial MongoDB Docker images by random people, but they could be old versions of MongoDB, and there is no guarantee they don't have any malicious code.
- If sometime in the future some working database binaries become available, link to them will be added here.
- If sometime in the future support for other databases is added to Wekan, info about it could be added here, although adding support for other databases could be a lot of work.

## Download

- wekan-VERSION-ppc64le.zip at https://releases.wekan.team/ppc64le/

## Install

Installing is similar like at https://github.com/wekan/wekan/wiki/Raspberry-Pi

You can start it with start-wekan.sh from https://github.com/wekan/wekan or add SystemD service.

Setup ROOT_URL like described at https://github.com/wekan/wekan/wiki/Settings

At https://github.com/wekan/wekan/wiki is also info about Caddy/Nginx/Apache etc.

Some related links also at https://github.com/wekan/wekan/wiki/Platforms

## How this ppc64le bundle package was created

1. Install Node.js
2. scp wekan-VERSION.zip bundle to ppc64le server, and unzip it
2. Clone and build Fibers, copy fibers to bundle, and zip bundle.
```
git clone https://github.com/laverdet/node-fibers
cd node-fibers
npm install
node build.js
cd ..
cp -pR /home/ubuntu/node-fibers/bin/linux-ppc64-72-glibc bundle/programs/server/node_modules/fibers/bin/
zip -r wekan-VERSION-ppc64le.zip bundle
```
