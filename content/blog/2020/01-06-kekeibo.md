---
title: Kakeibo - Japanese budgeting system
description: A story how my fear meets three Japanese concepts and produces an ultimate budgeting app
permalink: /kakeibo
date: 2020-01-06
image: /img/kakeibo.png
category: WebDev
tweet: https://twitter.com/razbakov/status/1233894656846462976
---

A story how my fear meets three Japanese concepts and produces an ultimate budgeting app.

Since I start university I was always frightened to spend any money. Sometimes this fear would play a cruel trick with me and I would spend everything I have in one day. I didn't have any financial literacy that time. From that time I was looking for any money strategies I could apply in my life.

## Apps that spoiled me

Year after year, from time to time I am looking for some budgeting app to set limits for my expenses. From all the apps I can highlight [Toshl](https://toshl.com/) and [Pennies](https://www.getpennies.com/).

Premium Toshl provides many budgets. It worked for me only if I would use it on daily basis and do all the planning in the app.

Pennies is super simple and provides only budgets per day. Kinda what I needed, but it's available only for iPhone and I haven't find a worthy alternative for Android. Also there is no web version.

## A step towards simplicity and flexibility

As for monthly planning I haven't found any better solution then Google Spreadsheet. It is very flexible and you can make projections in any form you like. For one year I switched completely to Google Spreadsheet even for day to day usage.

I wanted also to track my expenses from my phone, but editing a Spreadsheet is not that comfortable. I came up with very simple Telegram bot. An IFTTT service that would add expenses from chat to Transactions table.

Sometimes I was pulling my expenses from my N26 bank to that same table to have all my finance data. Few functions and I got auto-matched tags with my expense categories. I automated it with very simple Puppeteer script. It logged in to my N26 bank account, download CSV and import to Google Spreadsheet.

That took a lot of discipline to use this setup. And sometimes my laziness took over and I started avoiding it for months.

What I learned so far about myself is that to make it work it should be simple. I should be able to forget about this tool and be able to start over again without going into much work. There is no need to track regular expenses on daily basis that I have no control about.

## Japanese rush to the rescue!

![Kameido](https://thepracticaldev.s3.amazonaws.com/i/oc0y2hxt9ia541nfn2o4.jpg)

Later I discovered [Kakeibo](https://www.credit.com/personal-finance/kakeibo/). It is a Japanese offline Budgeting System. As from what I read everyone who have tried this method is happy. It reminded me about Envelope system my friend once showed me. Kakeibo guides you to budget 4 categories: Needs, Wants, Culture, Extra. As I mentioned above I find it unproductive to keep track every day on regular expenses. I decided to avoid it in my calculations. It became a part of monthly planning routine. The only thing I need to know is how much money available for this month to spend. Calculation goes as follows:

    Available = Income + Rollover - Contracts - Savings - Limit

After that I split Available money between that 4 envelopes as a group of expense categories:

- Needs (groceries, medicine)
- Wants (going out, shopping)
- Culture (courses, books)
- Extra (gifts, unexpected expenses)

Year and Month planning I can do in Google Spreadsheet.

![gsheet](https://thepracticaldev.s3.amazonaws.com/i/8qvppzlvwcsqtkpz1qd9.png)

But how do I see how much money left in envelope per day?

A paper envelope is not a solution anymore. It is not always possible nowadays to use only cash. So far I was tracking all expenses with Telegram or even simple Google Notes. I found myself calculating every week how much money actually left. Later I came back to idea of how much money left per day. Old paper envelopes do not have that kind of magic.

I was back on surfing around the App Store looking for apps that could provide that simple function. I haven't found a satisfactory solution.

## Time to code! Ikigai!

Ikigai is another Japanese word, it brings self-awareness. Reason for which you wake up in the morning, thing that you do, that you can be paid for, that world needs, that you love, and that you are good at. Programming is my ikigai.

And here am I drawing UX scenarios, screens, prototypes. Under big impression of Sprint by Jake Knapp I started defining user goals and ways to reach them.

![scenarios](https://thepracticaldev.s3.amazonaws.com/i/kmvbczaqu1ukh4dvt26b.jpeg)

By the end of the first day I got interactive prototype and fake data developed with [Quasar](https://quasar.dev/). Big role played paper sketches that I had aside of my table. It was total fail to start with empty file with no code. Pen and paper somehow gives brain an air full of ideas. 

![screens](https://thepracticaldev.s3.amazonaws.com/i/mg8ki335jv4vat69a4nl.jpeg)

Anyway I came up with some better ideas of interface, while actually clicking it through. 

By the end of the second day I was struggling if I should continue. It was difficult to switch mind and jump into coding. Way forward was unclear.

Another Japanese method came to rescue - Kaizen. Kaizen is a strategy to ask yourself, in my case question was "what is the smallest next step I can do?".

I remembered a nice library to get backend done fast - [Vuex Easy Firestore](https://mesqueeb.github.io/vuex-easy-firestore/). I decided to give it a try. That determined fate of the project. I installed library and set up Firebase project. Time to define data structures. Paper and pen again saved my day. Few minutes later and Firestore collection is ready. It's time to blow life into project and make it show values from Firestore. Vuex Easy Firestore made this super easy.

First commit and I had ability to create budgets. I thought that's it for today, but then decided to do some Kaizen again. So second commit and I got functional app. I thought to postpone some features. Kaizen came again. I got all desirable features done. Smart history and smart calculation of daily available money are available.

So if you setup a daily budget lets say 300 EUR and there are 30 days in this month that would mean that:

    Available per day = Budget / Days in month * Current Day - Already Spent

But what happens if you decide to spend 100 EUR in first day? This would show you negative amount. In this case we can agree that it make sense to warn you about overspending, but show positive amount, right?

    Amount = Money Left / Days Left

So in the end I have very simple app that show me how much money I can spend today. I don't need to track all regular expenses, it is already tracked by bank.

![pocket-budget.png](https://thepracticaldev.s3.amazonaws.com/i/10gh4doy9fizjtui7u5t.png)

And in case I want to take a break from the app for some time - no problem. If I decide to start over I need to **Log Balance** to let it know what is actual amount of money in each envelope left. It will track difference as Unknown expenses.

![log.png](https://thepracticaldev.s3.amazonaws.com/i/9a06znwgbawdrrgj7fvb.png)

And today I decided to publish this app on Netlify.

Here you go - [MoneyDo](http://bit.ly/2twa6kG). You can also add it to your Homescreen and access it anytime to check if you are safe to spend money now.

My ultimate goal is to have balanced life and not to be under influence of my fear. I want to know that at this specific moment it's okay to go to restaurant or buy new pair of shoes and I won't struggle by the end of the month without having any money left to eat. PocketBudget is a very simple solution to that problem.

Which budgeting system do you use? Is it implementable with PocketBudget? Is there some MUST HAVE I should add to the app?