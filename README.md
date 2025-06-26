-- PATIENT TABLE
CREATE TABLE Patient (
    PatientID INT PRIMARY KEY,
    Name VARCHAR(100),
    Age INT,
    Gender VARCHAR(10),
    Contact VARCHAR(20),
    AdmissionDate DATE
);

-- DOCTOR TABLE
CREATE TABLE Doctor (
    DoctorID INT PRIMARY KEY,
    Name VARCHAR(100),
    Specialty VARCHAR(50),
    Contact VARCHAR(20)
);

-- APPOINTMENT TABLE
CREATE TABLE Appointment (
    AppointmentID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    AppointmentDate DATE,
    Diagnosis TEXT,
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);

-- BILLING TABLE
CREATE TABLE Billing (
    BillID INT PRIMARY KEY,
    PatientID INT,
    TotalAmount DECIMAL(10,2),
    PaidAmount DECIMAL(10,2),
    PaymentDate DATE,
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID)
);

1. Insert Sample Records

INSERT INTO Patient VALUES (1, 'John Doe', 34, 'Male', '9876543210', '2025-06-20');
INSERT INTO Doctor VALUES (101, 'Dr. Smith', 'Cardiologist', '9845123456');
INSERT INTO Appointment VALUES (1001, 1, 101, '2025-06-21', 'High blood pressure');
INSERT INTO Billing VALUES (501, 1, 1500.00, 1500.00, '2025-06-22');

2. Show all patients admitted in June 2025

SELECT * FROM Patient
WHERE MONTH(AdmissionDate) = 6 AND YEAR(AdmissionDate) = 2025;

3. Get all appointments for a specific doctor

SELECT a.AppointmentID, p.Name AS PatientName, a.AppointmentDate, a.Diagnosis
FROM Appointment a
JOIN Patient p ON a.PatientID = p.PatientID
WHERE a.DoctorID = 101;

4. Find total billed amount for each patient

SELECT p.Name, SUM(b.TotalAmount) AS TotalBilled
FROM Billing b
JOIN Patient p ON b.PatientID = p.PatientID
GROUP BY p.Name;

5. List doctors with more than 3 appointments

SELECT d.Name, COUNT(a.AppointmentID) AS NumAppointments
FROM Doctor d
JOIN Appointment a ON d.DoctorID = a.DoctorID
GROUP BY d.Name
HAVING COUNT(a.AppointmentID) > 3;

6. View unpaid bills

SELECT p.Name, b.TotalAmount - b.PaidAmount AS DueAmount
FROM Billing b
JOIN Patient p ON b.PatientID = p.PatientID
WHERE b.TotalAmount > b.PaidAmount;
