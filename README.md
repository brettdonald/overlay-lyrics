# overlay-lyrics

Webpage which uses the OpenLP API to continuously display song lyrics. We use this webpage
within OBS to overlay song lyrics onto our church livestream.

## Setup

1. Download the webpage and save it onto the machine running OBS
2. Edit the webpage to point to your OpenLP server (edit the `openLPHost` constant on line 89)
3. In your OBS scene, add a Browser source, selecting the Local File option
4. Start OpenLP and display some song lyrics
5. The song lyrics will be visible in the OBS scene

## Logic

The page contains some smarts to help figure out the best font size to use when displaying
each song. The default font size is **4vw**, but if, at that size, any line of the song is
wider than the screen, the font size of the entire song is reduced in 0.1vw steps until
all lines of the song fit the screen.

Many songs in our database include meta information about the song on intro slides. Slides
containing meta information are rendered in a smaller font size (1.2vw) and are excluded
when performing the width calculation described in the previous paragraph. This app
identifies meta slides as those:

* which are tagged as an Intro slide in OpenLP, **and**
* which contain any of the following strings:
  * ©
  * ccli
  * lyrics
  * music
  * publish

## Connectivity

OpenLP provides real-time updates via a [WebSocket connection](https://gitlab.com/openlp/wiki/-/wikis/Documentation/websockets).
Every operation within OpenLP sends a message over this connection; although each message
contains very limited information. However, receipt of these messages is a useful trigger
to request more detailed information from OpenLP.

So, whenever the page receives a message via the WebSocket, it calls the `/controller/live-items`
endpoint of the [OpenLP API](https://gitlab.com/openlp/wiki/-/wikis/Documentation/HTTP-API).
This endpoint responds with detailed information on the currently-displayed item.

If the WebSocket connection cannot be established, a small orange dot appears in the lower
right corner to alert the operator that there is no connection to OpenLP. The operator can
click this orange dot to see error details. This orange dot will also appear if the
connection is terminated, or if a fetch request generates an error or times out.

Following an error, the page will automatically continue attempting to connect, reconnect
or refetch at a rate of once every 5 seconds. As soon as connectivity is established,
re-established or the fetch succeeds, the orange dot disappears.
