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
- generate report
## database schema (tables)
- empolyee (id, firstname, lastname, email, password, department_id, role)
- role (id, user_role)
- warehouse (id, name, address, phone, email)
- sheives (id, category, warehouse_name)
- products (id, name, category, size, weight, price, place_Of_Production, company_of_production, description, warehouse_id, sheives_id)
- department (id, name, category)
- category(id, name)
- transferOrder (id, name, date, description, content, amount)
- transferTpye (id, type, status)
- checkinOrder(id, checkin_time, type, create_time, operator, warehouse_id, status, specifier, products_detail)
- checkoutOrder(id, checkout_time, type, create_time, operator, warehouse_id, status, specifier, products_detail)
- checkinOrderDetail(id, checkin_time, type, create_time, operator, warehouse_id, sheives_id, QA, QA_time, QA_status, finish_time, status, specifier, products_detail)
- checkoutOrderDetail(id, checkout_time, type, create_time, operator, warehouse_id, sheives_id, QA, QA_time, QA_status, finish_time, status, specifier, products_detail)
- supplier(id, company_name, company_type, firstname, lastname, phone, address, priority, detail)
- consumer(id, company_name, company_type, firstname, lastname, phone, address, priority, detail)
- Order(id, checkout_time, type, create_time, operator, warehouse_id, sheives_id, finish_time, status, specifier, products_detail)
- return(id, checkout_time, type, create_time, operator, warehouse_id, sheives_id, finish_time, status, specifier, products_detail)
- invoice(id, from, to, price, start_date, expair_date, balance)
## high level design (microservice architecture) and provide module pictures

