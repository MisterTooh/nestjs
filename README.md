```
/* ************************************************************************** */
/*                                                                            */
/*                                                        :::      ::::::::   */
/*   nestjs                                             :+:      :+:    :+:   */
/*                                                    +:+ +:+         +:+     */
/*   By: hawadh <hawadh@student.42Abudhabi.ae>      +#+  +:+       +#+        */
/*                                                +#+#+#+#+#+   +#+           */
/*   Created: 2022/07/26 12:02:48 by hawadh            #+#    #+#             */
/*   Updated: 2022/07/26 13:28:50 by hawadh           ###   ########.fr       */
/*                                                                            */
/* ************************************************************************** */
```

# `NestJS`

## `Web Development Framework`

Creating a Nest project from scratch (Basics)

```
npm init -y

npm install @nestjs/common@7.6.17 @nestjs/core@7.6.17 @nestjs/platform-express@7.6.17 reflect-metadata@0.1.13 typescript@4.3.2

```

## File Naming Conventions

-   **Main File:**
    -   File: `main.ts`
    -   Function: `bootstrap();`
-   **App Controller:**
    -   File: `app.controller.ts`
    -   Class:  `AppController {}`
-   **App Module:**
    -   File: `app.module.ts`
    -   Class: `AppModule {}`
- ***etc; pattern will always be the same so it can easily be identified***

#   Basics

## `Dependencies:`

1.  @nestjs/common:
    -   Contains Vast majority of functions, classes, etc;

2.  @nestjs/platform-express:
    -   The framework to handle incoming requests (HTTP Server Implementation).
    -   Installs an Adapter between nestjs & express.
3.  reflect-metadata:
    -   Helps make decorators work.

## `Setup`

1.  `tsconfig.json` See file.
2.  Create a Nest module and a controller:
    1.  Creates a Server request response cycle.
    2.  Code to handle request:
        - Process & Tools:
            1.  Validate Data Contained in the request:
                - Pipes.
            2.  Make sure the user is authenticated:
                - Guard.
            3.  Route the request to a particular function:
                - Controller.
            4.  Run some Business Logic:
                - Service.
            5.  Access a Database:
                - Repository.
    3.  Response.

##  `Parts of Nest`

Note:
   - Simplest form of Nest App, bare minimum, consists of at least 1 module and 1 cotnroller.

1.  `Controllers:`
    - Handles incoming requests.
2.  `Services:`
    - Handles data access and business logic.
3.  `Modules:`
    - Groups together code.
4.  `Pipes:`
    - Validates incoming data.
5.  `Filters:`
    - Handles errors that occur during request handling
6.  `Guards:`
    - Handles Authentication.
7.  `Interceptors:`
    - Adds extra logic to incoming requests or outgoing responses.
8.  `Repositories:`
    - Handles data stored in a DB.

## `Building`

Note:   95% of the time we will import from `nestjs/common`, the import `@nestjs/core` is mostly imported in `main.ts`

1.  main.ts:
    -   First file to be executed and starts listening to all traffic.
    -   Usually modules and controllers are in their own files.
2.  Create Class and place Decorator to make controller:
    -   Nest extensively uses decorators.
    -   The class will handle enroute incoming requests.
    -   Add methods that will handle each type of requests.
        -   above method we place decorator to assign method job.
3.  Create Module:
    -   It will wrap up a controller..
    -   For module decorator we must pass configuration option or object.
4.  Create `async functio bootstrap() {}`
    -   Commonly named bootstrap, see `nestjs/src/main.ts` line `#22`
    -   tell app to listen to port (we have selected `1234`), see line `#25` same file as above

## `Controller`

Note: See `/src/app.controller.ts`

-   Adding strings in the Decorator `@Controller("/app")` & `@Get("/asdf")` makes the controller route requests through the strings:
    -   So now instead of `https://localhost:1234` we now have to access the webpage via `https://localhost:1234/app/asdf`
    -   The method `getRootRoute()` now gets accessed via above link
-   Adding a second method, with decorator `@Get("/bye")` now adds a new route to the controller that will route any requests made for `"/bye"` in the webpage (`https://localhost:1234/app/bye`)

# API Clients

1.  postman:
    -   I am using Postman, very easy to setup and use.

2.  VSCode REST Client Extension.
