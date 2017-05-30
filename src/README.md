Folder for your bot
--------------------

Create folder: `mkdir my-first-recast-chatbot`

Go to your folder: `cd my-first-recast-chatbot`

Clone the repository
--------------------

`git clone https://github.com/plieb/chatbot103.git`

Add services for your GitHub chatbot
--------------------

in `src/index.js` add the following code **at the top of the page**

```
const actions = {
  'trending-stars': require('./trending-stars'),
  'trending-forks': require('./trending-forks'),
  'show-language': require('./show-language'),
  'laugh': require('./laugh'),
}
```
in `src/index.js` add the following code

```
const actions = {
  'trending-stars': require('./trending-stars'),
  'trending-forks': require('./trending-forks'),
  'show-language': require('./show-language'),
  'laugh': require('./laugh'),
}
```

Create show language service
--------------------

Create file: `touch src/actions/show-language.js`

Copy/Paste the following code to create the language service

```
const agent = require('superagent-promise')(require('superagent'), Promise)
const formatter = require('../formatter')
const languages = ['Asssembly', 'C', 'C++', 'Go', 'Java', 'JavaScript', 'Python', 'Ruby', 'Rust', 'Swift',]

export default async function showLanguage(res) {
  console.log('SHOW LANGUAGE')

  const replies = []
  const quickReplies = []
  languages.forEach((l) => {
    quickReplies.push({
      name: l,
      value: `Top starred repos in ${l}`,
    })
  })
  replies.push(formatter.formatQuickReplies(quickReplies,res.reply()))
  return replies
}
```

Create trending forks service
--------------------

Create file: `touch src/actions/trending-forks.js`

Copy/Paste the following code to create the trending forks service

```
/* trending forks */
const agent = require('superagent-promise')(require('superagent'), Promise)
const formatter = require('../formatter')

export default async function trendingForks(res) {
  console.log('TRENDING FORKS')

  const replies = []
  const cardsReplies = []
  const language = res.getMemory('language')
  console.log('======================================')
  console.log(language)
  console.log('======================================')
  replies.push(formatter.formatMsg(res.reply()))
  const response = await agent('GET', `https://api.github.com/search/repositories?q=language:${language.value}&sort=forks&order=desc`)
  console.log('======================================')
  console.log(response)
  console.log('======================================')
  const repos = response.body.items.slice(0, 10)
  console.log('======================================')
  console.log(repos)
  console.log('======================================')
  repos.forEach((rep) => {
    cardsReplies.push({
      name: rep.name,
      picture: rep.owner.avatar_url,
      link: rep.html_url,
    })
  })
  replies.push(formatter.formatCardsReplies(cardsReplies))
  return replies
}
```

Create trending stars service
--------------------

Create file: `touch src/actions/trending-stars.js`

Copy/Paste the following code to create the trending stars service

```
/* trending-stars */
const agent = require('superagent-promise')(require('superagent'), Promise)
const formatter = require('../formatter')

export default async function trendingStars(res) {
  console.log('TRENDING STARS')

  const replies = []
  const cardsReplies = []
  const language = res.getMemory('language')
  console.log('======================================')
  console.log(language)
  console.log('======================================')
  replies.push(formatter.formatMsg(res.reply()))
  const response = await agent('GET', `https://api.github.com/search/repositories?q=language:${language.value}&sort=stars&order=desc`)
  console.log('======================================')
  console.log(response)
  console.log('======================================')
  const repos = response.body.items.slice(0, 10)
  console.log('======================================')
  console.log(repos)
  console.log('======================================')
  repos.forEach((rep) => {
    cardsReplies.push({
      name: rep.name,
      picture: rep.owner.avatar_url,
      link: rep.html_url,
    })
  })
  replies.push(formatter.formatCardsReplies(cardsReplies))
  return replies
}
```

Create laugh service
--------------------

Create file: `touch src/actions/laugh.js`

Copy/Paste the following code to create the laught service

```
const agent = require('superagent-promise')(require('superagent'), Promise)
const formatter = require('../formatter')

export default async function yes(res, payload) {
  console.log('LAUGH')

  const replies = []
  const laughs = [
    'https://media.giphy.com/media/ZqlvCTNHpqrio/giphy.gif',
    'https://media.giphy.com/media/3NtY188QaxDdC/giphy.gif',
    'https://media.giphy.com/media/jQmVFypWInKCc/giphy.gif',
    'https://media.giphy.com/media/xUPOqrl3x2SkKjE3Is/giphy.gif',
  ]
  replies.push(formatter.formatImg(laughs[Math.floor(Math.random() * laughs.length)]))
  return replies
}
```

Call to services
--------------------

In order the bot to use services, add after line 11 in `src/index.js`

```
/* call to services */
if (actions[currentAction]) {
    console.log('Enter action')
    replies = await actions[currentAction].default(res, payload)
}    
```
