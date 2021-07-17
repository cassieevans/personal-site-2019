---
title: Client side data in 11ty
metaTitle: Client side data in 11ty
metaDesc: 'Heck yes 11ty hacks'
image: /images/notes.png
alt: 'a scrap of paper with Client side data in 11ty written on it'
socialImage: '/images/notes.png'
date: '2021-07-17'
tags:
  - notes
  - 11ty
---

### A little context

I recently built out a fun two-part project for Edinburgh Science festival.

The first bit was a [quiz](https://dnation.science/). It was built with Vue, hosted on Netlify, and made use of serverless functions to pop the data to airtable. Big ups to Jason for his amazing course on [frontend Masters](https://frontendmasters.com/courses/serverless-functions/).

The second part was a ['thankyou' page](https://thankyou.dnation.science/) to be sent to donators. This thankyou page needed to showcase some of the data collected the airtable base.

I was up against it a bit timewise by the time we got to the second part so I figured a nice trusty 11ty site was the way to go. I could fetch data at build time with no worries about people snooping on my API keys, and could even [trigger a regular build](https://www.11ty.dev/docs/quicktips/netlify-ifttt/) to keep that data nice and current.

I started building out the page, and it was all going well until I realised I didn't _just_ need this data in my templates. I needed to use it in my client side JS too, specifically, in my animation code. Balls.

Luckily I stumbled upon this super cool 11ty trick! The ``permalink`` field in our file's Front Matter Data allows us to change the output filetype of an exported template. 

🥳 Heck yeah, we can build out our own 'api' using 11ty data and consume it on the client side.

I just jotted down the bits of data I needed and told 11ty to output it as JSON for me. 

### The good stuff

```json
---
permalink: '/api/data.json'
---
{% raw %}
[
  {
    "score": "{{ donators.score }}",
    "characters": "{{ donators.characters }}",
    "ages": "{{ donators.ages }}"
  }
]
{% endraw %}
```

Then grabbed the data to use in my animations. Bosh. Problem solved.

```js
axios.get('/api/data.json')
  .then(function (response) { 
   // do stuff with the data. woop!
  }
```
11ty rules.