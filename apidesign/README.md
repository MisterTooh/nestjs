```
/* ************************************************************************** */
/*                                                                            */
/*                                                        :::      ::::::::   */
/*   API Design                                         :+:      :+:    :+:   */
/*                                                    +:+ +:+         +:+     */
/*   By: hawadh <hawadh@student.42abudhabi.ae>      +#+  +:+       +#+        */
/*                                                +#+#+#+#+#+   +#+           */
/*   Created: 2022/08/31 19:59:05 by hawadh            #+#    #+#             */
/*   Updated: 2022/08/31 19:59:05 by hawadh           ###   ########.fr       */
/*                                                                            */
/* ************************************************************************** */
```

#  **`Used Car Pricing API`**

In this project we will start with API design, and design a Used Car Pricing API.

Best begin by outlining what modules we will need to create to wrap up the different things we create inside our application.

Each Module will contain a Controller, Service and Repository. We will use a database.

We will also begin using Databases other than our `.json` file method.

To begin with we will start with TypeORM -> SQLite for ease of setup but will eventually move to TypeORM->Postgres

### `Why do we create modules?`
1.  

Users will be able to:
1. Signup with email and password
2. Get an estimate for how much their car is worth based on the Make/Model/Year and Mileage.
3. Users can report what they sold their vehicles for.

Admins have to approve reported sales.

##  `Requirements`

Generate new project:
```
$ nest new "projectname"
$ // choose >npm

$ nest g module users
$ nest g module reports
$ nest g service users
$ nest g service reports
$ nest g controller users
$ nest g controller reports
$ npm install @nestjs/typeorm typeorm sqlite3  // 3 different libraries
```
- `@nestjs/typeorm`:
  - Library makes typeorm and nestjs work together
- `typeorm`
  - typeorm library
- `sqlite3`
  - Database implementation

# **`Design Documentation`**

**Note:** Numbered request, and Body/Query String data content

### `Method and Route:`

1.  POST `/auth/signup` - Create a new user and sign-in
2.  POST `/auth/signin` - Sign in as an existing user
3.  GET  `/reports` - Get an estimate for the cars value
4.  POST `/reports` - Report how much a vehicle sold for
5.  PATCH `/reports/:id` - Approve or reject a report submitted by a user

### `Body or Query String:`

1.  Body - { email, password }
2.  Body - { email, password }
3.  Query String: make, model, year, mileage, longitude, latitude
4.  Body - { make, model, year, mileage, longitude, latitude, price }
5.  Body - { approved }


# `Modules`

### `Three Modules will be required:`

1.  AppModules (Which will have connection to SQLite DB) which will hold:
    -   UsersModule:
        - User Entity: Lists the different properties that User has. (No Functionality)
        - Users Repository: Methods to find, update, delete, create User.
    -   ReportsModule:
        - Report Entity: Lists the different properties that a Report has. (No functionality)
        - Methods to find, update, delete, create a Report.
2.  We will create a connection  to the database in the AppModule:
    - The connection to the Database will automatically be connected to the UsersModule and ReportsModule.
3.  We will create Entity files. It is similar to a model:
    - Defines a single kind of resource that we want to store inside our application.
    - Will list out all the different properties our Entities should have.
    - We will feed these into nest to create the Repositories.

### `The Two Sub Modules Containing the below:`
  - #### `Controllers`
    1.  Users Module: POST `/auth/signup` & POST `/auth/signin`:
        - Users Controller
    2.  Reports Module: GET `/reports/`, POST `/reports/` & PATCH `/reports/:id`
        - Reports Controller
  
- #### `Services`
  1.  Users Module: POST `/auth/signup` & POST `/auth/signin`:
      - Users Service
  2.  Reports Module: GET `/reports/`, POST `/reports/` & PATCH `/reports/:id`
      - Reports Service
- #### `Repositories`
  1.  Users Module: POST `/auth/signup` & POST `/auth/signin`:
      - Users Repository
  2.  Reports Module: GET `/reports/`, POST `/reports/` & PATCH `/reports/:id`
      - Reports Repository

## `Connecting SQLite DB`

1.  Import `TypeOrmModule` from `@nestjs/typeorm`  line `#2` in `/src/app.module.ts`.
2.  Add `TypeOrmModule.forRooot()` in list of imports on line `#9`in `/src/app.module.ts`.
3.  Pass configuration object into `forRoot({[>Config Object here<]})` line `#9`:
    - Properties in the `forRoot` configuration:
        - type: `'sqlite'`, we want typeorm to creatte a sqlite database
        - database: `'db.sqlite'`,
        - entities: `[]`, will be an array that contains all the different entities we want to store in our application.
        - synchronize: `true`;
4.  Notice `db.sqlite` file was created. SQLite is a file based database, and will store all information in a database within a single file.

## `Creating The Users and Reports Entity File`

Inside the `UsersModule` and `ReportsModule` we will create the entity files, TypeORM and nest will automatically connect and create the Repositories.

### `3 Steps to create an Entity file`


### Using Users as an example:
1.  Create an entity file, and create a class in it that lists all the properties that your entity  will have:
    - Create Entity file eg; `/src/users/user.entity.ts` and import decorators from `typeorm`:
        - Entity:
        - Column:
        - PrimaryGeneratedColumn:
    - Create and export class and list it's properties.
    - Add decorators to the class and properties.
2.  Connect the entity to its parent module. this creates a repository:
    - In `/src/users/users.module.ts/` import `TypeOrmModule` from `@nestjs/typeorm`.
    - Import the `User` entity from `/src/users/user.entity.ts`.
    - Add `imports: [TypeOrmModule.forFeature([User])]` line `#8` in `/src/users.module.ts` to the `UsersModule` `@Module({})`.
3.  Connect the entity to the root connection (in app module):
    - Import the `User` Entity in`/src/app.module.ts`
    - Add `User` to the Entity type array. `entities: [User]`. Line `#13` in `/src/app.module.ts`.

4.  Repeat same three steps for every entity you wish to create.

In Theory TypeORM and Nest should have created a Repository for each of these Modules for us.

# `Databases`

Nest works fine with any ORM/Database but works well with TypeORM and Mongoose.

1.  TypeORM:
    - SQLite (Single File Database, See Connecting SQL Above).
    - Postgres
    - MySQL
    - MongoDB (No SQL database)

2.  Mongoose:
    - MongoDB
