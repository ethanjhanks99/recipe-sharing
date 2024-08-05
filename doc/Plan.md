# Recipe Sharing Application Development Plan
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

### Endpoints and Pages

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

#### GET Recipe Page

* The recipe page will display all the details of a specific recipe
* The front end will make a request for a specific recipe's data

```python
def get_recipe(req, id):
  recipe = Recipe object(id = id)
  return JsonResponse({recipe: recipe})
```

