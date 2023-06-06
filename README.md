# Ultra.io : API Challenge

## Project: Developing a Postman Collection for a dummy API and automate its execution using Newman
<br>

### Description: 

The expected outcome is a Postman collection covering the testing of one of the 4 REST API resources available on `https://gorest.co.in/`. <br>
The authentication token for the requests can be generated on `https://gorest.co.in/consumer/login` or on `https://dummy.restapiexample.com/` (as an alternative in case the first one doesn’t work for you)<br>

<br>

You are expected to:

1. Cover as many testing levels and types as possible to make sure your tests are catching all potential critical defects
2. Enable the testing of the provided functionality as part of a CI/CD pipeline (hint: Postman collections can be executed via newman)
3. Describe the chosen API testing approach
4. Provide execution instructions and enough information explaining the final solution
5. An extra bonus is considered developing an automation framework covering the scenarios from the Postman collection --> This was implemented here: https://github.com/arielmachado-uy/ultra-api-challenge

<br>

### **1. Cover as many testing levels and types as possible to make sure your tests are catching all potential critical defects**

<br>

List of devised scenarios

Positive scenarios
1. Create new user
2. Update user's values
3. Delete user

Negative scenarios
1. Get user by ID - not found
2. Update user - not found
3. Delete user - not found
4. Create new user with empty fields
5. Create new user with wrong values
6. Create new user with a repeated email
7. Request with incorrect token
8. Request with incorrect url

These positive and negative scenarios execute all the posible requests in the API
1. POST /public/v2/users
2. GET /public/v2/users/1629
3. PUT /public/v2/users/1629
4. DELETE /public/v2/users/1629
5. GET /public/v2/users

After the execution of every request some validations are performed to the response
1. Validate response status
2. Validate response values
3. Validate response schema
4. Validate the values stored at the environment level

<br>

### **2. Enable the testing of the provided functionality as part of a CI/CD pipeline (hint: Postman collections can be executed via newman)**

<br>

Newman-Postman automation project

Repository link: https://github.com/arielmachado-uy/ultra-postman-challenge

<br>

**Automation files**
- Postman collection: `ultra_collection.json`
- Postman environment file: `ultra_environment.json`

**CI/CD implementation**
- This project runs an entire Postman collection using Newman
- The automation project is integrated with github actions for CI/CD
- The automated test case will run after every push to the `main` branch
- The automated test case can be run manually using the github workflow
 
 <br>

![Image](/documentation/github_job.png)

<br>

### **3. Describe the chosen API testing approach**

<br>

All the scenarios were created in a way that can be executed in series from top to bottom

<br>

**Positive Scenarios**

![Image](/documentation/positive.png)

1. Create new user
    - Uses a `POST` request to create a user
    - Uses a `GET` to retrieve all the users and validate that the last one is the newly created

<br>

2. Update user's values
    - Uses a `GET` to find a user by its ID
    - Uses a `PUT` to update the user's data
    -  Uses a `GET` to find a user by its ID and validate the updated values<br>

<br>

3. Delete user
    - Uses a `DELETE` to remove the user

<br>

**Negative Scenarios**

![Image](/documentation/negative.png)

1. Get user by ID - not found
    - Uses a `GET` to try to find a user with a not existing ID
    - Returns 404

<br>

2. Update user - not found
    - Uses a `PUT` to try to find a user with a not existing ID
    - Returns 404

<br>

3. Delete user - not found
    - Uses a `DELETE` to try to find a user with a not existing ID
    - Returns 404

<br>

4. Create new user - empty fields
    - Uses a `POST` to try to create a user with empty fields
    - Example: "name": ""
    - Returns 422

<br>

5. Create new user - Wrong values
    - Uses a `POST` to try to create a user with wrong values in some fields
    - Example: "gender": "blablabla"
    - Returns 422

<br>

6. Create new user - Repeated email
    - Uses a `POST` request to create a user
    - Uses a `POST` to try to create another user with the email address of the previous user
    - Uses a `DELETE` to remove the user
    - Returns 422

<br>

7. Create new user - Incorrect token
    - Uses a `POST` to try to create a user using an incorrect token
    - Returns 401

<br>

8. Create new user - Incorrect url
    - Uses a `POST` to try to create a user with an incorrect api url
    - Returns 404


<br>

### **4. Provide execution instructions and enough information explaining the final solution**

<br>

**POstman implementation**

Postman setup

To run the collection inside postman, follow these steps:
1. Open postman
2. Import the collection file
3. Import the environment file
4. Click on the 3 dots next to the collection name and select `Run collection`
5. Leave all the requests selected and click on the name that says `Run` and the name of the collection (in my case it says `Run Ultra.io`)
6. After the execution finishes, you will get the results of the execution as in this image

<br>

![Image](/documentation/postman_collection.png)

<br>

**Automation implementation**


Project setup

1. Checkout the code from the provided url (https://github.com/arielmachado-uy/ultra-postman-challenge)
2. Open the project on VS Code
3. Open the terminal and run `yarn` in order to install needed dependencies

<br>

Project execution

- There is a script in the package.json file to run whole collection using newman
- `yarn pm:run`

<br>
Information about the project implementation

<br>

The script will use the attached collection file to run the requests contained in the Postman collection and consuming the variables from the attached postman environment variable

The script will run the collection from top to bottom as if we were running it in Postman and then return the results

![Image](/documentation/newman.png)
