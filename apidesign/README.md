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
```

## `Design Documentation`

**Note:** Numbered request, and Body/Query String data content

### **`Method and Route:`**

1.  POST `/auth/signup` - Create a new user and sign-in
2.  POST `/auth/signin` - Sign in as an existing user
3.  GET  `/reports` - Get an estimate for the cars value
4.  POST `/reports` - Report how much a vehicle sold for
5.  PATCH `/reports/:id` - Approve or reject a report submitted by a user

### **`Body or Query String:`**

1.  Body - { email, password }
2.  Body - { email, password }
3.  Query String: make, model, year, mileage, longitude, latitude
4.  Body - { make, model, year, mileage, longitude, latitude, price }
5.  Body - { approved }


# `Modules`

### `Two Modules will be required:`

1.  Users Module
2.  Reports Module

### `Both Containing the below:`
<br>

#### `Controllers`

1.  Users Module: POST `/auth/signup` & POST `/auth/signin`:
    - Users Controller

2.  Reports Module: GET `/reports/`, POST `/reports/` & PATCH `/reports/:id`
    - Reports Controller
  
#### `Services`

1.  Users Module: POST `/auth/signup` & POST `/auth/signin`:
    - Users Service

2.  Reports Module: GET `/reports/`, POST `/reports/` & PATCH `/reports/:id`
    - Reports Service

#### `Repositories`

1.  Users Module: POST `/auth/signup` & POST `/auth/signin`:
    - Users Repository

2.  Reports Module: GET `/reports/`, POST `/reports/` & PATCH `/reports/:id`
    - Reports Repository



