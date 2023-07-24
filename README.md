# overlay-lyrics

Webpage which uses the OpenLP API to continuously display song lyrics. We use this webpage
within OBS to overlay song lyrics onto our church livestream.

## Setup

1. Download the webpage and save it onto the machine running OBS
2. Edit the webpage to point to your OpenLP server (edit the `openLPHost` constant on line 81)
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