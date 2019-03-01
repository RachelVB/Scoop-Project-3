
```js 
function createArticle(url, request) {
  
  const requestArticle = request.body && request.body.article; 
  
  // here we are checking to see if the request has a body and has information on the article we want to add

  const response = {};

  if (requestArticle && requestArticle.title && requestArticle.url &&
      requestArticle.username && database.users[requestArticle.username]) { // this very long if condition is checking that the article information given by the request has all the properties/information we want. If the request object doesn't then we don't want to create the article object nor add it to our db 

    const article = {
      id: database.nextArticleId++,
      title: requestArticle.title,
      url: requestArticle.url,
      username: requestArticle.username,
      commentIds: [],
      upvotedBy: [],
      downvotedBy: []
    }; // Here we are creating the article object using the information from the create/post request which we will use to add to our db

    database.articles[article.id] = article; 
    // here we add the new article object we just created t the db where we are holding our articles. Take note that we use the article id to specify where to add the article in the db.
    database.users[article.username].articleIds.push(article.id); // here we are updating our users info list in our db to tell our db that this particular user/author of the article created an article.

    response.body = {article: article}; // here we are setting up the body of our APIs response to hold the article we just created. The response is what the frontend will get once we finish adding the article to our db
    response.status = 201; // here is where we add a status code to tell the frontend or clientside that the request of adding an article was successful. 
  } else {
    response.status = 400; // if we end up here, we were unsuccessful at creating the new article in our db. A reason being is that the request given by the frontend didn't have all the fields we wanted as indicated in the if statement. 
  }

  return response; // here we actually send the response! woot woot. 
} 
```