![warehouse](https://github.com/bestHenryJ/antra_projects/assets/130790693/fcb21f29-8c9a-416f-b641-91b36f7fb4d0)


## rest api design (design 2 - 4 rest apis)
- ProductCheckinService
```
    	@GetMapping(value = {"/supplier/{id}"})
    	public ResponseEntity<?> getSupplier(@PathVariable long id) {
		SupplierDto supplierDto = ProductCheckinService.findSupplierById(id)
	                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage))
        	return new ResponseEntity<?>(supplierDto, Http.status.ok);
    	}
```
```
    	@PutMapping(value = {"/checkin/{id}"})
    	public ResponseEntity<String> register(@RequestBody NewOrderDto newOrderDto
                                               @PathVariable long id) {
		CheckinOrderDto checkinOrderDto = ProductCheckinService.findOrderById(id)
	                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
		CheckinOrderDto checkinOrderDto = ProductCheckinService.UpdateOrderByNewOrder(id, newOrderDto);
        	return new ResponseEntity<>(checkinOrderDto, HttpStatus.ok);
    	}
```
- ProductCheckoutService
```
    	@PostMapping("/checkout/order")
    	public ResponseEntity<?> createCheckoutOrder(@RequestBody CheckoutOrderDto checkoutOrderDto) {
        	return new ResponseEntity<>(ProductCheckoutService.createOrder(checkoutOrderDto), HttpStatus.CREATED);
    	}
```
```
	@DeleteMapping("/checkout/order/{id}")
    	public ResponseEntity<String> deleteCheckoutOrderById(@PathVariable(name = "id") long id){
        	ProductCheckoutService.deleteOrderById(id);
        	return new ResponseEntity<>("Successful delete", HttpStatus.OK);
    }
```
- InventoryManagementService
```
	@GetMapping(value = "/inverntory/products/{id}")
	public ResponseEntity<?> findProducts(@PathVariable long id) {
	        String notFoundMessage = getNotFoundMessage(id);
	        ProductDto productDto = inventoryService.findProductsById(id, order)
	                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
		return new ResponseEntity<?>(productDto, Http.status.Ok);
	}
```
```
	@GetMapping(value = "/inverntory/employee/{name}")
	public ResponseEntity<EmployeeDto> findEmployee(@PathVariable String name) {
	        String notFoundMessage = getNotFoundMessage(name);
	        EmployeeDto employeeDto = inventoryService.findEmployeeByName(name)
	                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
		return new ResponseEntity<?>(employeeDto, Http.status.Ok);
	}
```
```
	@PutMapping(value = "/inverntory/order")
	public ResponseEntity<String> updateInventoryProducts(@RequestBody OrderDto order) {
	        inventoryService.updateProductsByOrder(order);
	        return new ResponseEntity<>("Successful update", HttpStatus.OK);
	}
```
## Data flow, prepare 2 - 3 data flow diagram (example: when user client some buttons to upload some files, what happens next, how does request go through your services)

### Supplier

![supplier](https://github.com/bestHenryJ/antra_projects/assets/130790693/78958946-6a9f-4545-9ab6-93d46cabd766)


### Consumer

![consumer](https://github.com/bestHenryJ/antra_projects/assets/130790693/45d3e663-43d2-400f-91dc-ab410cf9fe50)


### Employee

![employee](https://github.com/bestHenryJ/antra_projects/assets/130790693/81487315-3f25-4728-957d-22d543cf1203)


## message queue
- AWS SQS: applying for async processing, two service as individual service are not require to complete the whole process in real time and could not block other service and improve efficiency and reliability.
  - create queue between ProductsCheckinService and QualityControlService
    - After ProductsCheckinService generate a order and send a message to queue(topic 1), QualityControlService will fetch a message(order) to check with inventory information and process message from queue according to its processing ability.
    - ProductsCheckinService will response to supplier to commit order successfully
    - After QualityControlService finish checking function then it will send a new order to ProductsCheckinService for supplier to confirm the order detail.
  - create queue between ProductsCheckoutService and QualityControlService
    - After ProductsCheckoutService generate a order and send a message to queue(topic 2), QualityControlService will fetch a message(order) to check with inventory information and process message from queue according to its processing ability.
    - ProductsCheckinService will response to consumer to commit order successfully
    - After QualityControlService finish checking function then it will send a new order to ProductsCheckinService for consumer to confirm the order detail.
  - create queue(topic 3) between ProductsCheckinService and PaymentService
  - create queue(topic 4) between ProductsCheckoutService and PaymentService
  - create queue(topic 5) between QualityControlService and InventoryManagementService
  - create queue(topic 6) between InventoryManagementService and InventoryTransferService
## biggest challenge(technical challenge)
- Optimized data retrieval and storage.
  - For example, quailtyControlService frequently visits products_table to operate data validation. Frequent queries slow down api response. 
  - set up index and change execution plan (change to hash_map storage) to speed up the query.
  - data catche: used LRU strategy to store most frequnt data in catche to reduce the visit rate on products_table.
- debug and maintain microservice:
  - Implement microservice component automate is kind of difficult. For each component we must follow the stages of Build, Deploy and, Monitor. Debugging is difficult to find out each service for an error. It is essential to maintain centralized logging and dashboards to debug problems.
  - For example, implemented unique global transcation ID to tracing the error in microservice component's log.
## Aws
- EC2: configure cpu, memory storage and OS.
- Amazon ECS
- S3: store amount of products' information like photo or video.
- route53: like DNS to parse the url to ip address
- API gateway: routing the message to send to the right service.
- SQS + SNS
- RDS: deploy Oracle database in RDS
## Prepare stories based on your resume: example,  where did you use multi-threading in your last project? Where did you use builder design patterns in your last project?
- multi-threading:
  - eg. when ProductInventoryService need to call OrderValidationService and InventoryManagementService and use their response's data to generate new request, I need to appliy multi-theading.
  - I use Executors to create completableFuture thread pool and asynchronized send restful api to other services, then collecting their response to generate new instance, It could extremely reduce the response time and improve recource usage rate.
- builder design patterns:
  - eg. Response entity has more parameters and parameters are generate dynamicly or response entity will be created by multi steps.
  - the builder pattern can be used to avoid using telescopic constructors. Here I created a response entity class which is called ApiResponse and I have used the builder pattern to create this class.
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
- tools: Amazon ECR
- source reviewer
- configure git hook/git merge 
- build
  - compile source & dependencies
  - run unit tests
- test environment
  - run integration tests
  - Errors, latency, pressure test
- report
  - code coverage report
  - security report
- package dorcker image
- deploy
  - AWS
## monitor in project
- tools: Retrace APM
- application availability
  - make a simple http ping monitor that runs every minute which is a heartbeat detection for every http status.
  - also it allow to check specifying text for the part of response.
- application errors
  - on the error monitoring dashboard, I could check the error rate being too high about page load time and find out the top Errors or Exception log in that period time.
- important web request or key transactions
  - for my microservice application, QAservice send high-volume api request to inventoryManagementService. Through monitor the average response time, error rate, requests per minute to analysis the satisfaction score
- application dependencies (SQL, MongoDB, Redis, etc)
  - Through monitor the external web service like SQL or redis to target the issue for response slowing down or some part is not working well.
- web application metrics
  - received log messages per minute, correlate things like server CPU, request per second and other metrics to monitor the application perfermance.
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
## self intro
- Hi, my name is Henry. Thank you for taking time to interview me. I am an experienced software engineer with around 6 years of professional experience in developing Java based web application. I have solid knowledge on core java and java 8 new features and familiar with all spring framework including spring MVC, springboot, spring data JPA, etc. As a experienced backend java developer, my core job duties is driving complex, large-scale technical projects covering software architecture design, maintainance and optimized database connection and network development. I also have fullstack experience in part of my previous projects which implemented front-end view engine using angular and built cloud service environment using AWS.I have satisfied performance of collaboration in agile scrum team and ability of identfiying and solving problem independently. I have confidence to handle this positon well in the future. Thank you.
## what was your responsibility in project
### As a back-end java developer, My responsibility in project is:
1. identify product features with team manager
2. plan out for new sprint and manage my hour properly.
3. implement back-end server set up including designing restful api, building database connection and developing controller, repository, service in spring framework, etc. Using TDD techique to develop new feature and pass the unit test and integration test.
4. have good communication with front-end engineer and team manager to adjust the api design and features timely.
5. release new features and retrospective.
## project architecture
- Inventory Tracking System is a microservice based single page application.
- 5 layers to design architecture for this project:
- presentation layer: UI components/ UI process using angular to implement.
- Business layer: ProductsChectinService, ProductsCheckoutService, OrderValidationService, InventoryManangmentService, ProductsInverntoryService, InverntoryTransferService, PaymentService, Exception Handling, log monitor, utilities use spring MVC, springboot to lmplement.
- network transport layer: restful API, Http, https.
- Data access layer: data access components, service register, service gateway like API gateway, Eureka, spring data JPA
- Data Layser: database, SQL queries, stored procedures using Oracle
- for each service:
  - ProductsChectinService: get request from front-end supplier input, process supplier order, send reqest to OrderValidationService and response to frontend
  - ProductsCheckoutService:get request from front-end consumer input, process consumer order, send reqest to OrderValidationService and response to frontend.
  - OrderValidationService: valiate the order infomation and generate valid order and return order, response to checkin service and checkout service.
  - InventoryManangmentService: process order, completed product add in and out.
  - ProductsInverntoryService: product inventory analysis and create report.
  - InverntoryTransferService: product transfer between different warehouse.
  - PaymentService: payment process and invoice.
## what's your daily work
- sprint planning meeting: pull features or ticket from backlog
- assign points to developers
- stand up meeting
- check new branch: design, coding, develop, test
- clarify meetings with PM
- meeting with frontend developer or leader
- retrospective meeting when sprint is ending.
## how did you deploy application
- User instances deploy on AWS cloudFront CDN
- object store in Amazon S3
- configue the Kubernetes cluster with docker containers running microservices on Amazon ECS
- API gateway + Route53 to organize restful api
- SQS + SNS to manage message transport
- Data cached by ElastiCache
- configue database on RDS or DynamoDB.
