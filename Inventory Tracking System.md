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
- user (id, username, compay_name, email, password, role)
- role (id, user_role)
- category (id, product_category)
- order (id, user_id, details)
- product (id, name, description, inventory, category)
- supply (id, user_id, details)
- invoice (id, name, date, description, content, amount)
## high level design (microservice architecture) and provide module pictures

![warehouse](https://github.com/bestHenryJ/antra_projects/assets/130790693/58dc4382-a027-4c7a-a0bc-be772fcad96f)


## rest api design (design 2 - 4 rest apis)
- orderService
```
	@GetMapping("/order/{id}")
    	public ResponseEntity<?> getOrderById(@PathVariable Long id) {
        	String notFoundMessage = getNotFoundMessage(id);
        	orderDTO order = orderService.findById(id)
                	.orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
        	return ResponseEntity.ok(order);
    	}
```
```
	@PostMapping(value = "/order")
	public ResponseEntity<OrderDTO> createOrder(@RequestBody OrderDTO orderDTO) {
		return new ResponseEntity<orderDTO>(orderService.createOrder(orderDTO), Http.status.Create);
	}
```
- supplyService
```
    	@GetMapping("/supply/{id}")
    	public ResponseEntity<?> getSupplyById(@PathVariable Long id) {
        	String notFoundMessage = getNotFoundMessage(id);
        	supplyDTO supply = suppleService.findById(id)
                	.orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
        	return ResponseEntity.ok(supply);
    	}
```
```
	@DeleteMapping(value = "/supply/{id}")
	public ResponseEntity<SupplyDTO> createSupply(@RequestBody SupplyDTO supplyDTO) {
		return new ResponseEntity<SupplyDTO>(orderService.createSupply(supplyDTO), Http.status.Create);
	}
```
- inventoryService
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
```
	@PutMapping(value = "/inverntory/order/{id}")
	public ResponseEntity<Patient> updateInventoryByOrder(@PathVariable  Long id,
                                                          @RequestBody orderDto order) {
	        String notFoundMessage = getNotFoundMessage(id);
	        Patient updateInventory = inventoryService.update(id, order)
	                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
	        return ResponseEntity.ok(updateInventory);
	}
```
## Data flow, prepare 2 - 3 data flow diagram (example: when user client some buttons to upload some files, what happens next, how does request go through your services)

<img width="1014" alt="屏幕快照 2023-08-13 上午10 34 57" src="https://github.com/bestHenryJ/antra_projects/assets/130790693/69cc1fc9-c132-4803-a858-8403ae5d9d64">

## message queue
- AWS SQS: applying for async processing, two service as individual service are not require to complete the whole process in real time and could not block other service and improve efficiency and reliability.
  - create queue between OrderService and InventoryService
    - After orderService generate a order and send a message to queue(topic 1), InventoryService will fetch a message and process message from queue according to its processing ability.
  - create queue between SupplyService and InventoryService
    - After orderService generate a supply and send a message to queue(topic 2), InventoryService will fetch a message and process message from queue according to its processing ability.
  - create queue between InventroyService and PaymentService
## biggest challenge(technical challenge)
- speed up retrieval in database layer:
  - set up index and change execution plan
- debug and maintain microservice:
  - Implement microservice component automate is kind of difficult. For each component we must follow the stages of Build, Deploy and, Monitor. Debugging is difficult to find out each service for an error. It is essential to maintain centralized logging and dashboards to debug problems.
## Aws
- EC2: configure cpu, memory storage and OS.
- S3: store amount of products' information like photo or video.
- RDS: deploy Oracle database in RDS
## Prepare stories based on your resume: example,  where did you use multi-threading in your last project? Where did you use builder design patterns in your last project?
- multi-threading:
  - I used it when I used restTemple API to call differet microservice and need the response to return to the user. It could extremely reduce the response time and improve recource usage rate.
- builder design patterns:
  - I can use this pattern to build the response entity object when we use ResponseEntity in RestControllers. If the response entity has more parameters, the builder pattern can be used to avoid using telescopic constructors. Here I created a response entity class which is called ApiResponse and I will use the builder pattern to create this class.
- Implement JWT tokens:
  - use JWT tokens to implement user verification. Realize one time login and multi-times operation to improve user experience on response time and user security.
## Come up team size(if you are the leader, how many people do you need and their responsibilities)
- size 5 ~ 6
- project manager (monitor whole project progress)
- team leader/scrum master (quality control and techique support)
- frontend developer (UI/UX, frontend view engine developing, html, css, js)
- backend developer (database, spring mvc，ORM(Hibernate))
- de-ops (CI/CD)
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
-  tools: Jira
-  create a plan or outline
-  set goals and expectations
-  use Jira to control timeline, cost and tickets to monitor project progress
-  generate and evaluate reports
-  make necessary improvements
## daily user / TPS / QPS
- max-connections + accept-count
- tools: Jmeter
- create a test plan
- set up thread group/ total request amount
- add http request
- generate aggregate report
- report:
  - simple-------------10000(5000 thread and 2 request per thread)
  - average------------203
  - median-------------24  
  - 90% line-----------671
  - 95% line-----------1018
  - 90% line-----------1091
  - min----------------0
  - maxinum-----------1057
  - error%-------------0.0%
  - throughput(QPS)---3423.5/sec
  - received KB/s------561.67 kb/s
  - send KB/s---------702.8 kb/s
## frontend story
### Implement
- angular & bootstrap
### story
- Use tokens for authorization: Use JSON Web Tokens (JWT) to implement authorization in this application. When a user logs in, my authentication service should generate a JWT and return it to the client. The client can then include this token in subsequent API requests to authenticate the user and authorize access to protected resources.
- Lazy loading: Lazy loading is a technique used to load only the required code for a specific route, instead of loading the entire application upfront. I apply it to reduce the initial loading time and improve the perceived performance of your application.
- Minimize HTTP requests: Minimizing the number of HTTP requests required to load my application can significantly improve the performance of your application. I achieved this by concatenating and minifying my CSS and JavaScript files, using a CDN, and caching resources.
