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
  * ingredient - Ingredient 
  * instructions - string
  * title - string
* User - User
  * firstname - string
  * lastname - string
  * email - string
  * password - hashed string
* Ingredient - Ingredient
  * name - string

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



