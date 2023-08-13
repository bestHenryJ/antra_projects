# Inventory Tracking System
## Overview
- Inventory Tracking System involved computerization of entire warehouse to maintain all operations and transactions pertaining to the inventory online. It would keep track of the entire inventory information which was currently manufactured and stored.
### Features
- reduced costs
- enables automation
- reduced errors
- persistent archiving
- simple use
### Functionalities
- register
- login/logout
- retrieval products
- create orders
- products management(CRUD)
- balance inventory
## database schema (tables)
- user (id, username, email, role)
- role (id, user_role)
- category (id, product_category)
- order (id, user_id, product_id, count)
- product (id, name, description, inventory, category)
- supply (id, user_id, product_id, count)
- invoice (id, name, date, description, content, amount)
## high level design (microservice architecture) and provide module pictures

<img width="958" alt="屏幕快照 2023-08-13 上午8 07 39的副本 2" src="https://github.com/bestHenryJ/antra_projects/assets/130790693/eabc03c6-6113-4fbf-8ba5-2aa010a22c63">

## rest api design (design 2 - 4 rest apis)
```
	@PostMapping(value = "/order")
	public ResponseEntity<OrderDTO> createOrder(@RequestBody OrderDTO orderDTO) {
		return new ResponseEntity<orderDTO>(orderService.createOrder(orderDTO), Http.status.Create);
	}
```
```
	@PutMapping(value = "/inverntory/order/{id}")
	public ResponseEntity<String> updateInventoryByOrder(@PathVariable long id) {
		inventoryService.updateInventoryByOrder(id);
		return new ResponseEntity<String>("Inventory is up to date!", Http.status.Ok);
	}
```
```
	@PutMapping(value = "/inverntory/supply/{id}")
	public ResponseEntity<String> updateInventoryBySupply(@PathVariable long id) {
    		inventoryService.updateInventoryBySupply(id);
		return new ResponseEntity<String>("Inventory is up to date!", Http.status.Ok);
	}
```
```
	@DeleteMapping(value = "/product/{id}")
	public ResponseEntity<ProductDTO> deleteProduct(@PathVariable long id) {
		return new ResponseEntity<ProductDTO>(inventoryService.deleteProductById(id), Http.status.Ok);
	}
```
## Data flow, prepare 2 - 3 data flow diagram (example: when user client some buttons to upload some files, what happens next, how does request go through your services)

<img width="1014" alt="屏幕快照 2023-08-13 上午10 34 57" src="https://github.com/bestHenryJ/antra_projects/assets/130790693/69cc1fc9-c132-4803-a858-8403ae5d9d64">

## message queue
- AWS SQS
  - create queue between OrderService and InventoryService
  - create queue between SupplyService and InventoryService 
## biggest challenge(technical challenge)
- speed up retrieval in database layer: set up index and change excution plan
## Aws
- EC2: configure cpu, memory storage and OS.
- S3: store amount of products' information like photo or video.
- RDS: deploy Oracle database in RDS
## Prepare stories based on your resume: example,  where did you use multi-threading in your last project? Where did you use builder design patterns in your last project?
- multi-threading: I used it when I used restTemple API to call differet microservice and need the response to return to the user. It could extremely reduce the response time and improve recource usage rate.
- builder design patterns: I can use this pattern to build the response entity object when we use ResponseEntity in RestControllers. If the response entity has more parameters, the builder pattern can be used to avoid using telescopic constructors. Here I created a response entity class which is called ApiResponse and I will use the builder pattern to create this class.
## Come up team size(if you are the leader, how many people do you need and their responsibilities)
- size 5 ~ 6
- project manager
- team leader/scrum master
- frontend developer
- backend developer
- de-ops
## Design a ci/cd pipeline flow to (AWS / local) depends on your resume project(if you want to keep AWS)
- tool: Jenkins
- source reviewer
- build
  - compile source & dependencies
  - run unit tests
  - code coverage
- test environment
  - run integration tests
- prod environment
  - Errors, latency, pressure test
- deploy
  - AWS or docker
## monitor in project
- Jira
## daily user / TPS / QPS
- max-connections + accept-count
## frontend story
### Implement
- angular & bootstrap
### story
- Use tokens for authorization: Use JSON Web Tokens (JWT) to implement authorization in this application. When a user logs in, my authentication service should generate a JWT and return it to the client. The client can then include this token in subsequent API requests to authenticate the user and authorize access to protected resources.
- Lazy loading: Lazy loading is a technique used to load only the required code for a specific route, instead of loading the entire application upfront. I apply it to reduce the initial loading time and improve the perceived performance of your application.
- Minimize HTTP requests: Minimizing the number of HTTP requests required to load my application can significantly improve the performance of your application. I achieved this by concatenating and minifying my CSS and JavaScript files, using a CDN, and caching resources.
