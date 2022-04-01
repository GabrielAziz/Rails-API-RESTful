# Rails API - RESTFUL 

In this README i will document my study of the course [Developing REST / RESTful APIs with Ruby on Rails](https://www.udemy.com/course/rubyonrails-api/) by [Jackson Pires](https://github.com/jacksonpires) and train my English in the processs.

***

## Sumary

<br>
<small>

* Exemple
* Exemple
* Exemple
* Exemple
* Exemple

</small>    

<br>

***

## Install

<br>

- **RVM**
    - Ruby: **2.7.0v**
    - Rails: **7.0.2v**
- **Postman**

<br>

***

## Creating your project

<br>

```
$ rails _2.7.0_ new project_name --api
```

### _2.0.7_

>the snippet forces the project to run with ruby 2.0.7, this argument is not required

<br>

### _api flag_

>the --api flag indicates that the project will be in api format, that is, we will only interact with the model and controller 

<br>

***

## Creating your fist CRUD

<br>

```
$ rails g scaffold contact name:string email birthdate:date
```
### _rails g scaffold_
> we create with **rails g scaffold** all the necessary files for crud (_routes,controller,model, migration_)

* **rails g** = rails generate (**generates something**)
* **rails d** = rails destroy (**destroy something**)

you can generate:
* rails **g controller** (**only controller**)
* rails **g model** name:type (**model and migration**)
* rails **g migration** CreateBook name:type (**only migration**)

<br>

### _contact_
> contact is the name of the created table that will be created, and the following arguments, their columns, represented by ***' name:type '***

* ***if the name does not have a type, its default typing will be :text***

<br>

***

## Up the API

<br>

```
$ rails db:create
$ rails db:migrate
$ rails s
```
* rails **db:create** = create your database (**in SQLite by defaut**) 
* rails **db:migrate** = read all migrations and modify your database 
* rails **s** = up the application (**in the port :3000**)
    * ctrl + c in your terminal to down the application

<br>

***

## Populating the database with fake database

<br>

### Installing the Faker gem

1. down the application (**crtl + c**)
2. in the Gemfile (**file in project**) add the line:
~~~ruby
1 gem "faker"
~~~

into the group:

~~~ruby
1 group :development, :test do
2 
3 end
~~~

result:

~~~ruby
1 group :development, :test do
2   gem "debug", platforms: %i[ mri mingw x64_mingw ]
3   gem "faker"
4 end
~~~
* ***merely ilustrative lines***
* ***your code can have more gems***

3.install the new gem(**faker**) with command:
```
$ bundle
```
* the **bundle** command **read your gemfile** and **manage your gems**

<br>

### Creating a new task

```
$ rails g task dev setup
```
* the command will **create** a new file **dev.rake** in **/lib/task**

the generated file will be:
~~~ruby
1 namespace :dev do
2   desc " "
3   task setup: :environment do
4
5   end
6 end
~~~

modify the content to:

~~~ruby
1  namespace :dev do
2    desc "Generate a fake datas "
3    task setup: :environment do
4  
5      puts "Registering contacts"
6      100.times do |i|
7        Contact.create!(
8          name: Faker::Name.name,
9          email: Faker::Internet.email,
10         birthdate: Faker::Date.between(from: 65.years.ago, to: 18.years.ago )
11       )
12     end
13     puts "Registred contacts"
14     
15   end
16 
17 end
~~~

<br>

### Unraveling the code
~~~ruby
2    desc "Generate a fake datas "
~~~
* **desc** is the **description** in (**$ rails -T**)

~~~ruby
6      100.times do |i|
7        
8          
9          
10        
11       
12     end
~~~
* **repeat** for **100 times** (**generate 100 Contacts**)

~~~ruby
7        Contact.create!(
8          name: ,
9          email: ,
10         birthdate:        
11       )
~~~
* **create** one Contact **in database** with the columns **name**, **email** and **birthdate**
* ( **!** ) to **show** the **error if any**

~~~ruby
8          name: Faker::Name.name,
9          email: Faker::Internet.email,
10         birthdate: Faker::Date.between(from: 65.years.ago, to: 18.years.ago )

~~~
* **assingns** each column **a fake data** with **faker** gem
* [**Github Faker GEM**](https://github.com/faker-ruby/faker)

<br>

### Run the task

```
$ rails dev:setup
```

<br>

### View contacts in Browser

```
$ rails s
```

* **open in browser** - http://localhost:3000/contacts

<br>

### Json result

~~~json
{

    "id": 1,
    "name": "Chan Rempel",
    "email": "doretta@ortiz.net",
    "birthdate": "1974-07-03",
    "created_at": "2022-03-31T10:50:04.801Z",
    "updated_at": "2022-03-31T10:50:04.801Z"

}
~~~

<br>

---

## Routes

- ***Only nuon and plurall***
- Methods HTTP
    - GET : **Select** one or all records
    - POST : **Insert** a new record
    - PATCH / PUT : **Update** one record
    - DELETE : **Delete** one record
- Routes
    - GET all records
        - URL: **/contacts**
        - LINK TO: contacts<b>#index</b>
    - GET one record
        - URL: **/contacts/(:id)**
        - LINK TO: contacts<b>#show</b>
    - POST a new record
        - URL: **/contacts**
        - LINK TO: contacts<b>#create</b>
    - PUT / PATCH a record
        - URL: **/contacts/(:id)**
        - LINK TO: contacts<b>#update</b>
    - DELETE a record
        - URL: **/contacts(:id)**
        - LINK TO: contacts<b>#delete</b>

<br>

>the URL may **be similar**, but what **changes** is the **method used**

>contacts<b>#index</b> indicates that the route **sends to** the **contacts controller** in the **method #index**

<br>

***To view all routes go to http://localhost:3000/rails/info/routes***

<br>

### Resources

>Create all routes of one CRUD

<br>

~~~ruby
resources :contacts
~~~
this is equal to
~~~ruby
get '/contacts', to: "contacts#index"
get '/contacts/:id', to: "contacts#show"
post '/contacts', to: "contacts#create"
put '/contacts/:id', to: "contacts#update"
patch '/contacts/:id', to: "contacts#update"
delete '/contacts/:id', to: "contacts#destroy"
~~~

<br>

---
## Render Json

<br>

### Active Support JSON
* ***encode***
> *convert a hash to Json string*
~~~ruby
ActiveSupport::JSON.encode({ team: 'rails', players: '36' })
# => "{\"team\":\"rails\",\"players\":\"36\"}"
~~~

* ***decode***
> *convert json to hash*
~~~ruby
ActiveSupport::JSON.decode("{\"team\":\"rails\",\"players\":\"36\"}")
# => {"team" => "rails", "players" => "36"}
~~~

<br>

### Active Model Serializers JSON

* ***as_json***
> *convert a object to hash*

~~~ruby
contact = Contact.first
contact.as_json
# => {"name"=>"joao", "email"=>"joao@gmail.com", "birthdate"=>"1969-12-17"}
~~~

* ***to_json***
> *convert hash to json string*
~~~ruby
hash = {"name":"joao", "email":"joao@gmail.com", "birthdate":"1969-12-17"}
hash.to_json
# => "{\"name\":\"joao\", \"email\":\"joao@gmail.com\", \"birthdate\":\"1969-12-17\"}"
~~~

<br>

### Render Json
~~~ruby
render json: @contact
~~~

this is equal to

~~~ruby
render: @contact.as_json.to_json
~~~
* ***the function render: dont exist***
<br>

---

## Status Code

### Status code HTTP 

* Info response ( **100 - 199** )
* Sucess response ( **200 - 299** )
* Redirect ( **300 - 399** )
* Client Error ( **400 - 499** )
* Server Error ( **500 - 599** )

[***More informations in with http status :***](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status)

<br>

### Status code Rails

to use the rails status code we do:

~~~ruby
status: :rails_code
~~~

exemple

~~~ruby
render json: @contact, status: :created
# return => 201, Created
~~~
* if the request **does not have a specified code**, it will return: **200 OK**

[***All rails status codes :***](http://www.railsstatuscodes.com/)

<br>

---

## Formating the Json

<br>

### Root

> if you want your response to **return** with the **creator element** include:

~~~ruby
root: true
~~~

exemple:

~~~ruby
# GET /contacts/1
  def show
    render json: @contact, root: true
  end
~~~

json result:

~~~json
{
    "contact": {
        "id": 1,
        "name": "Chan Rempel",
        "email": "doretta@ortiz.net",
        "birthdate": "1974-07-03",
        "created_at": "2022-03-31T10:50:04.801Z",
        "updated_at": "2022-03-31T10:50:04.801Z"
    }
}
~~~

<br>

### Only

> if you want your response to **return only** with some elements:

~~~ruby
only: [:element]
~~~

exemple:

~~~ruby
render json: @contact, only: [:name, :email]
~~~

json result:

~~~json
{
    "name": "Chan Rempel",
    "email": "doretta@ortiz.net",
}
~~~

<br>

### Except

> if you want your response to **return except** some elements:

~~~ruby
except: [:element]
~~~

exemple:

~~~ruby
render json: @contact, except: [:created_at, :updated_at]
~~~

json result:

~~~json
{
    "id": 1,
    "name": "Chan Rempel",
    "email": "doretta@ortiz.net",
    "birthdate": "1974-07-03"
}
~~~

### Merge

> if you want your response to **return except** with some extra elements in the ***#show*** ( ***one contact*** )::

~~~ruby
@contact.attributes.merge({element: ""})
~~~

exemple:

~~~ruby
def show
   render json: @contact.attributes.merge({author_name: "Gabriel", author_age:18})
end

~~~

json result:

~~~json
{
    "id": 1,
    "name": "Chan Rempel",
    "email": "doretta@ortiz.net",
    "birthdate": "1974-07-03",
    "created_at": "2022-03-31T10:50:04.801Z",
    "updated_at": "2022-03-31T10:50:04.801Z",
    "author_name": "Gabriel",
    "author_age": 18
}
~~~
### Map merge

> if you want your response to **return except** with some extra elements in the ***#index*** ( ***all contacts*** ):

~~~ruby
@contacts.map{ |contact| contact.attributes.merge({element: ""})}
~~~

exemple:

~~~ruby
def index
   @contacts = Contact.all

   render json: @contacts.map{ |contact| contact.attributes.merge({author_name: "Gabriel", author_age:18})}
end
~~~

json result:

~~~json
[
    {
        "id": 1,
        "name": "Chan Rempel",
        "email": "doretta@ortiz.net",
        "birthdate": "1974-07-03",
        "created_at": "2022-03-31T10:50:04.801Z",
        "updated_at": "2022-03-31T10:50:04.801Z",
        "author_name": "Gabriel",
        "author_age": 18
    },
    {
        "id": 2,
        "name": "Pamula Spinka",
        "email": "daron@dicki.net",
        "birthdate": "1959-11-21",
        "created_at": "2022-03-31T10:50:04.807Z",
        "updated_at": "2022-03-31T10:50:04.807Z",
        "author_name": "Gabriel",
        "author_age": 18
    }
]
~~~

<br>

### Methods

> using **methods** to **simplify**

in controller:
~~~ruby
render json: @contacts, methods: :author
~~~

in model:
~~~ruby
def author
   "Gabriel"  
end
~~~

exemple:

~~~ruby
  # GET /contacts
def index
   @contacts = Contact.all

   render json: @contacts, methods: :author 
end

  # GET /contacts/1
def show
   render json: @contact, methods: :author
end
~~~

json result:

~~~json
/* #index */
[
    {
        "id": 1,
        "name": "Chan Rempel",
        "email": "doretta@ortiz.net",
        "birthdate": "1974-07-03",
        "created_at": "2022-03-31T10:50:04.801Z",
        "updated_at": "2022-03-31T10:50:04.801Z",
        "author": "Gabriel"
    }
]
/* #show */
{
    "id": 1,
    "name": "Chan Rempel",
    "email": "doretta@ortiz.net",
    "birthdate": "1974-07-03",
    "created_at": "2022-03-31T10:50:04.801Z",
    "updated_at": "2022-03-31T10:50:04.801Z",
    "author": "Gabriel"
}
~~~

<br>

### Methods

> modifying **as_json** to **default**

in controller:
~~~ruby
render json: @contacts
~~~

in model:
~~~ruby
def author
    "Gabriel"  
end

def as_json(options = {})
    super(methods: :author, root: true)
end
~~~
*   ***super() to include the original as_json***


json result:

~~~json
/* #index */
[
    {
        "id": 1,
        "name": "Chan Rempel",
        "email": "doretta@ortiz.net",
        "birthdate": "1974-07-03",
        "created_at": "2022-03-31T10:50:04.801Z",
        "updated_at": "2022-03-31T10:50:04.801Z",
        "author": "Gabriel"
    }
]
/* #show */
{
    "id": 1,
    "name": "Chan Rempel",
    "email": "doretta@ortiz.net",
    "birthdate": "1974-07-03",
    "created_at": "2022-03-31T10:50:04.801Z",
    "updated_at": "2022-03-31T10:50:04.801Z",
    "author": "Gabriel"
}
~~~


