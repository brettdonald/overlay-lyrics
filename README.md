# overlay-lyrics

Webpage which uses the OpenLP API to continuously display song lyrics. We use this webpage within
OBS to overlay song lyrics onto our church livestream.

## Setup

1. Download the webpage and save it onto the machine running OBS
2. Edit the webpage to point to your OpenLP server (edit the openLPHost constant in the script)
3. In your OBS scene, add a Browser source, selecting the Local File option
4. Start OpenLP and display some song lyrics
5. The song lyrics will be visible in the OBS scene
