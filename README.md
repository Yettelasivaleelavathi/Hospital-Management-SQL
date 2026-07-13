-- Create Database
CREATE DATABASE HospitalManagement;

-- Use Database
USE HospitalManagement;

-- Department Table
CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(50)
);

-- Doctor Table
CREATE TABLE Doctors (
    DoctorID INT PRIMARY KEY,
    DoctorName VARCHAR(100),
    Specialization VARCHAR(100),
    DepartmentID INT,
    Phone VARCHAR(15),
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

-- Patient Table
CREATE TABLE Patients (
    PatientID INT PRIMARY KEY,
    PatientName VARCHAR(100),
    Age INT,
    Gender VARCHAR(10),
    Phone VARCHAR(15),
    Address VARCHAR(150)
);

-- Appointment Table
CREATE TABLE Appointments (
    AppointmentID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    AppointmentDate DATE,
    AppointmentTime TIME,
    Status VARCHAR(20),
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctors(DoctorID)
);

-- Prescription Table
CREATE TABLE Prescriptions (
    PrescriptionID INT PRIMARY KEY,
    AppointmentID INT,
    Medicine VARCHAR(100),
    Dosage VARCHAR(50),
    FOREIGN KEY (AppointmentID) REFERENCES Appointments(AppointmentID)
);

-- Billing Table
CREATE TABLE Billing (
    BillID INT PRIMARY KEY,
    PatientID INT,
    TotalAmount DECIMAL(10,2),
    PaymentStatus VARCHAR(20),
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID)
);

-- Departments
INSERT INTO Departments VALUES
(1,'Cardiology'),
(2,'Neurology'),
(3,'Orthopedics');

-- Doctors
INSERT INTO Doctors VALUES
(101,'Dr. Ravi Kumar','Cardiologist',1,'9876543210'),
(102,'Dr. Priya Sharma','Neurologist',2,'9876543211'),
(103,'Dr. Arun Reddy','Orthopedic',3,'9876543212');

-- Patients
INSERT INTO Patients VALUES
(201,'Rahul',25,'Male','9876500001','Hyderabad'),
(202,'Sneha',30,'Female','9876500002','Bangalore'),
(203,'Amit',40,'Male','9876500003','Chennai');

-- Appointments
INSERT INTO Appointments VALUES
(301,201,101,'2026-07-20','10:00:00','Completed'),
(302,202,102,'2026-07-21','11:30:00','Pending'),
(303,203,103,'2026-07-22','09:15:00','Completed');

-- Prescriptions
INSERT INTO Prescriptions VALUES
(401,301,'Paracetamol','500mg Twice Daily'),
(402,302,'Vitamin D','One Tablet Daily'),
(403,303,'Pain Relief Gel','Apply Twice Daily');

-- Billing
INSERT INTO Billing VALUES
(501,201,1500.00,'Paid'),
(502,202,2500.00,'Pending'),
(503,203,1800.00,'Paid');
-- View all patients
SELECT * FROM Patients;

-- View all doctors
SELECT * FROM Doctors;

-- View appointments
SELECT * FROM Appointments;

-- Join Patient and Doctor Details
SELECT
P.PatientName,
D.DoctorName,
D.Specialization,
A.AppointmentDate,
A.Status
FROM Patients P
JOIN Appointments A ON P.PatientID = A.PatientID
JOIN Doctors D ON A.DoctorID = D.DoctorID;

-- Patients whose bill is pending
SELECT PatientName, TotalAmount
FROM Patients
JOIN Billing
ON Patients.PatientID = Billing.PatientID
WHERE PaymentStatus='Pending';

-- Count patients
SELECT COUNT(*) AS TotalPatients
FROM Patients;

-- Total revenue collected
SELECT SUM(TotalAmount) AS Revenue
FROM Billing
WHERE PaymentStatus='Paid';

-- Doctors in Cardiology
SELECT DoctorName
FROM Doctors
WHERE DepartmentID=1;

-- Update Appointment Status
UPDATE Appointments
SET Status='Completed'
WHERE AppointmentID=302;

-- Delete a Prescription
DELETE FROM Prescriptions
WHERE PrescriptionID=403;
