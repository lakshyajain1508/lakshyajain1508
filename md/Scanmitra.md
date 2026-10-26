# ScanMitra — Smart Attendance Management System

> **Ek Scan, Attendance Done.**

ScanMitra is a mobile-first, role-based smart attendance management system designed for modern college classrooms. It combines **college ID barcode scanning, manual attendance, real-time attendance, offline-first attendance, automatic synchronization, timetable management, notifications, analytics, and role-specific dashboards** into one platform.

The system is designed around a simple classroom workflow:

**Teacher starts lecture → Student ID is scanned → Attendance is validated → Attendance is recorded → Teacher dashboard updates → Student receives notification → If offline, data is stored locally and synchronized automatically when the network returns.**

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Problem Statement](#problem-statement)
3. [Objectives](#objectives)
4. [Core Idea](#core-idea)
5. [Key Features](#key-features)
6. [User Roles](#user-roles)
7. [System Architecture](#system-architecture)
8. [Technology Stack](#technology-stack)
9. [Project Structure](#project-structure)
10. [Application Flow](#application-flow)
11. [Teacher Workflow](#teacher-workflow)
12. [Student Workflow](#student-workflow)
13. [Admin Workflow](#admin-workflow)
14. [Barcode Attendance](#barcode-attendance)
15. [Manual Attendance](#manual-attendance)
16. [Offline-First Attendance](#offline-first-attendance)
17. [Automatic Synchronization](#automatic-synchronization)
18. [Student Notifications](#student-notifications)
19. [Timetable Management](#timetable-management)
20. [CSV Timetable Import](#csv-timetable-import)
21. [Timetable Photo / OCR Architecture](#timetable-photo--ocr-architecture)
22. [Lecture Detection](#lecture-detection)
23. [Testing / Development Mode](#testing--development-mode)
24. [Authentication](#authentication)
25. [Role-Based Authorization](#role-based-authorization)
26. [Admin Portal](#admin-portal)
27. [Teacher Portal](#teacher-portal)
28. [Student Portal](#student-portal)
29. [Notification System](#notification-system)
30. [Attendance Status Model](#attendance-status-model)
31. [Attendance Data Model](#attendance-data-model)
32. [Firestore Collections](#firestore-collections)
33. [Offline IndexedDB Model](#offline-indexeddb-model)
34. [Security Design](#security-design)
35. [Duplicate Attendance Protection](#duplicate-attendance-protection)
36. [Network Failure Handling](#network-failure-handling)
37. [Scanner Compatibility](#scanner-compatibility)
38. [Responsive Design](#responsive-design)
39. [UI / UX Design System](#ui--ux-design-system)
40. [Brand Identity](#brand-identity)
41. [Pages and Routes](#pages-and-routes)
42. [Environment Configuration](#environment-configuration)
43. [Installation](#installation)
44. [Running the Project](#running-the-project)
45. [Development Commands](#development-commands)
46. [Deployment](#deployment)
47. [Mobile Testing](#mobile-testing)
48. [Functional Testing Checklist](#functional-testing-checklist)
49. [Offline Testing Procedure](#offline-testing-procedure)
50. [Barcode Scanner Testing Procedure](#barcode-scanner-testing-procedure)
51. [Authentication Persistence Testing](#authentication-persistence-testing)
52. [Notification Testing](#notification-testing)
53. [Timetable Testing](#timetable-testing)
54. [Admin Testing](#admin-testing)
55. [Known Constraints](#known-constraints)
56. [Future Enhancements](#future-enhancements)
57. [Suggested Report / Viva Points](#suggested-report--viva-points)
58. [Project Outcome](#project-outcome)

---

# Project Overview

**ScanMitra** is a smart attendance platform that replaces slow manual attendance workflows with a mobile camera based barcode system.

Instead of requiring a USB barcode reader, Raspberry Pi, or dedicated scanner hardware, the teacher uses a **mobile phone camera inside the web application**.

Each student has a college identity card containing a barcode. When the teacher points the mobile camera at the barcode:

1. The barcode is decoded.
2. The PID/student identifier is extracted.
3. The student is validated against the current lecture.
4. Duplicate attendance is checked.
5. Attendance is marked as **Present** or **Late** according to the lecture rules.
6. The teacher dashboard updates.
7. The student dashboard updates after synchronization.
8. A student notification is generated after successful attendance commitment.

When there is no network connection, the same process continues locally using an **offline-first architecture** and the attendance is synchronized later.

---

# Problem Statement

Traditional attendance systems create several classroom problems:

- Teachers spend several minutes taking attendance.
- Manual roll calls are repetitive.
- Students may answer on behalf of absent students.
- Teachers may need to maintain paper records.
- Dedicated barcode hardware can increase cost and maintenance.
- Internet availability can be unreliable in classrooms.
- Attendance records may not immediately reach students.
- Timetable information may need to be managed separately.
- Manual attendance is still required when a student cannot present an ID card.

ScanMitra addresses these problems through a combined:

**barcode + manual + online + offline + synchronization + notification**

architecture.

---

# Objectives

The main objectives are:

### 1. Automate attendance

Use the existing college ID barcode to identify students.

### 2. Reduce attendance time

Allow teachers to scan students quickly rather than manually calling names.

### 3. Support mobile-only scanning

Use a phone camera instead of external scanner hardware.

### 4. Support offline classrooms

Continue attendance even when the internet temporarily disappears.

### 5. Synchronize automatically

Upload offline attendance when the network returns.

### 6. Keep students informed

Send an in-app notification when attendance is successfully recorded.

### 7. Support manual attendance

Allow teachers to mark students who are physically present but cannot scan their ID card.

### 8. Support role-based access

Separate Admin, Teacher, and Student experiences.

### 9. Support dynamic timetables

Use weekly timetable data to generate lecture instances and drive teacher notifications.

### 10. Provide a polished product experience

Use responsive, animated, modern interfaces rather than a basic CRUD dashboard.

---

# Core Idea

The core system can be summarized as:

```text
College ID Barcode
        ↓
Mobile Camera
        ↓
ZXing Decoder
        ↓
PID / Student Identification
        ↓
Student Validation
        ↓
Current Lecture Validation
        ↓
Duplicate Protection
        ↓
Attendance
   ↙          ↘
Online       Offline
  ↓             ↓
Firestore     IndexedDB
  ↓             ↓
Notification   Pending Queue
  ↓             ↓
Student UI ← Automatic Sync
```

---

# Key Features

## Attendance

- Mobile barcode scanning
- Manual attendance
- Present status
- Late status
- Duplicate protection
- Lecture-specific attendance
- Teacher live attendance
- Student attendance history
- Current lecture attendance counters

## Offline

- Offline session
- IndexedDB local storage
- Offline barcode attendance
- Offline manual attendance
- Pending synchronization queue
- Automatic synchronization after reconnect
- Local dashboard visibility while offline

## Timetable

- Weekly schedule
- Lecture time
- Attendance window
- Division
- Classroom
- Teacher assignment
- CSV import
- Future photo/OCR import pipeline
- Automatic lecture detection

## Notifications

- Attendance marked
- Lecture reminders
- Offline synchronization
- Student in-app notifications
- Teacher lecture reminders
- Stored notifications for later login

## Dashboards

- Admin dashboard
- Teacher dashboard
- Student dashboard
- Attendance analytics
- Live attendance
- Schedule views
- Profile pages
- Activity information

## Security

- Firebase authentication
- Server-side role validation
- Teacher ownership validation
- Student-scoped access
- Admin-scoped access
- Duplicate attendance protection
- No plaintext passwords in Firestore
- Sensitive credentials kept outside frontend code

---

# User Roles

## Admin

Admin is the system management role.

Admin can manage:

- students
- teachers
- timetable
- lectures
- attendance
- reports
- notifications
- activity logs
- system health
- settings

Admin access is intentionally separated from the normal login screen.

### Admin access

```text
/admin
```

The normal `/login` page does not expose an Admin role.

---

## Teacher

Teachers can:

- log in
- see their timetable
- see their current lecture
- start attendance
- scan student IDs
- manually mark students
- use offline attendance
- view live attendance
- view attendance roster
- view schedule
- receive lecture notifications
- manage development/testing sessions

Teacher data is scoped to the teacher's own lectures and authorized students.

---

## Student

Students can:

- log in
- view attendance
- view history
- view schedule
- receive notifications
- view profile
- see attendance percentages and subject summaries
- see testing/development attendance when it exists

Students are read-only for attendance.

---

# System Architecture

## High-Level Architecture

```text
                         ┌─────────────────────┐
                         │      Browser        │
                         │ React + Vite        │
                         └──────────┬──────────┘
                                    │
                           HTTPS / Same Origin
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
             Frontend / Vite                  Firebase Web SDK
                    │                               │
               /api proxy                           │
                    │                               │
                    ▼                               ▼
             ┌───────────────┐              ┌───────────────┐
             │ Node / Express│              │ Firebase      │
             │ Backend       │              │ Authentication│
             └───────┬───────┘              └───────┬───────┘
                     │                                │
                     ▼                                ▼
             Firebase Admin SDK                 Firestore
                     │
                     └───────────────┬────────────────┘
                                     │
                             Attendance Data
```

## Offline Architecture

```text
Mobile Browser
     │
     ├── Camera
     ├── React UI
     ├── Service Worker / PWA
     └── IndexedDB
             │
             ├── Cached Students
             ├── Cached Lectures
             ├── Offline Session
             ├── Attendance Queue
             └── Sync Metadata
                     │
                Network Returns
                     │
                     ▼
              Backend Sync API
                     │
                     ▼
                  Firestore
```

---

# Technology Stack

## Frontend

- React
- Vite
- JavaScript / JSX
- Tailwind CSS
- Framer Motion
- Lucide React
- Firebase Web SDK
- `@zxing/browser`
- Dexie / IndexedDB
- Vite PWA support

## Backend

- Node.js
- Express
- Firebase Admin SDK
- Firebase Authentication verification
- Firestore
- server-side attendance validation
- offline session signing
- synchronization APIs

## Database / Cloud

- Firebase Authentication
- Cloud Firestore
- Firebase Storage where required
- Firebase Admin SDK

## Browser Technologies

- `getUserMedia`
- `MediaStream`
- `MediaStreamTrack`
- IndexedDB
- Service Worker
- PWA

---

# Project Structure

```text
Smart-Attendance-System/
│
├── backend/
│   ├── src/
│   │   ├── routes/
│   │   ├── services/
│   │   ├── middleware/
│   │   ├── scripts/
│   │   └── app.js
│   ├── package.json
│   └── .env
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── context/
│   │   ├── firebase/
│   │   ├── hooks/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── offline/
│   │   ├── utils/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── styles.css
│   ├── .env
│   ├── package.json
│   └── vite.config.js
│
├── firebase.json
├── firestore.rules
├── README.md
└── .gitignore
```

---

# Application Flow

## Complete End-to-End Flow

```text
Teacher Login
      ↓
Teacher Dashboard
      ↓
Current Lecture Detection
      ↓
Scanner / Manual Attendance
      ↓
Student Validation
      ↓
Attendance Validation
      ↓
Duplicate Check
      ↓
Online?
 ┌────┴────┐
 YES       NO
 │          │
 ▼          ▼
Firestore  IndexedDB
 │          │
 │       Pending Queue
 │          │
 └────┬─────┘
      ↓
Teacher Dashboard
      ↓
Student Notification
      ↓
Student Dashboard
```

---

# Teacher Workflow

1. Teacher logs in.
2. Firebase authentication restores teacher identity.
3. Dashboard loads teacher-specific timetable.
4. Current lecture is detected from schedule/time.
5. Teacher opens scanner or manual attendance.
6. Student ID is scanned or student is manually selected.
7. System validates the student.
8. System validates the lecture.
9. Duplicate attendance is prevented.
10. Attendance is marked.
11. Teacher live attendance updates.
12. Student receives notification.
13. Offline records are synchronized when connection returns.

---

# Student Workflow

1. Student logs in using the configured student authentication flow.
2. Firebase authentication restores the session.
3. Student dashboard loads personal profile and attendance.
4. Student can view:
   - attendance
   - history
   - schedule
   - notifications
   - profile
5. When teacher marks attendance:
   - attendance becomes available
   - notification appears
6. If the student was offline/logged out:
   - notification remains stored
   - it is visible after the next session.

---

# Admin Workflow

Admin enters:

```text
/admin
```

Admin authentication is handled separately from normal Teacher/Student login.

Admin can manage:

```text
Admin Dashboard
     │
     ├── Students
     ├── Teachers
     ├── Timetable
     ├── Lectures
     ├── Attendance
     ├── Reports
     ├── Notifications
     ├── Activity Logs
     ├── System Health
     └── Settings
```

---

# Barcode Attendance

## Purpose

The scanner uses the student's existing college ID barcode.

No USB scanner is required.

No Raspberry Pi is required.

No external barcode device is required.

The phone camera is the scanner.

## Barcode Process

```text
Camera
  ↓
MediaStream
  ↓
ZXing
  ↓
Raw barcode value
  ↓
PID parsing
  ↓
Student lookup
  ↓
Lecture validation
  ↓
Duplicate check
  ↓
Attendance
```

## Scanner Technology

`@zxing/browser`

The scanner is designed to:

- use the rear camera
- continuously decode
- support common 1D barcode formats
- request useful resolution
- use autofocus where the browser/device exposes it
- optionally control torch where supported
- continue operating when torch is unavailable
- avoid duplicate readers/streams
- work with physical IDs and barcodes shown on another screen

## Important Device Compatibility Rule

Torch is optional.

A phone can have:

```text
Camera = connected
Torch = unavailable
ZXing = running
```

and the scanner should still function.

Browser hardware capability exposure varies by device.

---

# Manual Attendance

Manual attendance exists for students who are physically present but:

- forgot their ID card
- damaged the ID card
- cannot scan the barcode
- have a barcode that cannot be read immediately

## Flow

```text
Teacher Dashboard
      ↓
Manual Attendance
      ↓
Current Lecture
      ↓
Student Search / Roster
      ↓
Select Student
      ↓
Present or Late
      ↓
Confirm
      ↓
Attendance
```

## Manual Attendance Rules

- Teacher can mark Present.
- Teacher can mark Late.
- Students are not automatically marked Absent.
- Unmarked students remain `Not Marked`.
- Duplicate protection is identical to barcode attendance.
- Manual attendance uses the same attendance identity.
- Manual records carry an attendance source.

---

# Offline-First Attendance

The system is designed to continue attendance in classrooms without internet connectivity.

## Offline Flow

```text
Teacher logs in
      ↓
Offline bootstrap
      ↓
Today's lecture cached
Students cached
Offline session issued
      ↓
Network disconnects
      ↓
Scan / Manual
      ↓
Local validation
      ↓
IndexedDB
      ↓
Pending Sync
      ↓
Network returns
      ↓
Backend Sync API
      ↓
Firestore
      ↓
Synced
```

## Offline Data

The browser can cache only the minimum information required to continue the supported offline workflow.

Sensitive credentials are not cached.

---

# Automatic Synchronization

Synchronization runs when connectivity returns or a sync action is triggered.

## Typical States

```text
Pending Sync
Syncing
Synced
Failed
```

## Duplicate Sync Protection

Submitting the same offline attendance multiple times must not create multiple attendance records.

The deterministic lecture/student attendance identity protects the final write.

---

# Student Notifications

Whenever attendance is successfully committed, the student can receive an in-app notification.

Examples:

```text
Attendance Marked
Your attendance for AIML has been marked Present.
```

```text
Attendance Marked
Your attendance for Design Thinking has been marked Late.
```

For development mode, the record can be labeled as a development/test attendance.

## Notification Timing

Notifications should be created after successful attendance commitment.

A failed attendance attempt should not create a false success notification.

---

# Timetable Management

Timetables drive current lecture detection.

A schedule contains concepts such as:

- teacher
- day
- subject
- division
- classroom
- start time
- end time
- attendance window
- department
- source

The same timetable source should drive:

- teacher dashboard
- schedule page
- faculty profile
- current lecture
- reminders
- attendance context

---

# CSV Timetable Import

CSV import is supported as a structured timetable source.

All imported rows should be validated before they are accepted.

Typical fields include:

```text
day
subject
teacher
division
classroom
startTime
endTime
attendanceWindowEnd
department
```

---

# Timetable Photo / OCR Architecture

The system is designed to accommodate timetable photographs.

Target flow:

```text
Timetable Photo
      ↓
Vision / OCR Extraction
      ↓
Structured Timetable
      ↓
Review
      ↓
Edit if needed
      ↓
Confirm
      ↓
weeklySchedules
```

A safe implementation should always provide a human review step before publishing OCR-generated timetable rows.

The photo and CSV import paths should ultimately write to the same schedule model instead of maintaining independent scheduling systems.

---

# Lecture Detection

Lecture instances are generated from schedule data.

Typical statuses:

```text
UPCOMING
PREPARING
ACTIVE
LATE
CLOSED
```

The teacher-facing application uses the current date/time to determine the relevant lecture state.

---

# Testing / Development Mode

Development mode exists to verify:

- scanner
- attendance
- manual attendance
- offline operation
- sync
- notifications
- dashboard updates

Each new test session should have its own lecture/session identity.

This prevents an old test attendance record from incorrectly blocking a new test session.

---

# Authentication

Firebase Authentication is the identity layer.

The application must distinguish:

```text
authLoading
authenticated
unauthenticated
```

and separately:

```text
online
offline
```

A temporary network failure is not the same as a logout.

## Refresh Persistence

After successful login:

```text
Dashboard
↓
Refresh
↓
Firebase restores auth state
↓
Same authenticated route
```

Protected routes should wait for auth hydration before redirecting to `/login`.

---

# Role-Based Authorization

## Student

Student APIs/data are scoped to the current student.

## Teacher

Teacher APIs verify the teacher's identity and ownership of the relevant lecture/schedule.

## Admin

Admin APIs verify authenticated admin authorization server-side.

Client-side route hiding is not the primary security boundary.

---

# Admin Portal

Admin entry point:

```text
/admin
```

Admin is intentionally not shown as a normal role on `/login`.

## Admin Navigation

- Dashboard
- Students
- Teachers
- Timetable
- Lectures
- Attendance
- Reports
- Notifications
- Activity Logs
- System Health
- Settings

## Admin Responsibilities

- manage teacher accounts
- manage student records
- manage schedules
- inspect lectures
- inspect attendance
- review reports
- manage notifications
- inspect activity
- inspect system status

---

# Teacher Portal

Teacher portal responsibilities:

- current lecture
- scanner
- manual attendance
- live attendance
- attendance roster
- timetable
- notifications
- profile
- offline status
- development/testing mode

The Teacher Dashboard is designed as a classroom control center.

---

# Student Portal

Student portal responsibilities:

- personal attendance
- history
- schedule
- notifications
- profile
- attendance progress

The Student Portal is read-only for attendance.

It should not contain teacher schedule management controls.

---

# Swapnil Chitnis Support

The project supports multiple teachers.

A development teacher scenario was added for:

**Swapnil Chitnis**

Subject:

**Design Thinking (DT)**

Department:

**Mechanical Engineering**

For development, a temporary timetable/roster can be used until the actual Mechanical Department timetable and roster are provided.

The important design requirement is that Swapnil's dashboard data should be derived from his assigned schedules and authorized roster, not global data hardcoded into the dashboard.

When the real timetable is uploaded, the common timetable source should update:

```text
Teacher Dashboard
Faculty Profile
Assigned Lecture Slots
Schedule
Current Lecture
Notifications
Attendance Context
```

---

# Attendance Status Model

Core attendance status:

```text
present
late
not_marked
```

Technical synchronization states can additionally include:

```text
pending_sync
synced
failed
```

These should be treated separately from attendance status.

---

# Attendance Data Model

Conceptually:

```json
{
  "lectureId": "...",
  "studentId": "...",
  "status": "present",
  "recordedAt": "...",
  "source": "barcode",
  "syncStatus": "synced"
}
```

Possible additional metadata:

```text
teacherId
subject
division
classroom
deviceId
offlineSessionId
syncedAt
```

---

# Duplicate Attendance Protection

The attendance identity is lecture-scoped and student-scoped.

Concept:

```text
lectureId + studentId
```

Therefore:

```text
Same lecture + same student
→ one attendance record
```

A second scan/manual action returns an already-marked result instead of creating another record.

---

# Firestore Collections

The project is organized around collections such as:

```text
users
teachers
students
studentCredentials
weeklySchedules
lectures
attendance
notifications
auditLogs
settings
```

The deployed project should be treated as the final source of truth for exact field names.

---

# Security Design

Key rules:

- no plaintext passwords
- no frontend service account keys
- no password hashes exposed to clients
- no unrestricted attendance writes
- students cannot create arbitrary attendance
- teacher ownership must be server-validated
- admin role must be server-validated
- Firestore rules should follow least privilege
- development/test reset should never delete unrelated production attendance
- credentials must remain out of source control

---

# Network Failure Handling

## Offline

The application should switch to an offline state without destroying authentication.

```text
Authenticated
+
Offline
```

is valid.

## Reconnect

```text
Online
↓
Sync Pending Records
↓
Update Dashboard
↓
Create notifications where needed
```

The user should not be unexpectedly redirected to `/login` because of a temporary network outage.

---

# Scanner Compatibility

The scanner is designed for mobile devices.

## Required

- HTTPS
- rear/environment camera
- camera permission
- stable video stream
- continuous decode
- supported barcode formats

## Capability Handling

Torch, continuous focus, and zoom are optional camera capabilities.

If unsupported, the scanner should gracefully fall back rather than treating an unavailable capability as an application failure.

## Samsung / Device Compatibility

Some Android devices expose camera capabilities differently through the browser. The implementation should prefer standard browser capability detection and avoid making torch support a requirement for barcode detection.

---

# Responsive Design

All portals are intended to support mobile, tablet, and desktop.

### Mobile

- compact top bar
- hamburger navigation
- one-column content
- touch-friendly controls
- no horizontal overflow

### Tablet

- compact navigation
- adaptive grids

### Desktop

- sticky/fixed top navbar
- sticky/fixed sidebar
- independently scrollable content

---

# UI / UX Design System

The ScanMitra interface is deliberately:

- bright
- modern
- colorful
- premium
- responsive
- animated

## Visual Elements

- gradient cards
- rounded corners
- subtle shadows
- soft borders
- glass effects where useful
- modern typography
- clear semantic status colors

## Animation

Framer Motion is used for:

- page entrances
- card reveals
- live scan entries
- drawer animations
- modal transitions
- small interaction feedback

Heavy 3D effects are avoided in favor of performance.

---

# Brand Identity

## Product Name

**ScanMitra**

## Tagline

**Ek Scan, Attendance Done.**

## Visual Identity

Main colors:

- blue
- indigo
- violet
- cyan
- teal
- magenta

The supplied ScanMitra artwork is used as the main brand visual in suitable public/product locations such as the landing/login experience.

Small navigation areas should use a compact brand treatment rather than forcing a large artwork into the header.

---

# Pages and Routes

## Public

```text
/
```

Landing page.

```text
/login
```

Teacher/Student login.

## Admin

```text
/admin
/admin/students
/admin/teachers
/admin/timetable
/admin/lectures
/admin/attendance
/admin/reports
/admin/notifications
/admin/activity
/admin/system
/admin/settings
```

## Teacher

Typical:

```text
/teacher/dashboard
/teacher/scanner
/teacher/schedule
/teacher/attendance
/teacher/profile
/teacher/notifications
/teacher/reports
```

## Student

Typical:

```text
/student/dashboard
/student/attendance
/student/history
/student/schedule
/student/notifications
/student/profile
```

Actual route names should follow the current deployed routing configuration.

---

# Environment Configuration

## Frontend

```env
VITE_FIREBASE_API_KEY=
VITE_FIREBASE_AUTH_DOMAIN=
VITE_FIREBASE_PROJECT_ID=
VITE_FIREBASE_STORAGE_BUCKET=
VITE_FIREBASE_MESSAGING_SENDER_ID=
VITE_FIREBASE_APP_ID=
VITE_API_BASE_URL=
```

The production/mobile development configuration should use HTTPS and a same-origin/relative API strategy where the Vite proxy is part of the deployment workflow.

## Backend

```env
PORT=5000
FIREBASE_PROJECT_ID=
FIREBASE_CLIENT_EMAIL=
FIREBASE_PRIVATE_KEY=
```

Never commit secret environment files or service-account keys.

---

# Installation

## Frontend

```bash
cd frontend
npm install
```

## Backend

```bash
cd backend
npm install
```

---

# Running the Project

## Backend

```bash
cd backend
npm start
```

Expected development port:

```text
5000
```

## Frontend

```bash
cd frontend
npm run dev
```

Expected development port:

```text
5173
```

---

# Development Commands

## Frontend

```bash
npm run dev
npm run build
```

## Backend

```bash
npm start
```

## Health

```text
/api/health
```

Use the project health endpoint to verify that the Express backend is reachable.

---

# Deployment

For production/mobile camera use:

- deploy over HTTPS
- configure frontend Firebase settings
- configure backend Firebase Admin settings
- configure Firestore rules
- verify authentication providers
- verify API routing
- test the deployed scanner on physical devices

For development mobile testing, the project can be exposed through a single HTTPS tunnel with the frontend serving `/api` through its proxy.

---

# Mobile Testing

Recommended physical-device tests:

1. Open the deployed HTTPS URL.
2. Log in as a teacher.
3. Verify teacher session.
4. Start an eligible lecture.
5. Open scanner.
6. Grant camera permission.
7. Scan a physical ID.
8. Scan a barcode shown on another phone.
9. Test manual attendance.
10. Disable network.
11. Test offline scan.
12. Test offline manual attendance.
13. Reconnect.
14. Verify sync.
15. Log in as student.
16. Verify attendance and notification.

---

# Functional Testing Checklist

## Authentication

- [ ] Teacher login
- [ ] Student login
- [ ] Admin `/admin`
- [ ] Logout
- [ ] Browser refresh
- [ ] Inner-route refresh
- [ ] Network loss without logout

## Barcode

- [ ] Rear camera
- [ ] Camera permission
- [ ] ZXing running
- [ ] Physical college ID
- [ ] Barcode on another phone
- [ ] PID extraction
- [ ] Student lookup
- [ ] Attendance handoff

## Manual Attendance

- [ ] Open Manual Attendance
- [ ] Search student
- [ ] Mark Present
- [ ] Mark Late
- [ ] Duplicate protection
- [ ] Live attendance update
- [ ] Offline manual attendance

## Attendance

- [ ] Present count
- [ ] Pending Sync
- [ ] Synced
- [ ] Not Marked
- [ ] View Attendance
- [ ] Live Scans
- [ ] Student dashboard

## Offline

- [ ] Bootstrap
- [ ] Student cache
- [ ] Lecture cache
- [ ] Offline scan
- [ ] Offline manual attendance
- [ ] Queue
- [ ] Refresh offline
- [ ] Reconnect
- [ ] Sync
- [ ] Student notification

## Timetable

- [ ] Weekly schedule
- [ ] Current lecture
- [ ] Attendance window
- [ ] CSV import
- [ ] Photo/OCR pipeline
- [ ] Review before publish
- [ ] Assigned lecture slot calculation
- [ ] Notifications derived from timetable

---

# Offline Testing Procedure

### 1. Prepare online

Teacher logs in and starts/prepares a lecture.

Confirm the device has:

- cached lecture
- cached students
- valid offline session

### 2. Disable Internet

Turn off:

- Wi-Fi
- mobile data

### 3. Scan

Scan a student.

Expected:

```text
Attendance saved locally
Pending Sync = 1
```

### 4. Manual Attendance

Mark another eligible student manually.

Expected:

```text
Pending Sync increases
```

### 5. Refresh Offline

Expected:

- session remains authenticated
- local state remains
- pending records remain

### 6. Reconnect

Expected:

```text
Syncing
→
Synced
```

### 7. Student Verification

Student logs in and sees the attendance and corresponding notification.

---

# Barcode Scanner Testing Procedure

### Physical ID

Test with an actual college ID barcode.

Expected:

```text
Camera CONNECTED
ZXing RUNNING
Barcode detected
Raw value
Parsed student identifier
Student resolved
```

### Other Phone Screen

Display a supported barcode on another phone.

Scan with the teacher phone.

Expected:

```text
Barcode detected
```

Torch availability must not determine whether scanning works.

### Low Light

If the browser/device supports torch:

use it.

If torch is unavailable:

the scanner should continue normally and the UI should provide sensible guidance.

### Mild Blur

The scanner should benefit from autofocus where supported.

Strongly unreadable/blurred barcodes cannot be guaranteed to decode.

---

# Authentication Persistence Testing

## Teacher

```text
Login
↓
Dashboard
↓
Refresh
↓
Dashboard remains
```

## Student

```text
Login
↓
Dashboard
↓
Refresh
↓
Dashboard remains
```

## Admin

```text
/admin
↓
Login
↓
Dashboard
↓
Refresh
↓
Dashboard remains
```

Network loss must not automatically redirect an authenticated user to `/login`.

---

# Notification Testing

## Online

```text
Teacher marks attendance
↓
Successful attendance
↓
Notification created
↓
Student sees notification
```

## Offline

```text
Teacher marks attendance
↓
Pending Sync
↓
Network returns
↓
Backend accepts attendance
↓
Notification created
↓
Student sees notification
```

## Duplicate

```text
Same student
same lecture
second attempt
↓
Already Marked
```

No second attendance notification.

---

# Timetable Testing

## Development Schedule

A development timetable is useful for testing:

- current lecture detection
- attendance windows
- notifications
- scanner
- manual attendance
- offline mode

Development schedules must be clearly separated from real academic scheduling.

## Real Timetable Update

When actual timetable data is imported:

```text
Import
↓
Validate
↓
Review
↓
Confirm
↓
Save
```

Then verify:

- teacher dashboard
- current lecture
- faculty profile
- assigned lecture slots
- schedule
- notification schedule

all update from the common schedule source.

---

# Admin Testing

Verify:

- Admin is not shown in normal `/login`
- `/admin` opens Admin Login
- authorized Admin reaches Admin Dashboard
- teacher/student accounts cannot call admin APIs
- student records load
- teacher records load
- timetable loads
- attendance loads
- reports load
- notifications load
- activity logs load
- system health loads

---

# Data Integrity Principles

The application should follow these principles:

### Single source of truth

Schedules should drive lecture state.

### Lecture-scoped attendance

Attendance belongs to a specific lecture.

### Student-scoped access

Students see their own records.

### Teacher-scoped access

Teachers see their own authorized lectures/rosters.

### Admin-wide access

Admins can manage system-wide resources subject to authorization.

### No global roster leakage

A student existing globally does not automatically make them eligible for every teacher/lecture.

---

# Known Constraints

1. Browser camera capabilities differ across devices.
2. Torch support is not guaranteed through browser APIs.
3. Continuous focus control is device-dependent.
4. Some Samsung/browser camera combinations can expose capabilities differently.
5. Extreme barcode blur cannot be guaranteed to decode.
6. Offline operation requires the necessary data to have been cached first.
7. Browser storage can be cleared.
8. OCR/table-photo extraction depends on image quality and configured vision capability.
9. OCR results should be reviewed before publication.
10. Cloud/service features can depend on Firebase project configuration and plan.

---

# Future Enhancements

Potential improvements include:

- stronger barcode image preprocessing
- scan sound/vibration
- improved camera compatibility heuristics
- timetable OCR confidence scoring
- richer attendance analytics
- attendance shortage prediction
- push notifications
- email/SMS/WhatsApp notification integration
- advanced audit logging
- advanced admin analytics
- bulk student import
- bulk teacher import
- academic calendar integration
- multi-college / multi-campus support

---

# Suggested Report / Viva Points

## Why ScanMitra?

To reduce attendance effort while retaining control, visibility, and reliability.

## Why barcode?

Existing college IDs can be reused.

## Why mobile camera?

No dedicated barcode hardware is required.

## Why offline-first?

Attendance should continue even when classroom connectivity is unreliable.

## Why manual attendance?

Students can be present without a usable ID card.

## How is duplication prevented?

Attendance is uniquely scoped by lecture and student.

## How are students notified?

A successful attendance commitment generates an in-app notification.

## How does timetable affect the system?

The timetable determines the active/upcoming lecture and its attendance context.

## How is security handled?

Firebase Authentication plus backend/server authorization and scoped data access.

## What makes the project different?

The system combines:

**mobile barcode scanning + manual attendance + offline-first operation + automatic synchronization + notifications + timetable-driven lectures + role-specific management**

in a single responsive platform.

---

# Project Outcome

ScanMitra combines classroom attendance requirements into one system:

```text
                   SCANMITRA
             "Ek Scan, Attendance Done."
                         │
         ┌───────────────┼────────────────┐
         │               │                │
      ADMIN            TEACHER          STUDENT
         │               │                │
         ▼               ▼                ▼
    Management       Attendance       My Attendance
    Students         Scanner          History
    Teachers         Manual           Schedule
    Timetable        Offline          Notifications
    Reports          Sync              Profile
    System Health    Live View
         │               │
         └───────┬───────┘
                 ▼
          Firebase / Backend
                 │
        ┌────────┴────────┐
        ▼                 ▼
    Firestore          IndexedDB
        │                 │
        └───────┬─────────┘
                ▼
         Automatic Sync
```

The system is built around the principle:

> **Attendance should be fast for teachers, visible to students, manageable by administrators, and resilient when classroom connectivity is unreliable.**

---

# Final Project Summary

| Category | ScanMitra |
|---|---|
| Project Name | ScanMitra |
| Tagline | Ek Scan, Attendance Done. |
| Category | Smart Attendance / EdTech |
| Interface | Responsive Web App / PWA |
| Identification | College ID Barcode |
| Scanner | Mobile Phone Camera |
| Barcode Library | `@zxing/browser` |
| Frontend | React + Vite |
| Styling | Tailwind CSS |
| Animation | Framer Motion |
| Icons | Lucide React |
| Backend | Node.js + Express |
| Authentication | Firebase Authentication |
| Database | Cloud Firestore |
| Offline DB | IndexedDB / Dexie |
| Offline Sync | Backend synchronization API |
| Roles | Admin, Teacher, Student |
| Attendance Modes | Barcode + Manual |
| Timetable Sources | Manual / CSV / Photo-OCR-ready architecture |
| Notifications | In-app |
| Main Differentiator | Offline-first mobile attendance |

---

# Final One-Line Description

**ScanMitra is a mobile-first, offline-capable barcode and manual attendance platform that connects teachers, students, timetables, notifications, synchronization, and administration in one responsive system.**

---

## Security Reminder

This README intentionally does **not** contain real passwords, PINs, private keys, service-account credentials, or other secrets.

Keep all secrets in environment variables or secure credential/configuration systems and never commit them to Git.
