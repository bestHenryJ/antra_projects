# KareXpert Advanced HIMS
## Overview
- KareXpert is fully integrated Digital Healthcare Platform, a SaaS-based offering, will enable hospitals to achieve end-to-end automation of their patient journeys and provide the best patient treatment experience.
### Features
- This application helps in management of Patients, doctors, admin in a easy and comfortable way.
- using this Application patients can quickly Sign up, Login, view his/her profile, view doctors, book Appointment, view Report, choose doctor, view Appointments, give feedback, pay online and logout.
- Admin can add Doctors,view patients list, view Doctors list,remove doctors, see feedback given by patients,view reports,logout.Doctor can login, view profile, viewAppointments, Attend Patients and logout.
### Functionalities
- login
- Admin’s DashBoard
- hospital's DashBoard
- Patient’s DashBoard
- Doctor’s DashBoard
- Report-Table
- Appointment-Table
- Feedback Form
- Booking Appointment
- Choosing Doctor
- Payment Process

## database schema (tables)
- user
- hospital
- patient
- doctor
- appointment
- report
- feedback
## rest api design (design 2 - 4 rest apis)
```
    @GetMapping
    public ResponseEntity<List<Doctor>> getDoctors() {
        List<Doctor> doctors = doctorService.findAll();
        return ResponseEntity.ok(doctors);
    }
```
```
    @GetMapping("/{id}")
    public ResponseEntity<?> getPaientById(@PathVariable Long id) {
        String notFoundMessage = getNotFoundMessage(id);
        Patient patient = patientService.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
        return ResponseEntity.ok(patient);
    }
```
```
    @PostMapping
    public ResponseEntity<Patient> addEmployee(@RequestBody Patient patient) {
        Patient newPatient = PatientService.save(patient);
        return new ResponseEntity<>(newPatient, HttpStatus.CREATED);
    }
```
```
    @PutMapping("/{id}")
    public ResponseEntity<Patient> updateEmployeeById(@PathVariable  Long id,
                                                       @RequestBody Patient patient) {
        String notFoundMessage = getNotFoundMessage(id);
        Patient updatePatient = patientService.update(id, patient)
                .orElseThrow(() -> new ResourceNotFoundException(notFoundMessage));
        return ResponseEntity.ok(updatePatient);
    }
```
## Data flow, prepare 2 - 3 data flow diagram (example: when user client some buttons to upload some files, what happens next, how does request go through your services)
![image](https://github.com/bestHenryJ/antra_projects/assets/130790693/736d8a79-0335-4fac-b5eb-732b960d1b00)
## biggest challenge(technical challenge)
- implement dynamic database source swith based on SAAS platform
  - Base on SAAS platform, my strategy is to use independent database to implement hospital resouce management.
  - super admin is required to add new client(hospital) and guarantee database connecting currectly.
  - super admin mannully add database info like url, username, root, schema... for a hospital.
  - implement SAAS upgrade and data isolation.
## Docker

## Prepare stories based on your resume: example,  where did you use multi-threading in your last project? Where did you use builder design patterns in your last project?
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
## frontend story
### Implement
- Angular & bootstrap
### challenge

### story
