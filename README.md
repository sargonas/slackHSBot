# Innkeeper Slackbot
A webserver that supplies data to a Slack outgoing webhook for a hearthstone-themed slack bots. Ideal solution when a full featured slackbot does not make sense or is unavailable.
Currently used for finding cards in hearthstone and linking their image or text into chat. 
I've forked https://github.com/skeltont/slackHSBot and made a fair bit of improvements and modifications for my own slack server.
(If you make use of my own app, please make sure to give him due credit for all the initial heavy lifting!)

## Install & Run
- `git clone` repo
- `npm install`
- For non-Heroku use:
  - rename example.config.json to config.json
  - Obtain an API key (see below) and place it in config.json
  - `node app.js`
- For Heroku and local toolbelt use:
  - rename .example.env to .env
  - Obtain an API key (see below) and place it in .env
  - `heroku local:run node app.js`

(If running in production on Heroku, be sure to set your ENV variables in the Heroku dashboard and keep your .env ignored from git!)

## Usage
Configure an outgoing webhook in slack to send a POST request to the server (http://example.com/get_card) when it identifies your trigger (we use 'innkeeper' for ours).
There are (currently) two routes you can POST to, depending on your preferences. /get_card is "vanilla" and returns a normal card image, with 3 flags (gold, flavor, and text-only).
A second route, /get_gold_card, can be optionally configured instead to _always_ return a gold image as the default, with the same flags but swapping the gold argument for plain. 
(In the event a gold image can not be found in the API, a regular will be returned instead.)
<br /> *Example:* <br />
``` Innkeeper [C'Thun] ```
This should return 
>"here you go: http://wow.zamimg.com/images/hearthstone/cards/enus/original/OG_279.png"

##### Options
Three additional options are available:
- **-g**: ```Innkeeper [C'Thun] -g``` Will link a .gif of the *golden* card image into chat. (only available on /get_card)

 or

- **-p**: ```Innkeeper [C'Thun] -g``` Will link a .gif of the *plain* card image into chat. (only available on /get__gold_card)
- **-f**: ```Innkeeper [C'Thun] -f``` Will add the flavor text for the card into chat after the image. 
- **-t**: ```Innkeeper [C'Thun] -t``` Will return only all of the card data (skipping undefined sections) with no image into chat. 

## Testing your server's response directly
```curl -X POST --data "text=Innkeeper [C'Thun]" http://localhost:3000/get_card```

should return

```{"text":"here you go: http://wow.zamimg.com/images/hearthstone/cards/enus/original/OG_279.png"}```

## API dependency
This app makes use of an external api called [hearthstoneapi](http://hearthstoneapi.com/). You'll need to obtain your own API key for it, from Mashape (linked to from the API's site).

## Example
![Example Image](http://i.imgur.com/e7XXx1t.png)
