# Chrome River INVOICE
## Overview
- Chrome River INVOICE help buyer and seller to generate a a mutually understandable and govern standard format invoice. Its automatic process and modern UI could extremely simplified manual operations. When electronic invoicing systems are set up properly, they can help companies to cut costs, reduce turnover, and make better use of data.
### Features
- simple archiving
- Faster and More Convenient Payments
- reduced costs
- enables automation
- reduced turnover/errors
### Functionalities
- register
- login/logout
- create orders
- generate invoices
- store invoices and orders info
- retrieval invoices and produce report
- match and approve invoices
## database schema (tables)
- users
- roles
- invoices
- orders
- invoices_category
## high level design (microservice architecture) and provide module pictures
![image](https://github.com/bestHenryJ/antra_projects/assets/130790693/7be3f88b-b933-4ca5-8512-461482211ee5)
## rest api design (design 2 - 4 rest apis)
```
	@PutMapping(value = "/invoice/update")
	public ResponseEntity<InvoiceDTO> updateInvoice(@RequestBody InvoiceDTO in) {
		return new ResponseEntity<InvoiceDTO>(invoiceService.updateInvoice(in), Http.status.Ok);
	}
```
```
	@GetMapping(value = "/invoices")
	public ResponseEntity<List<InvoiceDTO>> getInvoice() {

		return new ResponseEntity<InvoiceDTO>(invoiceService.getInvoices(), Http.status.Ok);
	}
```
```
	@GetMapping(value = "/order/{id}")
	public ResponseEntity<InvoiceDTO> getInvoiceById(@PathVariable long id) {

		return new ResponseEntity<InvoiceDTO>(invoiceService.getInvoiceById(id), Http.status.Ok);
	}
```
```
	@PostMapping(value = "/order")
	public ResponseEntity<InvoiceDTO> createInvoice(@RequestBody InvoiceDTO invoiceDTO) {

		return new ResponseEntity<InvoiceDTO>(invoiceService.createInvoice(invoiceDTO), Http.status.Create);
	}
```
## Data flow, prepare 2 - 3 data flow diagram (example: when user client some buttons to upload some files, what happens next, how does request go through your services)
![image](https://github.com/bestHenryJ/antra_projects/assets/130790693/98cbcbd9-691f-4793-bbc9-497f467cec3f)
## message queue
- AWS SQS
## biggest challenge(technical challenge)
## Aws
- S3
- RDS
## Prepare stories based on your resume: example,  where did you use multi-threading in your last project? Where did you use builder design patterns in your last project?
## Come up team size(if you are the leader, how many people do you need and their responsibilities)
## Design a ci/cd pipeline flow to (AWS / local) depends on your resume project(if you want to keep AWS)
## monitor in project
- Jira
## daily user / TPS / QPS
## frontend story
### Implement
### challenge
### story
