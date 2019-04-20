---
title: Exploring Closures
date: 2019-04-19T20:34:07+00:00
author: Clayton Salem
layout: post
permalink: /exploring-closures/
---

I was inspired by Eric Elliot's post on closures. He posted on twitter with this super simple example `const secret = msg => () => msg;`. Here's the article, as usual I'll encourage you to read that first: https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36

The long and short is that the automatic lexical closure in javascript allows us to give us data privacy, this can be compared to protected or private properties in OOP.

Note: In the following example **I'm missing the main point!**, which is "Data privacy is an essential property that helps us program to an interface, not an implementation." What follows is a rather silly _implmentation_ using data privacy to hide secret values, not create an interface to be extended and built upon later. Most of these posts and side-projects are about playing and learning, so I'm excusing myself to do just that.

The application I build here is a list of "users", their names and pictures can be accessed by anyone. However, their "secrets" are accessible only by making a request with the corresponding `accessToken`. Here's an example user:

```
{
    jimmy: {
        secret: "I eat cat food",
        accessToken: "fdsasaf"
    }
}
```

I'm keying the object on the name for convenience. 

In order to get the `accessToken` we need to send a password, which then returns a token, that token can then be used to get a list of `accessTokens` for each of the other users.

In this whole process we want to keep our password, our token, and our list of tokens out of the global scope, and not saved to any external source (say session storage). This is where our closure comes into play.

Note: instead of creating a real server I've basically added another js file to the page. In this file is both the secret data, some page rendering, and some mocked `fetch` functions we can use to pretend we're calling to a server. 

```
// Page rendery server code
const users = {
    jimmy: {
        secret: "I just want to be alone.",
        accessToken: "l;kj",
        pic: "https://randomuser.me/api/portraits/men/9.jpg"
    },
    susy: {
        secret: "I eat fourteen bananas every day.",
        accessToken: "fdsa",
        pic: "https://randomuser.me/api/portraits/women/1.jpg"
    },
    andrew: {
        secret: "I had a foot transplant at the age of 8.",
        accessToken: "wert",
        pic: "https://randomuser.me/api/portraits/men/80.jpg"
    },
    melanie: {
        secret: "I like the smell of my own farts.",
        accessToken: "trew",
        pic: "https://randomuser.me/api/portraits/women/92.jpg"
    },
    bobby: {
        secret: "I have lice.",
        accessToken: "fess",
        pic: "https://randomuser.me/api/portraits/men/50.jpg"
    },
    ricky: {
        secret: "There are thirteen people living in my house.",
        accessToken: "xcvb",
        pic: "https://randomuser.me/api/portraits/men/3.jpg"
    }
}
const formBuilder = function (user) {
    return `<form class="retrieve-secret" data-id="${user}">
    <button>Retrieve My Secret</button>
    </form>` //style="display: none"
}

const htmlUserList = function (users) {
    return Object.keys(users).reduce((list, user) => `${list}<li><h3>${user}</h3><div class="user-container"><img src="${users[user].pic}"/>${formBuilder(user)}<span class="secret-display"></span></div></li>`, '')
}

document.querySelector("#user-list").innerHTML = htmlUserList(users);

// Ajaxy Server code
let userToken;
const fetchToken = ({ password }) => {
    return userToken = `asdfasdf${password}asdfasdf`;
}

const fetchTokens = ({ token }) => {
    return token === userToken ? Object.keys(users).reduce((list, user) => { list[user] = users[user].accessToken; return list }, {}) : '416';
}

const fetchUserSecret = ({ accessToken, userName }) => {
    // Once we have a token, we can make requests to the server with the token and get a good response.
    if (accessToken === users[userName].accessToken) {
        return users[userName].secret
    }
    else {
        return '416 or whatever'
    }
}
```

Needless to say (maybe), all this data _is_ in the global scope and available anywhere, again, we're just pretending it's on a server. Following is the fun stuff and the main closure:

```
const client = (password) => {
    let myToken;
    let userTokens;
    return {
        login: function () {
            myToken = fetchToken({ password });
        },
        getUserTokens: function () {
            userTokens = fetchTokens({ token: myToken })
        },
        getSecret: function (user) {
            return fetchUserSecret({ accessToken: userTokens[user], userName: user })
        }
    }
}
```

So, `client` is a functions that takes `password` as an argument. It returns an object with three methods embeded. Each of these methods has "closed around" one or more of the variables in the lexical scope, namely, `myToken`, `userTokens`, and `password`. 

```
// Wiring and fireing
document.querySelector("#login-form").addEventListener("submit", function (e) {
    e.preventDefault();
    const myClient = client(e.target.login.value);
    myClient.login();
    myClient.getUserTokens();
    document.querySelectorAll(".retrieve-secret").forEach((element) => {
        element.querySelector('button').style.display = "block";
        // element.style.display = 'none';
        element.addEventListener("submit", (e) => {
            e.preventDefault();
            element.parentNode.querySelector('.secret-display').innerText = myClient.getSecret(e.target.dataset.id)
        })
    })
})
```
Putting it together, we instantiate a client named `myClient` using the `client` function, passing in the password from the form. 

```
const myClient = client(e.target.login.value);
```
We log the user in, which "fills in" `myToken`.

```
myClient.login();
```
 `myToken` is then used to fetch the `userTokens`

```
myClient.getUserTokens();
```

Then, when we click the particular users show secret button, we use that user's `accessToken` to fetch their secret

```
myClient.getSecret(e.target.dataset.id)
// the method signature here is:
getSecret: function (user) {
    return fetchUserSecret({ accessToken: userTokens[user], userName: user })
}
```

There you have it, data privacy. And sure, the example is contrived, and defintely misses the point that this is most useful as a way to build applications, not implement secret management, but it was fun none-the-less. Thanks for hanging! The code is all [here](https://github.com/claytron5000/closure-experiment)