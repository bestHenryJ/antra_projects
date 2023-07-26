# Datafaction
## Overview
- Datafaction provides scale, control and security to family offices, business managers and sport management firms that offer bill pay and accounting services. The specialized multi-entity accounting and secure bill payment solution incorporates approval workflow, integration with City National Bank, document management and investment data integration.
### Features
- General ledger
- Accounts payable
- Accounts receivable
- Data integration
- Bill pay
- Multi-client entity management
- Investment integration
### Functionalities
- investment management
- revenue management
- Bill management
- report
- predication
## database schema (tables)
- investment
- income
- expense
- statement
- company
- user
- company_type
## rest api design (design 2 - 4 rest apis)
```
    @GetMapping(/report/{id}/{start}/{end})
    public ResponseEntity<List<Report>> getReports(@PathVariable Long id, @PathVariable Date start, @PathVariable Date end) {
        List<Report> reports = doctorService.findByRange(id, start, end);
        return ResponseEntity.ok(reports);
    }
```
```
    @PostMapping("/revenue")
    public ResponseEntity<Revenue> createRevenue(@RequestBody RevenueDTO revenueDTO) {
        RevenueDTO newRevenue = revenueService.save(revenueDTO);
        return new ResponseEntity<>(newRevenue, HttpStatus.CREATED);
    }
```
```
    @PostMapping("/expense")
    public ResponseEntity<Expense> createExpense(@RequestBody ExpenseDTO expenseDTO) {
        ExpenseDTO newExpense = ExpenseService.save(expenseDTO);
        return new ResponseEntity<>(newExpense, HttpStatus.CREATED);
    }
```
```
    @PutMapping("/{id}")
    public ResponseEntity<InvestmentDTO> updateInvestmentById(@PathVariable  Long id,
                                                       @RequestBody InvestmentDTO inv) {
        String notFoundMessage = getNotFoundMessage(id);
        InvestmentDTO updateInvestment = investmentService.update(id, inv)
                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
        return ResponseEntity.ok(updateInvestment);
    }
```
## Data flow, prepare 2 - 3 data flow diagram (example: when user client some buttons to upload some files, what happens next, how does request go through your services)
![image](https://github.com/bestHenryJ/antra_projects/assets/130790693/7496c3a3-92d2-4b16-94db-d1f25ce244e2)

## biggest challenge(technical challenge)
- 
## Prepare stories based on your resume: example,  where did you use multi-threading in your last project? Where did you use builder design patterns in your last project?
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
## frontend story
### Implement
### challenge
### story
