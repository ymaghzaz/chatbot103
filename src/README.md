Create a folder for your bot
--------------------

`mkdir my-first-recast-bot`
`cd my-first-recast-bot`

Clone the repository
--------------------

`git clone https://github.com/plieb/chatbot103.git`

Add services for our GitHub chatbot
--------------------

in `src/index.js` add the following code

`const actions = {
  'trending-stars': require('./trending-stars'),
  'trending-forks': require('./trending-forks'),
  'show-language': require('./show-language'),
  'laugh': require('./laugh'),
}`
