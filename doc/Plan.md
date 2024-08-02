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
* Download a recipe (my own or a publicly posted one)
* Delete my account

### Data used by the application

* Recipe - recipe
  * ingredient - string
  * instructions - string
  * name - string

