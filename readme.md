# Twitch HLS Cache Docker Container

## Introduction

This docker container acts a caching proxy server for HLS video streams from Twitch TV. For any network where multiple people are watching the same Twitch stream, this will result in saved bandwidth.

The primary use case is at LAN parties with hundreds of users. Many of these gamers will want to watch streams, especially if a large esports event is happening at the same time. The ability to replace 20+ 3mbps streams with a single stream will reduce the strain on the Internet connection greatly.

## Usage

You will need to have a DNS server forwarding queries to the machine your docker container is running on. You can use the [steamcache-dns](https://hub.docker.com/r/steamcache/steamcache-dns/) docker image to do this or you can use a DNS service already on your network see the [steamcache-dns github page](https://github.com/steamcache/steamcache-dns) for more information.

Run the steamcache container with the using the following to allow TCP port 80 (HTTP) through the host machine:

```
docker run --name twitch -p 192.168.0.5:80:80 steamcache/twitch:latest
```
## Quick Explanation

For a Twitch HLS cache to function on your network you need two services.

* A Twitch cache service [This container](https://github.com/steamcache/twitch)
* A special DNS service [steamcache-dns](https://github.com/steamcache/steamcache-dns)

The twitch cache transparently intercepts the Twitch traffic. If the stream is already being viewed, it will serve up the cached stream data that is already being fetched. If it's not being streamed, then it will start streaming and caching it.

The special DNS service handles DNS queries normally (recursively), except when they're about Twitch and in that case it responds that the twitch cache should handle it instead.

Note: steamcache-dns does not currently support Twitch. It will be updated shortly.

## DNS Entries

If you are running your own DNS servers, you will need to add the following DNS zones and records to your server in order to intercept the Twitch HLS streams.

* *.usher.justin.tv
* *.usher.twitch.tv
* *.hls.twitch.tv
* *.hls.ttvnw.net

## Suggested Hardware

The hard disk capacity required is minimal as the content is very time-specific. A small 2GB cache on a small SSD would be more than enough for caching multiple Twitch streams.

Each stream consumed will require up to 3.5mbps of bandwidth, so on a gigabit network you could serve up streams to approximately 280 people before you'd start hitting bottlenecks.

## Running on Startup

Follow the instructions in the Docker documentation to run the container at startup.
[Documentation](https://docs.docker.com/articles/host_integration/)

## License

The MIT License (MIT)

Copyright (c) 2016 Michael Smith, Robin Lewis, Brian Wojtczak, Jason Rivers

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
