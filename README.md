# Learn-Postman-with-me.
This is some main notes while my learning of Postman. <br/>
`Note:` If you want to learn, <br/>
Visit the link
<a href="https://www.youtube.com/watch?v=zp5Jh2FIpF0">Course here</a>.

---
## Content of this Repo is the following
![orange](https://placehold.co/15x15/orange/orange.png) Postman definition as a tool.

![orange](https://placehold.co/15x15/orange/orange.png) Postman interface

![orange](https://placehold.co/15x15/orange/orange.png) APIs in test

![orange](https://placehold.co/15x15/orange/orange.png) APIs to test

![orange](https://placehold.co/15x15/orange/orange.png) API requests methods

![orange](https://placehold.co/15x15/orange/orange.png) In the script section (for automation)

![orange](https://placehold.co/15x15/orange/orange.png) Variables

![orange](https://placehold.co/15x15/orange/orange.png) Most of assertions(checks) of an API

![orange](https://placehold.co/15x15/orange/orange.png) Schema validation (Not done)

![orange](https://placehold.co/15x15/orange/orange.png) Run test automation on CI/CD server (Not done)

---
## First: Postman definition as a tool
- Software platform that simplifies the process of working with APIs, making it easier for developers to interact with them.
- Used for building, testing, and documenting APIs.
- It automates requests, validates responses, and compares them against expected results

---
## Postman interface

![interface](interface.jpg)


1. You will find at the top `WorkSpaces` you can (create your own) or (fork from others)  
2. Every Workspace has its own collections + Enviornments 
3. A collection has (folders + requests) + some details like (variables + Auth + ...)
4. A folder that inside a collection has (sub-folders + requests)
5. A request contains all of API details that you need to `send` then watch the response and console of the script and the most importnat thing is  the code status.

---
## APIs in test
in general, We use APIs to fulfill its work accuratly and precisely. ‹br/› Whatever affects its works, Here you need to test the API. ‹br/› Yes, You can do it manually by putting the API and click on the send button.
but believe me, You don't have time for any $${\color{red}continuous \space integration}$$ the developer does. 
 ‹br› 
The solution for this situation is `making Scripts` on Postman and whenever there's integration or change, `You can run these Scripts to make sure everything is Ok` then moving forward to the next level of the API fastly.

---
## APIs to test
Here we will see what do APIs do and what to test.
1. It brings my data from the database server SO:
   - Is the response status or status code is 200 ok?
   - Is this data structured properly as it's expected and required?
   - Does data types of each key goes as expected.
   - Response or objects must obtain id, Does they obtain?
   - What about <b>Headers, Authentication and Authorization</b>?
   - Is the response body by JSON format or HTML?
   - Security of the data? Can all people access that sensitive data?
2. It add my data inside database server, SO:
   - Is status code 201 created?
   - Is the data structured well as expected.
   - Is Data Types as expected?
   - Is that client (who made the request) authenticated or autherized?
   - Are endpoint and data valid?
---

## API requests methods

It's important to know that whoever makes requests to the server, We call him a client made a request. <br/> 
So, In our situation here, Postman is a client to test our server response along with our API.<br/>
That does mean that client send to the server to do(to verb) something (GET data, POST data, DELETE data, PUT data, ...) <br/>

That's why <b>http methods</b> are (GET, POST, DELETE, PUT,...)<br/>
Secondly, you need to know after send to the server, What would the server say to you?<br/>
would it be Ok, not found or what? how to konw that? by <b>status code</b>

---
## In the script section (for automation)
- There are pre-request (before send) and post-request (after send)
- the best practice of pre-request is to set variables before send.
- Postman uses `javascript syntax`.
- There's a third party library called chai.js which contains easy-readable methods like (.to .be  .an .have)
It is used by postman for writing assesrtions(checks)
---
## Variables
- In Postman and programming world, You tends to save time by making variables that has a common value to use it many times.
- for instance => `{{baseUrl}}` a variable we use many times in all of requests of the collection.
- Also, `APIKey` a variable we use many times for auth of the request.
### Types of Variables (the following starts from biggest scope to the lowest) $${\color{yellow}starts \space from \space the \space biggest \space scope \space to \space the \space lowest }$$
 - Global Variables
 - collection variables
 - Environment variables
 - Data variables
 - Local variable
 
 To access to collection variables from script section you will write
  ```javascript
  //post-request section
  let myApiKey = pm.collectionVariables.get("apiKey"); //got a collection variable value
  console.log(myApiKey);
  ```
- Also, In Postman we have random variable that sometimes used in the body of POST request
```javascript
//Inside POST request - Body
{
"clientName":"{{$randomFullName}}", // random variable that every time used will change
"clientId": {{cId}} //collection variable
}
```
```javascript
//Inside POST request - script
//To access random variable but different from last time
console.log(pm.variables.replaceIn(${{randomFullName}})) // output is random name
```
---
## Most of assertions(checks) of an API
```javascript
pm.test("Is status code 200?", ()=>{
  pm.response.to.have.status(200);
  //OR..
  pm.expect(pm.response.code).to.eql(200); //strict equal
});
```
```javascript
let response;
pm.test("Is response json?",()=>{
    pm.response.to.be.json; // this is the test
    response = pm.response.json();
    console.log(response); //open console to see the value
})
```
```javascript
pm.test("Is it valid Product Price ",()=>{
  pm.expect(variable.price).to.be.greaterThan(0.0);
})
```
```javascript
pm.test("Is product available",()=>{
  pm.expect(variable.isAvailable).to.be.true;
  //OR..
  pm.expect(variable.isAvailable).to.eql(true);
})
```
```javascript
let response;
pm.test("are products inside array? ",()=>{
  response = pm.response.json();
  pm.expect(response).to.be.an("object", "response isn't object"); //if err you will find response isn't object
  pm.expect(response.products).to.be.an("array", "Products aren't inside array"); //if err You will find=> Products aren't inside array
  pm.expect(response.products[0]).to.have.property("title");
  pm.expect(response.products[0].price).to.be.a("number", "it's not a number");
})
```
```javascript
pm.test("Is valid Egyptian ID?",()=>{
  pm.expect(citizen.id).to.match(/^2[5-9]\d{12}$/); //Egyption ID regular expression
})
```
```javascript
pm.test("In Headers, x-powered-by is Express",()=>{
  pm.expect(pm.response.headers.get(x-powered-by)).to.eql("Express");
})
```
---
## Schema Validation
section is on progress 👷🏾‍♂️⚠️

---
## Run test automation on CI/CD server
section is on progress 👷🏾‍♂️⚠️
