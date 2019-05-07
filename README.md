[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Rails Many to Many

Let's build a rails application!

## Objectives

By the end of this, developers should be able to:

- Create a many-to-many relationship with existing models.
- Create and utilize a join table.
- Create a new resource using `scaffold`.
- Compare and contrast objects created in the join table versus those that were not.

## Steps for Adding a Joins Table

1. Generate necessary resource for the relationship
1. Generate the joins table with the 2 foreign keys
1. Add relationship macros to all 3 models
1. Update all 3 controllers
1. Update all 3 views

## Methodical, Error-Driven Development

Status checks will be frequent. As developers we want to be meticulous and make
sure we're getting errors where expected as we build our server.

This is called "error-driven development". The goal is to see an error, and to
learn what error to expect. Embrace the errors, and they will tell you what
your next task is.

Follow the steps outlined in good Error Driven Development

1. Route
    - Test the route, see that a route does not exist
    - Add the route
    - Test the route, see that a route does exist
1. Controller
    - Test the route, see that a route does exist but controller does not
    - Add the controller
    - Test the route, see that a controller exists
1. Model
    - Test the route, see that a controller exists but model does not
    - Add the model
    - Test the route, see that a Model exists
1. Migrations
    - Test the route, see that a Model exists but migrations must be run
    - Run migrations
1. View
    - Test the view, see that it does not exist
    - Add the view
1. Test Feature
    - Test the route, ensure actions are successful
    - Use Rails Console to ensure all data persists as expected
    
## Demo: Books

### User Stories 

- Version 2:
  - As a user, I want to view a single book
  - As a user, I want to view all books
  - As a user, I want to create a book with a title
  - As a user, I want to edit a book's title
  - As a user, I want to delete a book
  - As a user, I want to view an author
  - As a user, I want to view all authors
  - As a user, I want to create an author with a given name and family name
  - As a user, I want to edit an author's given name and family name
  - As a user, I want to delete an author
  - As a user, I want to assign a single author to a book
  - As a user, I want to assign multiple books to an author

- Version 3:
  - As a user, I want to view a single book
  - As a user, I want to view all books
  - As a user, I want to create a book with a title
  - As a user, I want to edit a book's title
  - As a user, I want to delete a book
  - As a user, I want to view an author
  - As a user, I want to view all authors
  - As a user, I want to create an author with a given name and family name
  - As a user, I want to edit an author's given name and family name
  - As a user, I want to delete an author
  - As a user, I want to assign a single author to a book
  - As a user, I want to assign multiple books to an author
  - As a user, I want to view a borrower
  - As a user, I want to view all borrowers
  - As a user, I want to create a borrower with a given name and family name
  - As a user, I want to edit a borrower's given name and family name
  - As a user, I want to delete a borrower
  - As a user, I want borrowers to loan many books
  - As a user, I want books to be loaned by many borrowers
  
### Entity Relationship Diagram (ERD) 

#### Version 2

**Author** has many **Books**

**Books** belong to **Author**

`Authors` -|--< `Books`

<table style="display:inline">
  <th colspan="2" style="text-align:center">Books</th>
  <th colspan="2" style="text-align:center">
  Authors
  </th>
  <tr>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
  </tr>
  <tr>
    <td>title</td>
    <td>string</td>
    <td>first_name</td>
    <td>string</td>
  </tr>
  <tr>
    <th> author_id </th>
    <th> foreign key </th>
    <td>last_name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>datetime</td>
    <td>created_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td>datetime</td>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
</table>

#### Version 3

`Authors` -|--< `Books`

`Books` -|--< `Loans` >--|- `Borrowers`

**Books** belong to **Author**

**Author** has many **Books**

**Books** have many **Borrowers** through **Loans**

**Borrowers** have many **Books** through **Loans**

**Loans** belongs to both a **Book** and a **Borrower**

<table style="display:inline">
<th colspan="2" style="text-align:center">
Books
</th>
  <th colspan="2" style="text-align:center">Authors</th>
  <tr>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
  </tr>
  <tr>
    <td>title</td>
    <td>string</td>
    <td>first_name</td>
    <td>string</td>
  </tr>
  <tr>
    <th>author_id</th>
    <th>foreign key</th>
    <td>last_name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>datetime</td>
    <td>created_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td>datetime</td>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
</table>

<br>

<table style="display:inline">
  <th colspan="2" style="text-align:center">
  Loans
  </th>
  <th colspan="2" style="text-align:center">
  Borrowers
  </th>
  <tr>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
  </tr>
  <tr>
    <th>borrower_id</th>
    <th>foreign key</th>
    <td>first_name</td>
    <td>string</td>
  </tr>
  <tr>
    <th>book_id</th>
    <th>foreign key</th>
    <td>last_name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>datetime</td>
    <td>created_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td>datetime</td>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
</table>

## Code Along: Hospital

### User Stories

- Version 2
  - As a user, I want to view a single patient
  - As a user, I want to view all patients
  - As a user, I want to create a patient with a first name, last name,
    diagnosis and born on
  - As a user, I want to edit a patient's first name, last name, diagnosis and
    born on
  - As a user, I want to delete a patient
  - As a user, I want to view a doctor
  - As a user, I want to view all doctors
  - As a user, I want to create a doctor with a given name, family name, zip
    code and specialty
  - As a user, I want to edit a doctor's given name, family name, zip code and
    specialty
  - As a user, I want to delete a doctor
  - As a user, I want to assign a single doctor to a patient
  - As a user, I want to assign multiple patients to a doctor

- Version 3
  - As a user, I want to view a single patient
  - As a user, I want to view all patients
  - As a user, I want to create a patient with a first name, last name,
    diagnosis and born on
  - As a user, I want to edit a patient's first name, last name, diagnosis and
    born on
  - As a user, I want to delete a patient
  - As a user, I want to view a doctor
  - As a user, I want to view all doctors
  - As a user, I want to create a doctor with a given name, family name, zip
    code and specialty
  - As a user, I want to edit a doctor's given name, family name, zip code and
    specialty
  - As a user, I want to delete a doctor
  - As a user, I want to assign a single doctor to a patient as their primary care physician
  - As a user, I want to assign multiple patients to a doctor as their primary care recipient
  - As a user, I want to create an appointment between a patient and a doctor
  - As a user, I want to delete an appointment between a patient and a doctor
  
### Entity Relationship Diagrams

#### Version 2

**Doctor** has many **Patients**

**Patients** belong to **Doctor**

`Patients` -|--< `Doctors`

<table style="display:inline">
  <th colspan="2" style="text-align:center">Patients</th>
  <th colspan="2" style="text-align:center">
  Doctors
  </th>
  <tr>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
  </tr>
  <tr>
    <td>first_name</td>
    <td>string</td>
    <td>first_name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>last_name</td>
    <td>string</td>
    <td>last_name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>diagnosis</td>
    <td>string</td>
    <td>zip_code</td>
    <td>string</td>
  </tr>
  <tr>
    <td>born_on</td>
    <td>date</td>
    <td>specialty</td>
    <td>string</td>
  </tr>
  <tr>
    <th>doctor_id</th>
    <th>foreign key</th>
    <td>created_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>datetime</td>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td>datetime</td>
    <td></td>
    <td></td>
  </tr>
</table>

#### Version 3

**Patients** have many **Doctors** through **Appointments**

**Doctors** have many **Patients** through **Appointments**

**Appointments** belongs to both a **Patient** and a **Doctor**

**Patients** belongs to **Doctor** as _Primary Care Physician_

**Doctor** as _Primary Care Physician_ has many **Patients**

```
Patients -|--< Appointments >--|- Doctors
 \/                                    |
  --------------------------------------
```

<table style="display:inline">
  <th colspan="2" style="text-align:center">Patients</th>
  <th colspan="2" style="text-align:center">
  Appointments
  </th>
  <th colspan="2" style="text-align:center">
  Doctors
  </th>
  <tr>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
  </tr>
  <tr>
    <td>first_name</td>
    <td>string</td>
    <th>patient_id</th>
    <th>foreign key</th>
    <td>first_name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>last_name</td>
    <td>string</td>
    <th>doctor_id</th>
    <th>foreign key</th>
    <td>last_name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>diagnosis</td>
    <td>string</td>
    <td>created_at</td>
    <td>datetime</td>
    <td>zip_code</td>
    <td>string</td>
  </tr>
  <tr>
    <td>born_on</td>
    <td>date</td>
    <td>updated_at</td>
    <td>datetime</td>
    <td>specialty</td>
    <td>string</td>
  </tr>
  <tr>
    <th>doctor_id</th>
    <th>foreign key</th>
    <td></td>
    <td></td>
    <td>created_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>datetime</td>
    <td></td>
    <td></td>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td>datetime</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

## Lab: Cafeteria 

### User Stories

- Version 2
  - As a user, I want to view a single ingredient
  - As a user, I want to view all ingredients
  - As a user, I want to create an ingredient with name and unit
  - As a user, I want to edit an ingredient's name and unit
  - As a user, I want to delete an ingredient
  - As a user, I want to view a recipe
  - As a user, I want to view all recipes
  - As a user, I want to create a recipe with a name and description
  - As a user, I want to edit a recipe's name and description
  - As a user, I want to delete a recipe
  - As a user, I want to assign a single recipe to an ingredient
  - As a user, I want to assign multiple ingredients to a recipe

- Version 3
  - As a user, I want to view a single ingredient
  - As a user, I want to view all ingredients
  - As a user, I want to create an ingredient with a name
  - As a user, I want to edit an ingredient's name
  - As a user, I want to delete an ingredient
  - As a user, I want to view a recipe
  - As a user, I want to view all recipes
  - As a user, I want to create a recipe with a name and description
  - As a user, I want to edit a recipe's name and description
  - As a user, I want to delete a recipe
  - As a user, I want to assign multiple ingredients to a recipe
  - As a user, I want to assign multiple recipes to an ingredient
  - As a user, I want to create a RecipeIngredient with an amount and unit that
    is specific for an ingredient being used in a specific recipe
    
### Entity Relationship Diagrams

#### Version 2

**Recipe** has many **Ingredients**

**Ingredients** belongs to **Recipe**

`Recipes` -|--< `Ingredients`

<table style="display:inline">
  <th colspan="2" style="text-align:center">Ingredients</th>
  <th colspan="2" style="text-align:center">
  Recipes
  </th>
  <tr>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
  </tr>
  <tr>
    <td>name</td>
    <td>string</td>
    <td>name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>unit</td>
    <td>string</td>
    <td>description</td>
    <td>string</td>
  </tr>
  <tr>
    <th>recipe_id</th>
    <th>foreign key</th>
    <td>created_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>datetime</td>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td>string</td>
    <td></td>
    <td></td>
  </tr>
</table>

#### Version 3

**Ingredients** have many **Recipes** through **RecipeIngredients**

**Recipes** have many **Ingredients** through **RecipeIngredients**

**RecipeIngredients** belongs to both an **Ingredient** and a **Recipe**

`Ingredients` -|--< `RecipeIngredients` >--|- `Recipes`

<table style="display:inline">
  <th colspan="2" style="text-align:center">Ingredients</th>
  <th colspan="2" style="text-align:center">
  RecipeIngredients
  </th>
  <th colspan="2" style="text-align:center">
  Recipes
  </th>
  <tr>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
    <td>id</td>
    <td>primary key</td>
  </tr>
  <tr>
    <td>name</td>
    <td>string</td>
    <td>amount</td>
    <td>integer</td>
    <td>name</td>
    <td>string</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>datetime</td>
    <td>unit</td>
    <td>string</td>
    <td>description</td>
    <td>string</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td>datetime</td>
    <th>recipe_id</th>
    <th>foreign key</th>
    <td>created_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <th>ingredient_id</th>
    <th>foreign key</th>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td>created_at</td>
    <td>datetime</td>
    <td>updated_at</td>
    <td>datetime</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td>updated_at</td>
    <td>datetime</td>
    <td></td>
    <td></td>
  </tr>
</table>

## Tasks

Developers should run these often!

- `bin/rake routes` lists the endpoints available in your API.
- `bin/rake test` runs automated tests.
- `bin/rails console` opens a REPL that pre-loads the API.
- `bin/rails db` opens your database client and loads the correct database.
- `bin/rails server` starts the API.

## Additional Resources

- Rails Docs
  - [ActiveRecord Association Basics#has_many :through](http://guides.rubyonrails.org/association_basics.html#the-has-many-through-association)
  - [ActiveRecord Validations](http://guides.rubyonrails.org/active_record_validations.html)
  - [ActiveRecord Callbacks](http://guides.rubyonrails.org/active_record_callbacks.html)

- Blogs
  - [ActiveRecord Joins](https://www.learneroo.com/modules/137/nodes/768)
  - [Complex has_many :through](http://nithinbekal.com/posts/complex-has-many-through/)
  - [Has many through - forms](https://learn.co/lessons/has-many-through-forms-rails)
