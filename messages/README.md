#   Messages

##  `Application`

1.  Store and retreive message stored in a plain `.json` 3 routes in totals
    1.  To list all messages saved
    2.  To retrieve message by ID
    3.  Create Message

2.  Requests will consist of:
    0. Note:  ***We will only create one controller, one service, one repository to service each request.***
    1.   Request - Get from `https://localhost:1234/messages`
        -   No Validation (No Pipe)
        -   No Authentication (No Guard)
        -   Needs Controller
        -   Needs Service
        -   Needs Repository (`.json` file)
    2.  Request - Post to `https://localhost:1234/messages`
        -   Validates Data (Needs Pipe)
        -   No Authentication (No Guard)
        -   Needs Controller
        -   Needs Service
        -   Needs Repository (`.json` file)
    3.  If User wants to request a message with particular ID from - `https://localhost:1234/messages/id`
        -   Validates Data (Needs Pipe)
        -   No Authentication (No Guard)
        -   Needs Controller
        -   Needs Service
        -   Needs Repository (`.json` file)

3.  Naming:
    -   Controller: MessagesController
    -   Service:    MessagesService
    -   Repository: MessagesRepository

4.  Modules: To wrap up sets of functionality (Groups of different Classes) and above Request requirements
    -   Name:   AppModule

5.  Nestjs CLI:
    1.  Generates a Project with many files and modules and autofills package.json etc;
        -   Generate will also connect everything by default.
        -   Nestjs auto generates `.eslintrc.js` by default that highlights errors around formatting (disable by commenting out entire file)
    2.  Generate a module via nest CLI:
        -   In command line: `nest generate module {insertName}`
            -   This will generate a new directory module file named `insertName.module.ts`
            -   Only write name`insertName`, not `insertNameModule`
    
    3.  Create Messages Controller:
        -   Run Command: `nest generate controller messages/messages --flat`
        -   `--flat`: Tells nest not to create a directory called `messages/controller`
    4.  Add Routes to Controller: *to view them in browser*
        -   Route Options we will use:
            -   List All Messages:
                -   GET/messages
            -   Create a message:
                -   GET/messages
            -   Get a Particular Message:
                -   GET/messages/:id
        -   Format within `MessagesController`:
            -   Route each request to their respective Methods
        ```
            @controller("/messages/")
            export class MessagesController {
                @Get()
                listMessages();
                @Post()
                createMessage();
                @Get()
                getMessage();
            }
        ```
        -   Extract information out of incoming request using decorators:
            -   HTTP Request Consists of: (Example: `https://localhost:1234/messages/5?validate=true`)
                -   Start Line
                    -   POST `/messages/5?validate=true HTTP/1.1`
                -   Headers
                    -   Host: `localhost:1234`
                    -   Content-Type: `application/json`
                -   Body:
                    -   `{"content": "Hi There"}`
            -   Decorators Used to extract the info from URL:
                -   @Param("id")
                    -   `5`
                -   @Query()
                    -   `validate=true`
                -   @Headers()
                    -   `localhost:1234`
                -   @Body()
                    -   `{"content": "Hi There"}`
            -   Method Decorators will determine which method the route request should take.
            -   Argumennts decorators will determine what information will be extracted or parsed by the methods.
            -   Validation  Pipe: (Pipe):
                -   Validate information before it reaches the controller If invalid, we reject.
                -   See `/src/main.ts` line `#7 -> #10`
            -   Validation Rules:
                1.   Tell  Nest use global Validation (See See `/src/main.ts` line `#8`) Only needed once.

                2.  Create a class that describes the different properties that the request body should have:
                    -   Data Transfer Object (DTO)

                3.  Add Validationn Rules:
                    -   Library: `npm install class-validator`
                    -   Additional: `class-transformer`
                    -   Add Decorator `@IsString`

                4.  Apply Class to request Handler:
                    -   `CreateMessageDto` - How does App recognise that this is a Type?
                    -   The Configurations in `tsconfig.ts`:
                        -   Config:  `"emitDecoratorMetadata": true` collects & sends some data from TypeScript to allow the converted javascript code to analyse as per required type annotated in controller.

    5.  Services & Repositories:
        -   Services and Repositories will always be classes.

            1.  Services:
                -   To run Business Logic
                -   To fetch data from Repositories
                -   Frequently uses multiple Repositories to find and store data.

            2.  Repositories:
                -   Storage Related Logic.
                -   Stores Data in Databases, files.
                -   *In our case, MessageRepository will be a file.
                -   Can be a Wrapper around other storage libraries
                (TypeORM, Mongoose schema)

            3.  Usually have similar methods, and method names:
                -   MessagesService:
                    -   `findOne(id: string)`
                    -   `findAll()`
                    -   `create(message: string)`
                -   MessageRepository
                    -   `findOne(id: string)`
                    -   `findAll()`
                    -   `create(message: string)`

            4.  It is up to a service to interact with a repository, and the controller will interact with a service.

            5.  Create Service & Repository: (From Scratch)
                -   Service will depend on code written in repository.
                -   Create Repository **First**
                    -   Create File in root Directory `{insertName.ts}`
                        -   Fill File with format to be stored see `messages.json.sample`.
                -   Create Service: **Second**
                    -   Service creates it's own dependency:
                        -   ***THIS IS BAD PRACTICE***
                    -   See `/src/messages/messages.service.ts`

            6.  Create Instance of Service and give it to controller.

