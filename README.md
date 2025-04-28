# Greetings Team Fliplet!

Below are instructions for running the challenges.

## For both challenges:

### Clone the repo

```bash
git clone https://github.com/danefondo/fliplet-technical-challenges.git
cd fliplet-technical-challenges
```

(OR download & extract ZIP file.)

## For Fliplet Todo Challenge

### Prerequisites:

- Node.js

### Commands

```bash
cd fliplet-todo-challenge
npm install
npm run serve
# Then go to the URL provided by the Terminal.
```

## For Fliplet RSS Challenge

### Commands

```bash
cd fliplet-rss-challenge
# open rss.html in your browser
```

## Commentary

1. A mistake I made was trying to fight with getting JSFiddle to work with various drag-and-drop libraries in the todo challenge, whereas I should've pivoted sooner to save time and could've likely saved ~20 minutes. I switched to my editor soon after.
2. I also spent too much time wrestling with CORS and trying different proxy URLs to get around issues in the RSS challenge. In the end, I put together a basic Node app endpoint on Heroku to avoid having to purchase paid proxy solutions or use unreliable free ones.
