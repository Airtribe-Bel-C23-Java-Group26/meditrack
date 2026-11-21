# MediTrack — Clinic & Appointment Management System

MediTrack is a console-based Clinic & Appointment Management System built using Core Java.
It will allow admins to manage:

- Patients
- Doctors
- Appointments

MediTrack is a Java console application built with [Apache Maven](https://maven.apache.org/).

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Java Development Kit (JDK)**: Version 17 or higher
- **Apache Maven**: Version 3.9.0 or higher [Setup Guide](https://maven.apache.org/install.html) OR `choco install maven`

## Steps to run the app:

Clone the app locally and run following commands in the terminal,

```bash
mvn clean package -DskipTests

java -cp target/meditrack-1.0-SNAPSHOT.jar com.airtribe.meditrack.App
```

Class Diagram:

```mermaid

classDiagram
    class App {
        +main(String[] args)
        +displayMenuOptions()
    }

    class MenuOptions {
        <<utility>>
        +ADD_NEW_PATIENT: String
        +VIEW_ALL_PATIENTS: String
        +SEARCH_PATIENT_BY_ID: String
        +SEARCH_PATIENT_BY_NAME: String
        +UPDATE_PATIENT: String
        +DELETE_PATIENT: String
        +ADD_NEW_DOCTOR: String
        +VIEW_ALL_DOCTORS: String
        +UPDATE_DOCTOR: String
        +DELETE_DOCTOR: String
        +CREATE_APPOINTMENT: String
        +VIEW_APPOINTMENTS_FOR_PATIENT: String
        +VIEW_APPOINTMENTS_FOR_DOCTOR: String
        +UPDATE_APPOINTMENT_STATUS: String
        +EXIT: String
        +ALL_OPTIONS: Map<Integer, String>
    }

    class PatientOperations {
        -patientService: PatientService
	    - scanner: Scanner
        +handleAddPatient()
        +handleViewAllPatients() ArrayList~Patient~
        +handleSearchPatientById() Patient
        +handleSearchPatientByName() Patient
        +handleUpdatePatient() Patient
        +handleDeletePatient() boolean
        -displayPatient(Patient patient)
    }

    class PatientService {
        -patientRepository: PatientRepository
        +PatientService()
        +addPatient(String firstName, String lastName, String emailAddress) Patient
        +getAllPatients() ArrayList~Patient~
        +searchPatient(int) Patient
        +searchPatient(String emailAddress)
        -validateData(String firstName, String lastName, String email)
    }


    class PatientRepository {
        -patients: ArrayList~Patient~$
        +getAllPatients() ArrayList~Patient~
        +addPatient(Patient)
        +searchPatientById(int) Optional~Patient~
        +searchPatientByEmail(int) Optional~Patient~
        +updatePatient(int, Patient) Patient
    }

    class DoctorService {
        -doctorRepository: DoctorRepository
        +DoctorService()
        +addDoctor(String firstName, String lastName, String emailAddress, Specialization specialization) Doctor
        +getAllDoctor() ArrayList~Doctor~
        +searchDoctor(int) Doctor
        +searchDoctor(String emailAddress)
        -validateData(String firstName, String lastName, String email)
    }


    class DoctorRepository {
        -doctors: ArrayList~Doctor~$
        +getAllDoctors() ArrayList~Doctor~
        +addDoctor(Doctor)
        +searchDoctorById(int) Optional~Doctor~
        +searchDoctorByEmail(int) Optional~Doctor~
        +updateDoctor(int, Doctor) Doctor
    }

    class Person {
        -id: int
        -firstName: String
        -lastName: String
        -email: String
        +Person(int id, String firstName, String lastName, String email)
        +getId() int
        +setId(int)
        +getFirstName() String
        +setFirstName(String)
        +getLastName() String
        +setLastName(String)
        +getEmail() String
        +setEmail(String)
        +getDisplayName() String
    }

    class Patient {
        +Patient(int id, String firstName, String lastName, String email)
        +getDisplayName() String
    }

    class Doctor {
        -specialization: Specialization
        -consultationFee: double
        +Doctor(int id, String firstName, String lastName, String email, String specialization)
        +getSpecialization() String
        +getConsultationFee() double
        +setConsultationFee(double)
        +getDisplayName() String
    }

    class Appointment {
        -id: int
        -patientId: int
        -doctorId: int
        -appointmentStartTime: LocalDateTime
        -appointmentEndTime: LocalDateTime
        -status: AppointmentStatus
        +Appointment(int id, int patientId, int doctorId, AppointmentStatus appointmentStatus)
        +getId() int
        +setId(int)
        +getPatientId() int
        +setPatientId(int)
        +getDoctorId() int
        +setDoctorId(int)
        +getStatus() AppointmentStatus
        +setStatus(AppointmentStatus)
    }

    class AppointmentStatus {
        <<enumeration>>
        PENDING
        CONFIRMED
        CANCELLED
        COMPLETED
    }

    class Bill {
        -id: int
    }

    class BillSummary {
        -id: int
    }

    class Specialization {
        <<enumeration>>
        GENERAL_PHYSICIAN
        CARDIOLOGY
        DERMATOLOGY
        PEDIATRICS
        ORTHOPEDICS
        GYNECOLOGY
        ENT
        -String displayName
        +getDisplayName() String
    }


    class Validator {
        <<utility>>
        +IsNotNullOrEmpty(String)
        +IsValidEmail(String)
        +IsGreaterThanZero(int)
    }

    class IdGenerator {
        <<utility>>
        +getNextPersonId() int
        +getNextDoctorId() int
        +getNextPatientId() int
        +getNextAppointmentId() int
    }


    class InvalidDataException
    class EntityNotFoundException

    App --> MenuOptions
    App --> PatientOperations
    PatientOperations --> PatientService

    PatientService --> PatientRepository
    PatientService --> Patient
    PatientService --> Validator
    PatientService --> IdGenerator
    PatientService --> InvalidDataException

    PatientRepository --> Patient

    DoctorService --> DoctorRepository
    DoctorService --> Doctor
    DoctorService --> Validator
    DoctorService --> IdGenerator
    DoctorService --> InvalidDataException

    DoctorRepository --> Doctor


    Person <|-- Patient : "extends"
    Person <|-- Doctor : "extends"



    Validator --> InvalidDataException
    PatientRepository --> EntityNotFoundException
```
