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
- user
- role
- product
- category
- order
- inventory
- supply
## high level design (microservice architecture) and provide module pictures
![image](https://github.com/bestHenryJ/antra_projects/assets/130790693/3c442aca-8c86-4e81-ae69-50a24ed9fdd2)
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
		return new ResponseEntity<InvoiceDTO>("Inventory is up to date!", Http.status.Ok);
	}
```
```
	@PutMapping(value = "/inverntory/supply/{id}")
	public ResponseEntity<String> updateInventoryBySupply(@PathVariable long id) {
    		inventoryService.updateInventoryBySupply(id);
		return new ResponseEntity<InvoiceDTO>("Inventory is up to date!", Http.status.Ok);
	}
```
```
	@DeleteMapping(value = "/product/{id}")
	public ResponseEntity<ProductDTO> deleteProduct(@PathVariable long id) {
		return new ResponseEntity<ProductDTO>(inventoryService.deleteProductById(id), Http.status.Ok);
	}
```
## Data flow, prepare 2 - 3 data flow diagram (example: when user client some buttons to upload some files, what happens next, how does request go through your services)
![image](https://github.com/bestHenryJ/antra_projects/assets/130790693/854272a6-a2e9-4940-8315-e048f59aca51)
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
### challenge
### story
