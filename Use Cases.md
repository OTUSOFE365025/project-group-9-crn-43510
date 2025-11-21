# Use Case 1 - Exam Schedule and Notifications

**Primary Actor:** Student  
**Goal:** Request and receive accurate answers on scheduled times for exams and tests for the student's currently enrolled courses.  
**Scope:** AIDAP platform, Registration Services, and Calendar systems.  
**Stakeholders/Interests:** Student, Registrar, IT Staff  
**Preconditions:** Student account exists and is actively enrolled; course registration is in order; SSO *(Single Sign-On)* session valid. (RS7–RS8)  
**Trigger:** Student asks the AI any question in the web, on their mobile device or desktop, or by voice activation. (R1, R4, RS9)  

---

# Use Case 2 - Personalized Dashboard Access

**Primary Actor:** Student  
**Goal:** Provide students with a consolidated dashboard of courses, performance metrics, notifications, and events  
**Scope:** AIDAP platform, LMS, Registration, Calendar  
**Stakeholders/Interests:** Students, IT Staff  
**Preconditions:** Student account active and enrolled; prior interactions logged for personalization.  
**Trigger:** Student logs in or requests “Show me my dashboard.” (RS3, RS5)  

---

# Use Case 3 - Course Material Access

**Primary Actor:** Student  
**Goal:** Enable lecturers to upload, update, or retrieve course materials via the assistant.  
**Scope:** AIDAP platform, LMS  
**Stakeholders/Interests:** Lecturers, Students  
**Preconditions:** Lecturer account active; course assigned.  
**Trigger:** Lecturer issues commands like “Upload lecture notes” or “Show materials for Week 3.” (RL1)  

---

# Use Case 4 - Announcements and Notifications

**Primary Actor:** Adminstrator  
**Goal:** Broadcast course-specific or campus-wide announcements to relevant users.  
**Scope:** AIDAP platform, LMS, Email, Calendar  
**Stakeholders/Interests:** Students, Lecturers, Administrators  
**Preconditions:** Announcement content prepared; target audience identified.  
**Trigger:** Authorized user issues broadcast command. (RS2, RL2, RA3)  

---

# Use Case 5 - Academic Analytics and Reports

**Primary Actor:** Lecturer  
**Goal:** Allow lecturers and administrators to view class performance, engagement, and aggregated statistics.  
**Scope:** AIDAP platform, LMS, Registration  
**Stakeholders/Interests:** Lecturers, Administrators  
**Preconditions:** Grades and attendance data available; lecturer/admin authorized.  
**Trigger:** Lecturer/admin requests a report or summary. (RL3, RL6, RA4)  

---

# Use Case 6 - Secure Authentication and Access Control

**Primary Actor:** Any User (Student, Lecturer, Adminstrator, System Maintainer)  
**Goal:** Ensure secure login via SSO and limit access to authorized users only.  
**Scope:** AIDAP platform, Institutional SSO  
**Stakeholders/Interests:** Students, Lecturers, Administrators, IT Staff  
**Preconditions:** User account exists and is valid.  
**Trigger:** User attempts to log in or access sensitive data. (RS7, RS8, RL8, RA5)  

---

# Use Case 7 - System Maintenance and Monitoring

**Primary Actor:** System Maintainer  
**Goal:** Enable system maintainers to deploy updates, monitor health, and manage AI model versions.  
**Scope:** AIDAP platform, Cloud deployment, AI services  
**Stakeholders/Interests:** System Maintainers, IT Staff  
**Preconditions:** Maintenance account exists; CI/CD pipelines configured.  
**Trigger:** Maintainer initiates update, monitors dashboards, or configures AI models. (RM1–RM3)  

---

# Use Case 8 - Data Synchronization with External Systems

**Primary Actor:** Data Source System (with external source)  
**Goal:** Keep LMS, registration, calendar, and email data consistent and up-to-date.  
**Scope:** AIDAP platform, External Data Sources  
**Stakeholders/Interests:** Administrators, IT Staff  
**Preconditions:** Data source accounts exist; APIs configured.  
**Trigger:** Scheduled sync or manual data refresh. (RD1–RD4)  


<img width="393" height="884" alt="image" src="https://github.com/user-attachments/assets/8ed090b6-7abe-43c3-82ad-2aee5302c24e" />


---

# Use Case 9 - Export Course calendar to personal calendar

**Primary Actor:** Student  
**Goal:** Allows access to ICS feed (iCalendar sharing service) for students to use for current enrolled courses.
**Scope:** AIDAP platform, LMS, Registration, Calendar  
**Stakeholders/Interests:** Students  
**Preconditions:** Preferences set; export enabled. (RS6)  
**Trigger:** Student selects option to 'export to my calendar'.

---

# Use Case 10 - User Data Export or delete for Privacy

**Primary Actor:** Student  
**Goal:** Export conversation and data (read/write) history for neccesary needs; deletion.
**Scope:** AIDAP platform, System, Student, External Service
**Stakeholders/Interests:** Students  
**Preconditions:** User identity verified, process allowed (R8, RA2)
**Trigger:** Student submit request to do so in account settings.

---
