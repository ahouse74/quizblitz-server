## Q1
Middleware is code that runs between getting a request and sending a response. Order matters because Express goes top to bottom. For example, cors() and express.json() are registered before any routes, so every request gets parsed correctly. If express.json() came after, the req.body would be undefined. Global middleware with app.use() runs on everything. Route middleware only runs when it gets added; i.e. verifyToken is only on app.post('/api/scores', verifyToken) so the public GET route doesn't need a token. verifyToken itself reads the Authorization header, calls jwt.verify(), and attaches the decoded user to req.user before calling next().
## Q2
If passwords are stored as plain text and the database gets hacked, every user's password is exposed. bcrypt.hash(password, 10) scrambles the password in a way that can't be reversed. The algorithm runs 10 times because of the 10 passed as a parameter. Bcrypt also uses salting so identical passwords hash differently. bcrypt.compare() doesn't reverse the hash (hashes can't be reversed). It just hashes the input the same way and checks if the results match.
## Q3
Register: client sends username and password, server hashes the password and saves the user to MongoDB, returns a success message.
Login: client sends credentials, server verifies the password with bcrypt, then signs a JWT containing the user's id and username and sends it back.
Submit score: client sends the JWT in the Authorization header, verifyToken checks the signature using the secret key and pulls out the user info, server saves the score to MongoDB.
The server doesn't need a database lookup each time because the JWT signature proves it was made by the server.
## Q4
Problem 1: Every server restart wipes the array. The server would go to sleep and all scores would disappear on wake.
Problem 2: The array only exists inside one process. Multiple server instances would each have separate arrays with different data.
With MongoDB, data lives outside the server entirely. Redeploying just reconnects to the same database, which is completely different from the in-memory array which only existed while the server was running.
## Q5
A leaderboard is meant to be seen by everyone, so requiring a token just to view scores would be pointless and annoying. People should be able to check the leaderboard before even making an account. POST is protected because without authentication anyone could submit fake scores. If GET also required a token it would add friction with no real benefit, since the score data is supposed to be public anyway.