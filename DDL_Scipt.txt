create schema G1_15;
set search_path to G1_15;

-- 1
create table patient(
    PID varchar(10) Primary Key,
    Fname varchar(20),
    Mname varchar(20),
    Lname varchar(20),
    Gender char,
    Dob date,
    Area Text,
    City varchar(20),
    Country varchar(20)
);

-- 2
create table Test_Details(
    Test_code varchar(10) Primary Key,
    Test_charge decimal(10, 2),
    Test_type varchar(30),
    Category varchar(20)
);

-- 3
create table Lab_report(
    Lab_Report_id varchar(10) Primary Key,
    PID varchar(10) references patient(PID),
    Report_date timestamp DEFAULT CURRENT_TIMESTAMP(0),
    Weight decimal(5,2),
    height decimal(5,2),
    Temperature decimal(5,2),
    Blood_Pressure varchar(20),
    Test_result Text
);

-- 4
create table Rooms(
    Room_no varchar(5),
    Bed_no int,
    Room_type varchar(20),
    Room_charge numeric(10,2) DEFAULT 1000, -- Per DAY
    Primary key(Room_no, Bed_no)
);

-- 5
create table Room_Allotment(
    Index varchar(10) Primary Key,
    Room_no varchar(5),
    Bed_no int,
    PID varchar(10),
    charge numeric(10,2) DEFAULT 1000, -- INITIALISED AS ROOMS(ROOM_CHARGE)
    Admit_date timestamp Default CURRENT_TIMESTAMP(0),
    Discharge_date timestamp DEFAULT NULL,
    Foreign key(PID) references patient(PID),
    Foreign key(Room_no,Bed_no) references Rooms(Room_no,Bed_no) on update cascade
);

-- 6
create table Patient_Phone(
    PID varchar(10) references patient(PID),
    phone_no numeric(10, 0),
    phone_type varchar(10) DEFAULT 'Default',
    Primary key(PID, phone_no)
);

-- 7
create table Patient_Email(
    PID varchar(10) references patient(PID),
    email varchar(30),
    email_type varchar(10) DEFAULT 'Default',
    Primary key(PID, email)
);

-- 8
create table Doctor(
    DocID varchar(10) Primary Key,
    Firstname varchar(20),
    Lastname varchar(20),
    Speciality varchar(30)
);

-- 9
create table Appointment(
    Apid varchar(10) Primary Key,
    Apmnt_time time,
    Apmnt_date date,
    DocID varchar(10) references Doctor(DocID),
    PID varchar(10) references patient(PID),
    Apmnt_description Text,
    Apmnt_created timestamp DEFAULT CURRENT_TIMESTAMP(0)
);

-- 10
create table Patient_Report(
    Report_id varchar(10) Primary Key,
    Report_date date DEFAULT DATE(CURRENT_TIMESTAMP(0)),
    DocID varchar(10) references Doctor(DocID),
    PID varchar(10) references patient(PID),
    Diagnosis Text
);

-- 11
Create table Medicine(
    Med_ID varchar(10) Primary key,
    Med_name varchar(30),
    Med_description Text,
    Med_cost numeric(10,2)
);

-- 12
create table Prescription_Details(
    Report_id varchar(10) references Patient_Report(Report_id),
    Med_ID varchar(10) references Medicine(Med_ID),
    Primary key(Report_id, Med_ID)
);

-- 13
Create table supplier (
    Supplier_ID varchar(10) Primary Key,
    Supplier_company varchar(30), 
    Area Text, 
    City varchar(20), 
    Country varchar(20)
);

-- 14
Create table Medicine_Details (
    Med_ID varchar(10) References medicine(Med_ID),
    Production_date date ,
    Expiry_date date,
    Quantity int,
    Company varchar(40),
    Origin_Country varchar(20),
    supplier_id varchar(10) references supplier(supplier_id),
    Primary key(Med_ID,Production_date)
);

-- 15
Create table supplier_email (
    Supplier_ID varchar(10) References supplier(supplier_id) on update cascade on delete cascade,
    Email varchar(30),
    Email_type varchar(10) DEFAULT 'Default',
    Primary key(Email,supplier_id)
);

-- 16
Create table supplier_contact (
    Supplier_ID varchar(10) References supplier(supplier_id) on update cascade on delete cascade,
    phone_no numeric(10,0),
    phone_type varchar(10) DEFAULT 'Default',
    Primary key(phone_no,supplier_id)
);

-- 17
create table Insurance(
    Insurance_no varchar(10) Primary Key,
    policy_no varchar(20),
    publish_date date,
    Insurance_company varchar(30),
    in_expiry_date date,
    med_coverage numeric(10, 2),
    insurance_plan Text
);

-- 18
create table Bill(
    Bill_no varchar(10) Primary key,
    Bill_date date Default DATE(CURRENT_TIMESTAMP(0)),
    PID varchar(10) references patient(PID),
    Doctor_charge decimal(10, 2),
    Lab_charge decimal(10, 2),
    Medicine_charge decimal(10, 2),
    Room_charge decimal(10, 2),
    Operation_charge decimal(10, 2),
    No_of_days int,
    Insurance_no varchar(20) references Insurance(Insurance_no) DEFAULT 'INS000'
);

-- 19
Create table Department(
    dept_id varchar(10) Primary key,
    dept_name varchar(30)
);

-- 20
Create table Employee_details(
    ID varchar(10) Primary key,
    fname varchar(20),
    minit char,
    lname varchar(20),
    dob date,
    gender char,
    dept_id varchar(10) References Department(dept_id),
    is_dept_id_mngr bool,
    area Text,
    city varchar(20),
    country varchar(20),
    Unique(ID,dept_id,is_dept_id_mngr)
);

-- 21
CREATE TABLE PAYROLL (
    ID  varchar(20) Primary key References Employee_Details(ID),
    account_no varchar(20),
    IBAN varchar(40),
    net_salary numeric(10,2),
    hourly_salary numeric(10,2),
    compensation numeric(10,2),
    salary numeric(10,2)
);

-- 22
CREATE TABLE EMPLOYEE_PHONE(
    ID varchar(10) References Employee_Details(ID),
    phone_no numeric(10,0),
    phone_type varchar(10) DEFAULT 'Default',
    Primary key(ID,phone_no)
);

-- 23
CREATE TABLE EMPLOYEE_EMAIL(
    ID varchar(10) References Employee_Details(ID),
    email varchar(30),
    email_type varchar(10) DEFAULT 'Default',
    Primary key(ID,email)
);

-- 24
CREATE TABLE TESTS_LIST(
    Lab_Report_id VARCHAR(10) References LAB_REPORT(Lab_Report_id),
    Test_code VARCHAR(10) References TEST_DETAILS(Test_code),
    Primary Key(Lab_Report_id,Test_code)
);

-- ALTER TABLE MEDICINE_DETAILS ALTER COLUMN COMPANY TYPE VARCHAR(40);
-- ALTER TABLE DOCTOR RENAME COLUMN LNAME TO LASTNAME;