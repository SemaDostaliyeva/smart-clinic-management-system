# Smart Clinic Management System - MySQL Database Schema Design

## Overview

The Smart Clinic Management System uses a relational MySQL database to manage doctors, patients, appointments, and prescriptions.

The database is designed to maintain relationships between doctors and patients and to support appointment scheduling and prescription management.

## Database Tables

The system contains the following main tables:

1. Doctor
2. Patient
3. Appointment
4. Prescription

---

## 1. Doctor Table

The Doctor table stores information about doctors registered in the clinic.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| doctor_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique doctor identifier |
| name | VARCHAR(100) | NOT NULL | Doctor's full name |
| email | VARCHAR(150) | NOT NULL, UNIQUE | Doctor's email address |
| phone | VARCHAR(20) | NOT NULL | Doctor's phone number |
| password | VARCHAR(255) | NOT NULL | Encrypted doctor password |
| speciality | VARCHAR(100) | NOT NULL | Medical speciality |
| available_times | TEXT | NULL | Doctor's available appointment times |

### SQL



2. Patient Table
The Patient table stores information about patients registered in the system.
Field	Data Type	Constraints	Description
patient_id	INT	PRIMARY KEY, AUTO_INCREMENT	Unique patient identifier
name	VARCHAR(100)	NOT NULL	Patient's full name
email	VARCHAR(150)	NOT NULL, UNIQUE	Patient's email address
phone	VARCHAR(20)	NOT NULL, UNIQUE	Patient's phone number
password	VARCHAR(255)	NOT NULL	Patient's password
date_of_birth	DATE	NULL	Patient's date of birth
address	VARCHAR(255)	NULL	Patient's address
SQL
CREATE TABLE Patient (
    patient_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    phone VARCHAR(20) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    date_of_birth DATE,
    address VARCHAR(255)
);
3. Appointment Table
The Appointment table stores appointments booked between doctors and patients.
Field	Data Type	Constraints	Description
appointment_id	INT	PRIMARY KEY, AUTO_INCREMENT	Unique appointment identifier
doctor_id	INT	NOT NULL, FOREIGN KEY	References Doctor
patient_id	INT	NOT NULL, FOREIGN KEY	References Patient
appointment_time	DATETIME	NOT NULL	Date and time of appointment
status	VARCHAR(30)	NOT NULL	Appointment status
reason	VARCHAR(255)	NULL	Reason for appointment
SQL
CREATE TABLE Appointment (
    appointment_id INT AUTO_INCREMENT PRIMARY KEY,
    doctor_id INT NOT NULL,
    patient_id INT NOT NULL,
    appointment_time DATETIME NOT NULL,
    status VARCHAR(30) NOT NULL,
    reason VARCHAR(255),

    CONSTRAINT fk_appointment_doctor
        FOREIGN KEY (doctor_id)
        REFERENCES Doctor(doctor_id),

    CONSTRAINT fk_appointment_patient
        FOREIGN KEY (patient_id)
        REFERENCES Patient(patient_id)
);
4. Prescription Table
The Prescription table stores prescriptions created by doctors for patients.
Field	Data Type	Constraints	Description
prescription_id	INT	PRIMARY KEY, AUTO_INCREMENT	Unique prescription identifier
doctor_id	INT	NOT NULL, FOREIGN KEY	References Doctor
patient_id	INT	NOT NULL, FOREIGN KEY	References Patient
medication	VARCHAR(255)	NOT NULL	Prescribed medication
dosage	VARCHAR(100)	NOT NULL	Medication dosage
instructions	TEXT	NULL	Medication usage instructions
prescribed_date	DATE	NOT NULL	Date the prescription was created
SQL
CREATE TABLE Prescription (
    prescription_id INT AUTO_INCREMENT PRIMARY KEY,
    doctor_id INT NOT NULL,
    patient_id INT NOT NULL,
    medication VARCHAR(255) NOT NULL,
    dosage VARCHAR(100) NOT NULL,
    instructions TEXT,
    prescribed_date DATE NOT NULL,

    CONSTRAINT fk_prescription_doctor
        FOREIGN KEY (doctor_id)
        REFERENCES Doctor(doctor_id),

    CONSTRAINT fk_prescription_patient
        FOREIGN KEY (patient_id)
        REFERENCES Patient(patient_id)
);
Database Relationships
The database uses the following relationships:
Doctor and Appointment
One doctor can have many appointments.
Doctor (1) --------< Appointment (Many)
The Appointment.doctor_id foreign key references Doctor.doctor_id.
Patient and Appointment
One patient can have many appointments.
Patient (1) --------< Appointment (Many)
The Appointment.patient_id foreign key references Patient.patient_id.
Doctor and Prescription
One doctor can create many prescriptions.
Doctor (1) --------< Prescription (Many)
The Prescription.doctor_id foreign key references Doctor.doctor_id.
Patient and Prescription
One patient can have many prescriptions.
Patient (1) --------< Prescription (Many)
The Prescription.patient_id foreign key references Patient.patient_id.
Complete Relationship Diagram
                    +----------------+
                    |     Doctor     |
                    +----------------+
                    | doctor_id (PK) |
                    | name           |
                    | email          |
                    | phone          |
                    | password       |
                    | speciality     |
                    | available_times|
                    +-------+--------+
                            |
                    +-------+--------+
                    |                |
                    | 1              | 1
                    |                |
                    | *              | *
            +-------v--------+   +---v-------------+
            |  Appointment   |   |   Prescription  |
            +----------------+   +-----------------+
            | appointment_id |   | prescription_id |
            | doctor_id (FK) |   | doctor_id (FK)   |
            | patient_id (FK)|   | patient_id (FK)  |
            | appointment_time|  | medication       |
            | status         |   | dosage           |
            | reason         |   | instructions     |
            +-------+--------+   | prescribed_date  |
                    |            +--------+----------+
                    |                     |
                    | *                   | *
                    |                     |
                    | 1                   | 1
                    |                     |
            +-------v---------------------v+
            |          Patient             |
            +------------------------------+
            | patient_id (PK)              |
            | name                         |
            | email                        |
            | phone                        |
            | password                     |
            | date_of_birth                |
            | address                      |
            +------------------------------+
Summary
The Smart Clinic Management System database contains four main relational tables:
Doctor - stores doctor information and availability.
Patient - stores patient information.
Appointment - manages appointments between doctors and patients.
Prescription - stores prescriptions created by doctors for patients.
Primary keys uniquely identify records, while foreign keys maintain the relationships between doctors, patients, appointments, and prescriptions.
This schema provides the relational database foundation required for the Smart Clinic Management System.