6.  Exceptions:
    -   See line `#33` in `/src/messages/messages.controller.ts`.

7.   Dependency Injection:
    -   Everything in nestjs revolves around dependency injection.
    -   Why:
        -   There is a dependency/heirarchy among our Classes:
            -   **Repository** > **Service** > **Controller**.
            -   We should not allow our Service and Controller to create their own dependencies.

8.  Dependency Injector - Nest DI Container/Injector:
    1.  List of Classes and their Dependencies:
        -   When we create our nest application, a DI container is created for us, when we startup our application we are going to take a look at all the different classeses that we created inside our applications except for our controllers.
        -   We are going to attempt register each of these different classes with our DI container:
            -   Take our messages service and feed it into our DI Container.
            -   The container is then going to analyse our class and look at all the dependencies and setup internal records that ensures that if a service is created, a Repository must be present.
        -   We take a Controller and feed it into Container:
            -   Container is going to look at controller and constructor arguments and controller dependencies
            -   The DI Containner will create a copy of the service, and it's dependency Repo, then create the controller.

    2.  The entire goal of the container is to register all of our different classes and record  all the different classes dependencies, and then use the conatiner to create a new instance of a controller and create all the appropriate instances based on the dependencies.
    
    3.  -> Flow:
        -   At Startup, register all Classes with the container.
        -   Container will figure out what each dependency each class has.
        -   We then ask the container to create an instance of a class for us.
        -   Container creates all required dependencies and gives us the instance
        -   Container will hold nto the created dependency instances and reuse them if needed.

    4.  If we ever need to create another class that has a similar dependency, (eg; controller), it will re-use the instance it already created.

8.  Inversion of Control Principle:
    -   Class should not create instances of it's dependencies on it's own.
        -   **BAD**:
            -   Class creates it's own coopy of MessagesRepsitory.
        -   **Better**:
            -   Class receives it's dependency.
        -   **BEST**:
            -   Class receives it's dependency, and it doesn't specifically require `'Repository'`.
                1.  Uses interface.
                2.  Single Class with property `message: Repository`.
                3.  Constructor to create type `Repository`.

            ```
            //Example Code:

            interface Repository  {
                findOne(id: string);
                findAll();
                create(content: string);
            }

            export class MessagesService {
                messagesRepo: Repository;

                constructor(repo: Repsitory) {
                    this.messagesRepo = repo
                }
            }
            ```

        -   ***Why Good:***
            -   Good for having live environment with Test Cases that can run without writing to hard disk or memory.
            -   Fake Classes/Repository can be made for testing
            -   Tests can be run Faster due to above.
            -   Unify Service Specific/Controller Specific Classes.
        
        -   ***Down sides***
            -   Adds Multiple lines of code (About 3 times more).
                -   Dependency Injection Solves this.
 
9.  Implementing DI Injector:

    1.  First 2 Sections of Flow: Use 'Injectible' Decorator on each class and add them to the modules list of providers. **SEE BELOW**.
        -   Import Injectable from `@nestjs/common`
    2.  **Note: We Do not need to register the controller because the controller is only consuming classes.**
    3.  Next 2 Sections of Flow: Happens Automatically - Nest will try to create cnotroller instances for us.
    4.  In Nestjs -> Providers = Things that can be used as dependencies for other classes.
    5.  See lines `#7 -> #8` providers in `/messages/messages.module.ts`

