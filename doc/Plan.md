# Recipe Sharing Application v0.0.1 Development Plan
## Analysis of Requirements
### Tools

* This application will use Django on the backend and React on the front
* Poetry will be used for dependency management
* SQLite will be used for data management

### As a user, I should be able to...

* Create a new account/log into an existing one
* Manage account settings
  * Changing name/email/password
* Upload a new recipe
* Update a recipe I have uploaded
* Delete a recipe I have uploaded
* View recipes I have uploaded
* View public recipes uploaded by other accounts
* Comment on recipes posted
* Delete a comment I have posted
* Download a recipe (my own or a publicly posted one)
* Delete my account
* Navigate through the application easily

### Data used by the application

* Recipe - Recipe
  * description - string
  * ingredient - string 
  * instructions - string
  * title - string
  * comments - Comments
  * public - boolean
  * user - User
* User - User
  * firstname - string
  * lastname - string
  * email - string
  * password - hashed string
* Comment - Comment
  * poster - User
  * header - string
  * comment - string

### Enpoints needed for the application

* Home page - GET
* Account page - GET
* Recipe page - GET
* Create recipe page - GET
* Create recipe - POST
* Update recipe page - GET
* Update recipe - POST
* Delete recipe - DELETE
* Comment - POST
* Delete a comment - DELETE

## Design
### Models
#### Recipe

* Contains the following data
  * Id - used to identify recipes
  * Title - a name for the recipe
  * Ingredients - the list of ingredients needed for the recipe
    * All ingredients must be separated by a comma
  * Instructions - a step by step guide to the recipe
    * Will be taken in as one string
  * Comments - a one to many relationship with the `Comment` model
    * A comment belongs to one post, a post can have many comments
  * Public - whether or not the posted recipe is visible to other users
  * User - the user that posted the recipe

```python
class Recipe(Models):
  id = integer(primary key)
  title = text
  ingredients = text
  instructions = text
  comments = foreignkey(Comment)
  public = boolean
  user = foreignkey(User)
```

#### Comment

* Contains the following data
  * Id - used to identify the comment
  * user - the user that posted the comment
  * header - a header that sums up the comment
  * comment - the actual comment

```python
class Comment(Models):
  id = integer(primary key)
  user = foreignkey(User)
  header = text
  comment = text
```

### Endpoints

* All pages will have the following
  * A navbar located on the left side of the screen
  * A topbar that will contain the name of the site

#### GET Home Page

* The home page will display publicly posted recipes
* When the front end displays the home page, it will make a request to the api
* The back end will then return a list of the publicly posted recipes from the database

```python
def get_public_recipes(req):
  recpies = Recipe object(public = true)
  recipes = model to dictionary(recipe) for recipe in recipes
  return JsonResponse({recipes: recipes})
```

#### GET Account Page

* The account page will display recipes posted by the user
* The account page will also display the username, as well as a button to access user settings
* When the front end displays the account page, the back end will provide all recipes posted by the user

```python
def account(req):
  recipes = Recipe objects(user = req.user)
  recipes = model to dictionary(recipe) for recipe in recipes
  return JsonResponse({recipes: recipes})
```

#### GET Recipe

* The recipe page will display all the details of a specific recipe
* The front end will make a request for a specific recipe's data
* This endpoint will be used for both the recipe page and the update recipe page

```python
def get_recipe(req, id):
  recipe = Recipe object(id = id)
  return JsonResponse({recipe: recipe})
```

#### POST Recipe Create

* This endpoint will be given data from the front end for the purpose of putting it into the Recipe data table
* First thing the endpoint will have to do is use `json.loads` to parse the json into a python dictionary
* The endpoint can then create a new Recipe entry

```python
def create_recipe(req):
  body = json.loads(req.body)
  new_recipe = Recipe(
    title = body title
    ingredients = body ingredients
    instructions = body instructions
    public = body public
  )

  save new recipe
```

#### DELETE recipe deletion

* This endpoint will receive the id of the recipe to be deleted from the front end
* It will then delete the specific recipe from the database

```python
def delete_recipe(req, id):
  recipe = Recipe object(id = id)
  delete recipe
  return JsonResponse({success: true})
```

#### POST comment

* This endpoint will save a new comment into the database

```python
def post_comment(req, id):
  body = json.loads(req.body)
  new_comment = Comment(
    header = body header
    comment = body comment
  )

  save new comment
```

#### DELETE comment deletion

* This endpoint delete's a given comment

```python
def delete_comment(req, id):
  comment = Comment object(id = id)
  delete comment
  return JsonResponse({success: true})
```

### Utilities

* These ustilities are used by the front end to make requests to the backend
* They are written in plain JavaScript
* This helps keep code organized and concise
* In this application, only app.js is necessary

#### app.js

* This is the main connection point between the frontend and the backend
* It has 5 methods:
  * makeRequests
    * This method performs the action of making a request to the backend api
    * It has the parameters `url`, `method`, and `body`
      * `url` is the desired enpoint the frontend is wanting to access
      * `method` is what method the frontend is using (e.g. GET, POST)
      * `body` is the body of the http request. This is not needed for things like GET requests
    * The method creates the GTTP request, then makes a fetch request
    * The method returns the response in json

```js
const makeRequest = (url, method, body) => {
  const request = {
    method,
    headers,
    body
  }

  const response = await fetch(url, request)
  return response
}
```

  * method functions
    * The method functions are mainly used to help make code more readable
      * Rather than calling `makeRequests`, you call app.get, or app.post
    * The remaining four functions will follow the same pattern
      * Take url and body as arguments
      * return makeRequest

```js
const method = (url, body) => {
  return makeRequests(url, method, body)
}
```

### Components

* These are functional parts of the application that are not page-specific, such as navbars and banners

#### Sidebar

* I will be bringing the sidebar that I created in another application and styling it differently
* It works by accepting a list of dictionaries that contain the page name and the url for that page as a prop
* The `sidebar.jsx` then takes that prop and displays it in a sidebar on the left side of the screen


### Pages

#### Home

* The home page is the first page the user sees when they login to the app
* It will show recipes in order from most recently posted first
