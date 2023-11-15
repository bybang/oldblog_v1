---
layout: single
title: "[Java] TIL Java Spring Boot - Movie Review Site 003"
categories: Java
tag: [Java, Spring Boot, TIL]
toc: true
published: true
---

![](https://velog.velcdn.com/images/devbang/post/44b25767-0d3c-4c6a-92fc-6542a8abf306/image.png)

# Full Stack Development with Java Spring Boot, React, and MongoDB – Full Course

### This is the course from freeCodeCamp to learn Java with react

> You can find the video on YouTube here:
> https://youtu.be/5PdEmeopJVQ?si=uyg8MHc6LDpfd3kw

# 🧩 Problem

## API Controller

### Movie Controller

We created Movie and Review models and connected the `mongoDB` with this application. The next thing we are going to make is the API controller.

As same as we did for the test API, we have to annotate this class as `@RestController`.

Next, instead of mapping it to `localhost:8080`, we will map it to `/api/v1/movies` with `@ResquestMapping("/api/v1/movies")`. With this setting, any requests to the `/api/v1/movies` endpoint will be handled in this `MovieController` controller.

Create the `GET` method inside the class by adding `@GetMapping`. We will add a random string and check the connection.

![](https://velog.velcdn.com/images/devbang/post/1f34d5d6-6f21-43d0-995e-afb5cff2279d/image.png)

If you see `All Movies` from the browser, we are ready to move on.

Although it is okay to return the `String`, it is always better to return `ResponseEntity`. We will set up the `ResponseEntity<String>` and return `new ResponseEntity<String>()` instead of plain string. Inside of the parenthesis, we set the return value and the `HttpStatus` code.

![](https://velog.velcdn.com/images/devbang/post/c07f9a85-b142-4071-9d75-44016a6447a2/image.png)

The browser might look the same, but we are actually returning the 200 status code. We can check this by running the following command.

```
$ curl -i http://localhost:8080/api/v1/movies
```

### Using Movie Class

We made the `Movie` class before, and to use it and pull data from the database, we will make the `Service` class and the `Repository` class.

Because the `repository` is a type of interface, we have to make the interface class.

This interface `extends MongoRepository<>`, and we must let the generic type know what type of `data and ID` we are dealing with.

So it becomes `<Movie, ObjectId>`, and we should annotate this as `@Repository` so the framework knows this interface as a repository.

![](https://velog.velcdn.com/images/devbang/post/be1dca16-2f3a-4266-bb76-41021a68a2ef/image.png)

Next, create a `Service` class.

Annotate this class as a service with `@Service`. In this class, we are going to write this database access method.

The first method will be `public allMovies()` and change the controller name to `getAllMovies()` because it is a `GET` mapping.

> Go to `MovieController` and change the method name

Put the return type in front of the `allMovies()`. We are going to return a list of movies from this method. Hence, the code should be similar to the following.

```java
public List<Movie> allMovies() {}
```

Inside of this `Service` class, we need a reference of a repository. We can call `MovieRepository` with this code.

```java
private MovieRepository movieRepository
```

In Java, we have to initialize this code using a constructor or the `@Autowired` annotation.

The `@Autowried` lets the framework know that we want the framework to instantiate the class `MovieRepository`.

![](https://velog.velcdn.com/images/devbang/post/ed68c934-f7f7-44db-949a-217da9e21f9b/image.png)

Use the method from `mongoDB`, and we can find the `findAll()` method returns a list of data types.

![](https://velog.velcdn.com/images/devbang/post/4ba3126e-36c5-4536-9569-47ec6a6abcce/image.png)

Go back to the `MovieController` and add a reference to the `Service` class by adding this line. Make sure to autowire two classes together with the annotation.

```java
@Autowired
private MovieService movieService;
```

We now know that the data from the `Service` class is a list of movies. Let's change accordingly by using the following code.

```java
@GetMapping
public ResponseEntity<List<Movie>> getAllMovies() {
	return new ResponseEntity<List<Movie>>(movieService.allMovies(), HttpStatus.OK);
}
```

Run the application, and check the browser that the database is printed in `JSON` format.

![](https://velog.velcdn.com/images/devbang/post/9eeb60b4-146e-4185-b9e5-10e688836aa0/image.png)

To wrap up, in **_REST API_**, there are multiple layers. Below is how the `MovieController` class, `MovieService` and `MovieRepository` works.

1. One of the layers is `MovieController`, which is also one of the API layers.
2. This only concerns the task of `GET`, which means getting a `request` from the user and returning a `response`.
3. The `MovieController` using the `Service` class and delegating to that `Service` class the task of fetching `allMovies` from the database.
4. When the controller gets the `request`, it calls the `allMovies` method inside of the `MovieService` class.
5. The `MovieService` class gets the list of the movies and passes it to the `MovieController`.
6. The `MovieController` returns the list of the movies with the `HttpStatus` code.
7. The `MovieController` just fetches the request since it's the API layer.
8. Most of the business logic lives under the `Service` class.
9. The `Service` class uses the `Repository` class. It talks to the database, gets the list of the movies and returns to the API layer.
10. The `Repository` layer is the data access layer for the API.
11. The `Repository` layer is the layer that talks with the database and gets the data back.

### Accessing Single Movie

We learned about the flow of how each of the classes and repositories works. We will now add a logic to get a single movie instead of the movie list.

Add an annotation for mapping, but this time, we will map to a specific address. By adding `"/{id}"`, we can search a movie by its `ID`.

```java
@GetMapping("/{id}")
public ResponseEntity<Movie> getSingleMovie(@PathVariable ObjectId id) {
}
```

This time we will get the single movie, so we added `<Movie>`, and inside the parenthesis, we have the `@PathVariable`. This annotation will let the framework know that we will be passing the information we got in the mapping as a path variable.

The `@PathVariable ObjectId id` part means that the data we are getting through the path variable `/{id}`, we want to convert that to an `ObjectId` called `id`.

Next, write a new method in the `Service` layer.

```java
public Movie singleMovie(ObjectId id) {
	return movieRepository.findById(id);
}
```

![](https://velog.velcdn.com/images/devbang/post/ca5c2d7c-d0a6-459c-9122-5b423ee42cd1/image.png)

We are getting an error because the `findById` method may not find the movie at all, or maybe the `id` has passed doesn't exist. In these cases, we have to return `null`.

In react, we can return the optional thing with `{option ? <> : <>}` or simply adding `?` to the variable.

Similarly, we can add the `Optional` class to the `Movie` class. Import the `Optional` class and add it just like the `List` class.

```java
public Optional<Movie> singleMovie(ObjectId id) {
	...
}
```

![](https://velog.velcdn.com/images/devbang/post/7da9c3bf-f210-4c2c-bccc-bcdc0c262a47/image.png)

We also need to update the controller with the `Optional` class. Fix `ResponseEntity` with the `Optional` class.

![](https://velog.velcdn.com/images/devbang/post/edce0656-41ab-43dd-b1b9-d6f820e30772/image.png)

Start the application again and copy one of the `id` from the `mongoDB` and add it behind the current address.

```
http://localhost:8080/api/v1/movies/{id}
```

We can see the single movie in the browser now, but the problem is for security purposes, we don't want to expose the `ObjectId` of the collection entities to the public.

Instead, we will use `imdbId` to search for the new movies.

![](https://velog.velcdn.com/images/devbang/post/d3a8ea25-b624-44fd-b98c-99e077c35722/image.png)

### Implement a method for `imdbId`

Although the repository comes with built-in methods for searching with `id`, it doesn't have a method for `imdbId`.

To implement a method for `imdbId`, go to the `MovieRepository`, and we use automatic queries.

> For more information on query creation on Spring Boot, check out [HERE](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation)

Import and add the `Optional` class, and following the above query creation sample, we will name it `findMovieByImdbId(String imdbId)`.

Just naming this method, the `Spring data mongoDB` will understand what this code is supposed to do.

![](https://velog.velcdn.com/images/devbang/post/5072e3b6-2198-4cd0-ba98-89604f5a0fc2/image.png)

Go back to the `MovieService` and update the `ObjectId` part since we are no longer getting it, but we are getting the `String imdbId`.

```java
public Optional<Movie> singleMovie(String imdbId) {
	return movieRepository.findMovieByImdbId(imdbId);
}
```

Don't forget to update the `findById` method to `findMovieByImdbId`, and go to the `MovieController` and update it correspondingly. Also, check the typo before running the application.

![](https://velog.velcdn.com/images/devbang/post/6f069541-e504-4624-95aa-9ca6588dfe98/image.png)

Copy one of the `imdbId` from the `mongoDB` compass, and if your browser shows that single movie, our code works as expected.

![](https://velog.velcdn.com/images/devbang/post/024720a0-8b4d-4307-8646-b42ae2d31175/image.png)

We can form any dynamic query like this using any property name in the model class. But make sure that the query name is unique. If that is not unique, you will get multiple movies with the same `id` or `name`.

# 🎯 Review Part

## Review Layers

### Review Service

Creating the `Review` part is a lot similar to the `Movie` part. Let's make `ReviewController`, `ReviewService`, and `ReviewRepository`.

Inside the `ReviewService`, we will create a method called `createReview()`.

The `createReview()` method takes two parameters, `String reviewBody` and `String imdbId`.

The logic for this method is,

1. Look for the movie with the given `imdbId`
2. We will create a new review and associate that review with the movie that we found with the `imdbId`

![](https://velog.velcdn.com/images/devbang/post/d8077a1f-86af-4dff-a9d7-3f1035d69cc5/image.png)

### Review Constructor

If you remember the previous post, we created the `Review` class but didn't go over the last part, which was the constructor.

> from the previous post
> ![](https://velog.velcdn.com/images/devbang/post/5c28c879-46e4-4269-9cb6-47b6b3e9d2d1/image.png)

We have `AllArgsConstructor` and `NoArgsConstructor` for the `Review` class, but since `Id`s are auto-generated, we can't pass an `id` to this class.

To solve this problem, we will create a custom constructor that only takes the `body`.

To create a custom constructor, right-click and look for the `generate` and the constructor. You will see the following window and select the body because we only need the `body` part.

![](https://velog.velcdn.com/images/devbang/post/9d0d53c8-b0e2-4c39-aaa6-0e12a9b2e9d3/image.png)

### Create a new Review

We have added a logic to create a new review and imagine we have a new review from the user.

The next step is associating a new review with one of the movies.

From the `MovieService`, we know that we need a reference to the `Repository` to communicate with the database.

Let's add a reference for `ReviewRepository` and wire it.

```java
@Autowired
private ReviewRepository reviewRepository
```

As we learned, one of the ways to communicate with the database is using the repository. The other way to communicate with the database is using a `template`. If you can't use `repository` for some reason or doesn't cut in, maybe the logic is too complex so that it can't be implemented within a `repository`, or if you can implement it with a `repository` but it is not suitable, we can use a `template`.

We can use this `template` to form a new dynamic query and do the job inside of the database without using a `repository`.

As same as the `repository`, wire it and add the following code.

```java
@Autowired
private MongoTemplate mongoTemplate;
```

To use this, add a `mongoTemplate.update()` and, in the parenthesis, decide which class will be updated.

We want to update one of the `Movies` with a new review. At the end of this method, we can see many operations by adding a dot.

![](https://velog.velcdn.com/images/devbang/post/81a938ee-6b12-4e4e-a6c6-71ba1b08eb58/image.png)

We want to use the `.matching()` operation to search the movie and set new criteria by following lines of code.

```java
mongoTemplate.update(Movie.class)
	.matching(Criteria.where("imdbId").is(imdbId))
```

The first `"imdbId"` is from the `mongoDB`, so it has to match exactly the same as the database. The second `imdbId` is the value we pass from the `String imdbId`.

Next, we will add an updated definition.

```java
mongoTemplate.update(Movie.class)
	.matching(Criteria.where("imdbId").is(imdbId))
    .apply(new Update().push("reviewIds").value(review))
```

It is important to remember that each `Movie` in the collection contains an array of `reviewIds`. Then, go to the database and `UPDATE` the `reviewIds` array and `PUSH` a new `reviewId` into the `reviewIds` array.

This logic's progress can be explained in the following steps.

1. If the application gets a new `review` from the user,
2. Check which movie we want to `UPDATE` with the `.matching()` operation.
3. We are updating a movie `WHERE` the `"imdbId"` of the movie in the database matches the `imdbId` that has been received from the user.
4. To apply this update, use the `.apply()` method.
5. Create a new update definition, and this will make the change inside of the database
6. The definition `Update` will push a new `reviewId` into the `reviewIds` array from the current `Movie`.
7. The value of the `Movie` class inside of the `reviewIds` will be updated with `value(reivew)`
8. The `Review review = new review(reviewBody);` is a review just created, and this will pushed into the `value()`.
9. This `value()` will passed into the `reviewIds` array.

Finally, add some `.first()` operation to prevent edge cases. The `.first()` will check we are getting a single movie and updating it.

Fix the `Review review =` part because when using `insert`, it returns the data we just pushed inside of the database. As we don't want to return the review here, fix it accordingly, and your code should be like this.

![](https://velog.velcdn.com/images/devbang/post/10cbbe67-918a-4e0c-a8c0-b48c3026f606/image.png)

To sum up,

- Create a new `mongoTemplate` and use the template to update the `Movie` with a new `review` that we just pushed using the `reviewRepository`.

### Review Controller

Inside the controller, we will have only one method.
Unlike `MovieController`, we don't have to read the review with the controller, but we need to update it.

To update the review, we will need a `POST` method.

Annotate it as a `@RestController` and add a `@RequestMapping("/api/v1/reviews")` to the `ReviewController`.

> Creating endpoints will depend on the projects or the developer's preferences. We may choose to organize it differently, but the instructor first decides to use `/movies` since the review form will be inside the movie details page or the page where the user sees a single movie. We can request the `movies` endpoint instead of creating a new `reviews` endpoint. But later, the instructor changed it to `/reviews`.

The rest of the part is much similar to the `MovieController`. We will wire the `ReviewService`, and create a new `POST` mapping.

The public method will return a `ResponseEntity`, and the type of this entity will be a type `<Review>`.

Name method like `createReview()` and this method takes `@RequestBody Map<String, String> payload`.

> This is different from the `Service` class's createReview. If you need clarification, you can name it differently.

This means the framework should convert the data we get from the `request body` to the map of `key(string):value(string)`. We will also name it a `payload`.

Inside of the method, return `new ResponseEntity<Review>` with the service class. This `reviewService` class is used with its own `createReview()`, and it takes two parameters.

The first one is `payload.get("reviewBody")`, and the second one is `payload.get("imdbId")`. Pass the `HttpStatus` with the `CREATED` code at the end. Since we created a new review, it should pass `201` instead of `200`.

```java
public class ReviewController {
    @Autowired
    private ReviewService reviewService;

    @PostMapping
    public ResponseEntity<Review> createReview(@RequestBody Map<String, String> payload) {
    	return new ResponseEntity<Review>(reviewService.createReview(payload.get("reviewBody"), payload.get("imdbId")), HttpStatus.CREATED);
    }
}
```

If you follow along, your code should be similar to the following picture.

![](https://velog.velcdn.com/images/devbang/post/747ce791-fd24-4f3e-990c-1cd280ca7ae5/image.png)

## Test with Postman

### Create a Collection and Requests

Since this is not a course for the postman, I will not go into details of the postman.

Create the `movie-api` collection and create some `GET` requests.

![](https://velog.velcdn.com/images/devbang/post/f960dd70-a597-4003-a5c0-1b1bd711f84b/image.png)

If you don't know how to create the collection, type it in the red circle to create it.

Next, type the address like we usually do in the browser. If you see the information below, it means your `GET` request works fine.

![](https://velog.velcdn.com/images/devbang/post/9efd8325-441a-4b2a-8ae3-9804158f002b/image.png)

If you created the `GET` request for the `POST` method, you can change it.

![](https://velog.velcdn.com/images/devbang/post/0b95f6ef-9e1d-4ad1-8db1-8d092124faf3/image.png)

Select the `raw` data and choose the `JSON` format. Type the raw `JSON` data like below and send the request.

![](https://velog.velcdn.com/images/devbang/post/73da9c70-ff70-4461-ba8a-c82772218261/image.png)

Make sure the key of the `JSON` data exactly matches the two keys inside of the `payload`.

```json
{
  "reviewBody": "I really enjoyed the movie. It's a good family movie!",
  "imdbId": "tt10298840"
}
```

The `createReview(payload.get("reviewBody"), payload.get("imdbId")), HttpStatus.CREATED);` part will receive the `JSON` data from the user and convert it into a `Map`. The `Map` takes keys and values as `String`.

From the `Map <String, String>`, we are able to access the `reviewBody` and `imdbId`, which are the `string`.

Through the `ReviewService` layer, we can create a new review on the database. Update the movie with the current review, and return the review.

Also, the `ReviewRepository` works as an intermediary layer between the `Service` class and the database.

Now, if we send the `POST` request to the server, we will see the screens below.

![](https://velog.velcdn.com/images/devbang/post/78ef0d72-10a8-44ad-b024-8a9485f46954/image.png)

We have `201 created` in green text, and in the box, it shows the `reviewIds` format. If you wish, you can check the single movie with the same `id`, and indeed it has the review that we just created.

![](https://velog.velcdn.com/images/devbang/post/496fa2e3-2385-47aa-b144-a2c603b8530e/image.png)

# 📌 Takeaway

Through this post, I learned about

- how to set up the multiple layers in Java Spring Boot
- the relation between the `API Controller`, `Repository`, and the `Service` class
- how to create `REST API` in Java Spring Boot
- how to reference each class by `@Autowired`
- the role of the `Controller`, `Service`, and `Repository`
- how to access a single item from the list of items by `@PathVariable`
- how the `Optional` class works in Java
- the dynamic query creation
- how to make the custom constructor only use the `body`
- the difference between the `Repository` and the `Template`
- how to use the operations by using the `template`
- how to `GET` a review from the user, and `POST` to the database with the `Update` definition
- how to convert the data from the `request body` to a `Map`

# 💻 Solution

- In my case, I needed to change the `reviewIds` to `review` in order to update and communicate with the front-end
- Also added `@CrossOrigin(origins = "*")` to each `Controller` to resolve the cross-origin error.
- If anyone doesn't want to use `ngrok`, this might be the alternative solution.

# 🔖 Review

- In Java Spring, we use `Controller` to create REST API.
  We can create the `GET` method by `@GetMapping`.
  By default, it sets to the parent value`("/api/v1/movies")`, but we can map to a specific address by adding parenthesis.

### The relation between the Layers

- The `Controller` uses a reference from the `Service`.
  The `Controller` is the API layer and is only responsible for `GET` and `POST` methods.
  When the controller gets the request, it calls the method from the `MovieService` class and returns the list of the movies.

- The `Service` communicates with the database by using `Repository`
  Most of the business logic goes under the `Service` class.
  The `MovieService` gets the list of the movies from the `MovieRepository` and passes it to the `MovieController`.

- The `Repository` class is a data access for the API
  The `MovieRepository` communicates with `mongoDB` and gets the data from it. It passes the data to the `MovieService`.

### Optional class

- If we use a built-in method like `findById`, sometimes we have to return null in case of the `id` doesn't exist or we can't find the movie.
- In this case, we can use `Optional<>` to let the framework know this can be `null` if the result doesn't exist.

### Dynamic Query

- The built-in methods are not always the solution for the business logic.
- In this case, we need to create a query like `findMovieByImdbId`.
- When the convention matches, the framework automatically catches what we are trying to do and connects it to the correct data.

### Create Review

- To create a review, we need several steps.
- More complex business logic goes under the `template`
- Inside of the `Review`, we need a custom constructor because we are going to use only `review body`.
- The `Service` logic is following:

  1. Create a new review associated with the `reviewRepository` and the `insert` methods.
  2. The application gets a new review from the user.
  3. Check the movie with `imdbId`, and this matches the database
  4. Create an updated definition.
  5. The update definition will handle the changes inside of the database
  6. With the `.apply()` method, the `Update()` will push to the `reviews` in the database with the value just created from 1.
  7. For the error handling, put `.first()` to avoid updating unintended movie.

### POST request

- To send a `POST` request to the browser, use `@PostMapping`.
- The `@RequestBody` annotation converts the data from the request body into the desired format.
- The `createReview()` comes from the `reviewService`, which takes two values, `String reviewBody, String imdbId`.
- Lastly, return the request with the `HttpStatus.CREATED` since we need 201, not 200.

### Front-end part

For the front-end part, I created with React, but since it is basic and didn't communicate with Spring Boot, I decided to skip the whole process in the post. Instead, I will go over specific components.

The below code is the main logic for the application.

![](https://velog.velcdn.com/images/devbang/post/08c03197-9cf4-4dc5-a257-bb2cb885d759/image.png)

The application gets data from the backend endpoint and uses hooks to set the list of movies, a single movie, and the list of reviews.

![](https://velog.velcdn.com/images/devbang/post/7d0362e5-64cd-4da2-8029-3ca005e99710/image.png)

Then, it uses those values as a prop for each component.

Let's look at the `Reviews` component since it is the core feature.

![](https://velog.velcdn.com/images/devbang/post/008edf12-91da-4619-9fff-83bf6db16d28/image.png)

It gets four props from the App.js. When the user visits the review page(`/Reviews/:movieId`), the `useEffect()` hook fires with the `getMovieData`.

And if the user types the review, we have `const revText = useRef();` to keep the text area data(review text), use `useParams()` to extract the `movieId` from the URL and save it to a variable.

![](https://velog.velcdn.com/images/devbang/post/7a1c463a-c5f3-41ae-a888-c46442898c60/image.png)

In the `addReview` function, we use the current reference from `revText`. After getting the data from the browser, we send the `POST` request to the backend.

> A quick reminder: the `creatReview()` class takes two values, which is the reviewBody and imbdId
> ![](https://velog.velcdn.com/images/devbang/post/5a090fe1-7f08-45a6-8f50-da787d94a5dc/image.png)

The `updatedReviews` variable checks if the `reviews` prop from the App is not `null` and updates accordingly.

If the `reviews` is not `null`, it uses the existing data from the `reviews` and updates only the `body` with the `review.value`(which is the same as the reviewBody).
If the `reviews` is `null`, we simply add the `body` with the data.

Lastly, the `setReviews()` changes the current review with the `updatedReviews`.

![](https://velog.velcdn.com/images/devbang/post/2e625c77-5f0d-4456-acec-28eace34a55e/image.png)

The event `e` comes from the form, and the `ReviewForm` component takes three props from here.

![](https://velog.velcdn.com/images/devbang/post/e4e20e5b-15da-4bf6-8f96-816cab4ac225/image.png)

The react-bootstrap library controls the form. The `Form.Control` needs some props, and the `ref` comes from the `Review` component's `useRef()` hook.

That is the basic logic for the `Review` part for the front-end. Below are two screenshots of some previews for the page.

- The Sample landing page
  ![](https://velog.velcdn.com/images/devbang/post/5a8df0a3-26d0-4263-9e3b-32c7726dfe10/image.png)

- The review page
  ![](https://velog.velcdn.com/images/devbang/post/a4d84231-e440-42a3-9d96-41efeec6626a/image.png)

You can find my code from the [Repository](https://github.com/bybang/java_movie_review)
