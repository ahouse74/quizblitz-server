# Quiz 3 Answers
**Name:** Allison House
**Date:** 13 4 2026



## Q1

B

## Q2

A 400 Bad Request response happens when the user sends incorrectly formatted data. In Quizblitz, this response i given if the user leaves the username or password box empty. A 401 Unauthorized response happens when a user isn't logged in. This could happen if the user tries to POST a score without including a JWT in the header. 404 Not Found happens when a user is searching for something that doesn't exist, like a user ID.

## Q3

The code doesn't wait for Score.find(). ```res.json({ message: 'done'})``` runs before the database can respond, and the scores never get sent. The function has to be async so it will wait.
```js
app.get('/api/scores', async (req, res) => {
  try {
    const scores = await Score.find().sort({ score: -1 }).limit(10);
    res.json(scores);
  } catch (err) {
    res.status(500).json({ error: 'Something went wrong' });
  }
});
```

## Q4

B

## Q5

One advantage of the cookie approach is that the browser handles it, so it's not my problem. One advantage of authorization headers is that they can be used outside of just browsers. For a mobile-accessible game like QuizBlitz, authorization headers are more appropriate. Apps are preferred over browser on mobile, something that authorization headers can handle and cookies can't.