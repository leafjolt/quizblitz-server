# Quiz
## Q1
B) app.use(express.json()) middleware is missing or registered after the route

## Q2
400 Bad Request means that the user sent invalid or missing data. For example, in the code for `POST /api/scores` we return 400 if `score` or `totalQuestions` is missing:
```js
if (score === undefined || !totalQuestions) {
    return res.status(400).json({ error: 'score and totalQuestions are required' })
}
```

401 Unauthorized means the user is not authorized to make the request. For example, we return this if the user tries to login with an invalid password:
```js
if (!passwordMatch) {
    return res.status(401).json({ error: 'Invalid email or password' })
}
```

404 Not Found means the resource being requested does not exist. We don't use this anywhere in the server code, but we could return this if the user tried to call a route that is not defined; 404 would be returned to show that that route is not there.

## Q3
The JSON object being sent back in the response does not include any scores in it, and there's no variable to hold the Score object we're finding. In addition, we need async/await to do the find operation. We also probably want to have a try/catch block in case of error. Here's the correct version:

```js
app.get('/api/scores', async (req, res) => {
    try {
        const scores = await Score.find().sort({ score: -1 }).limit(10)
        res.json({ message: 'done', scores })
    } catch (err) {
        res.status(500).json({ error: "Couldn't fetch scores" })
    }
})
```

## Q4
B) A schema defines the shape and validation rules for documents; a model is the class you use to query and save documents based on that schema

## Q5
One advantage of the cookie approach is that we no longer need to manually send authorization headers with every request, the cookie is automatically persisted with each request. One advantage of the Authorization header approach is that we have explicit control over it; we can choose to only include it in requests where authorization is required. In addition, the Authorization header approach is a lot more consistent and will work the same across all devices because we are handling it, whereas cookies could have some inconsistency across devices. 

I think the Authorization header approach is more appropriate for a mobile-accessible game like QuizBlitz, as it keeps things consistent across whatever device we access the game on, and lets us handle only exactly what we need with authorization.