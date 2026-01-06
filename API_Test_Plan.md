1. Overview

API Name: DummyJSON
Base URL: https://dummyjson.com 
Description: Public mock API for testing and prototyping, providing endpoints for users, posts, products, carts, todos, quotes, and authentication.
Purpose: To validate CRUD operations, authentication, response codes, and data integrity.

3. API Modules & Endpoints
Module	Endpoint	Method	Description
Auth	/auth/login	POST	Login, returns JWT token
Users	/users	GET	Get all users
Users	/users/{id}	GET	Get single user
Users	/users/add	POST	Create new user
Users	/users/{id}	PUT / PATCH	Update user
Users	/users/{id}	DELETE	Delete user
Posts	/posts	GET	Get all posts
Posts	/posts/{id}	GET	Get single post
Posts	/posts/add	POST	Create new post
Posts	/posts/{id}	PUT / PATCH	Update post
Posts	/posts/{id}	DELETE	Delete post

Protected endpoints require Authorization: Bearer {{token}} header.

3.  Headers
Header	Value
Content-Type	application/json
Authorization	Bearer {{token}} (for protected endpoints)

4.  Test Cases
A. Authentication Module (Auth)
TC ID	Scenario	Method	Request Body	Expected Response	Status
AUTH-01	Login valid user	POST	{ "username":"kminchelle", "password":"0lelplR" }	JSON with token, id, username	200
AUTH-02	Login invalid password	POST	{ "username":"kminchelle", "password":"wrongpass" }	{ "message": "Invalid credentials" }	400
AUTH-03	Login missing username	POST	{ "password":"0lelplR" }	{ "message": "Invalid credentials" }	400
AUTH-04	Login missing password	POST	{ "username":"kminchelle" }	{ "message": "Invalid credentials" }	400
B. Users Module
TC ID	Scenario	Method	Request Body	Expected Response	Status
U-01	Get all users	GET	N/A	Array of users	200
U-02	Get single user valid ID	GET	N/A	User object	200
U-03	Get single user invalid ID	GET	N/A	{ "message": "User not found" }	404
U-04	Create user	POST	{ "username":"johndoe","email":"john@example.com","password":"Pass1234" }	User object with id	200
U-05	Update user	PUT	{ "age": 31 }	Updated user object	200
U-06	Delete user	DELETE	N/A	{}	200
U-07	Create user missing field	POST	{ "username":"johndoe" }	{} (mocked)	200
C. Posts Module
TC ID	Scenario	Method	Request Body	Expected Response	Status
P-01	Get all posts	GET	N/A	Array of posts	200
P-02	Get single post valid ID	GET	N/A	Post object	200
P-03	Get single post invalid ID	GET	N/A	{ "message": "Post not found" }	404
P-04	Create post	POST	{ "title":"Test","body":"Some text","userId":1 }	Post object with id	200
P-05	Update post	PATCH	{ "title":"Updated" }	Updated post object	200
P-06	Delete post	DELETE	N/A	{}	200

D. Edge / Negative Cases
Scenario	Endpoint	Expected Response
Access user/post with non-numeric ID	/users/abc or /posts/xyz	400 / 404
Use invalid token	Protected endpoint	401 Unauthorized
No token for protected endpoint	/users/add or /posts/add	401 Unauthorized
Unsupported HTTP method	e.g., GET on /users/add	405 Method Not Allowed
Missing required fields in POST	e.g., {}	Mocked success (note limitation)

5. Test Tools

Postman: Manual & automated testing

Newman: Run Postman collections in CI/CD

GitHub: Store test cases & reports
