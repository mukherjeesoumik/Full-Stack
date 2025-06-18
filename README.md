# Full-Stack

## 📘 Spring Boot – Student Management API (Backend)
### 🗂️ 1. Folder Structure
```cs
springboot-student-api/
├── src/
│   └── main/
│       ├── java/com/example/studentapi/
│       │   ├── controller/        → REST APIs (HTTP requests)
│       │   │   └── StudentController.java
│       │   ├── dto/               → Data Transfer Objects
│       │   │   └── StudentDTO.java
│       │   ├── entity/            → Maps to DB tables
│       │   │   └── Student.java
│       │   ├── repository/        → Database operations
│       │   │   └── StudentRepository.java
│       │   ├── service/           → Business logic
│       │   │   └── StudentService.java
│       │   └── StudentApiApplication.java → Main class
│       └── resources/
│           └── application.properties     → Config (DB, port, etc.)
└── pom.xml                            → Project dependencies

```
### 📄 2. Entity Layer
🔹 Purpose:
Represents a database table.

Annotated with @Entity.

✅ Student.java
```cs
package com.example.studentapi.entity;

import jakarta.persistence.*;

@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;

    // Getters and Setters
}
```
or for full ⤵️
```cs
@RestController
@RequestMapping("/api/students")
@CrossOrigin(origins = "http://localhost:3000")
public class StudentController {

    @Autowired
    private StudentService service;

    // POST - Add new student
    @PostMapping
    public Student addStudent(@RequestBody StudentDTO dto) {
        return service.saveStudent(dto);
    }

    // GET - All students
    @GetMapping
    public List<StudentDTO> getAll() {
        return service.getAllStudents();
    }

    // GET - Student by ID
    @GetMapping("/{id}")
    public StudentDTO getById(@PathVariable Long id) {
        return service.getStudentById(id);
    }

    // PUT - Update student
    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Long id, @RequestBody StudentDTO dto) {
        return service.updateStudent(id, dto);
    }

    // DELETE - Delete student
    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable Long id) {
        service.deleteStudent(id);
        return "Student deleted successfully.";
    }
}

```
### 📄 3. DTO (Data Transfer Object) Layer
🔹 Purpose:
Used to send/receive data to/from frontend.

Does not represent a DB table.

Hides unnecessary or sensitive fields.

✅ StudentDTO.java
```cs
package com.example.studentapi.dto;

public class StudentDTO {
    private String name;
    private String email;

    // Getters and Setters
}
```
### 📄 4. Repository Layer
🔹 Purpose:
Handles database operations like save(), findAll(), deleteById().

✅ StudentRepository.java
```ch
package com.example.studentapi.repository;

import com.example.studentapi.entity.Student;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {}
```
### 📄 5. Service Layer
🔹 Purpose:
Contains business logic.

Converts between Entity ↔ DTO.

Talks to Repository.

✅ StudentService.java
```cs
package com.example.studentapi.service;

import com.example.studentapi.dto.StudentDTO;
import com.example.studentapi.entity.Student;
import com.example.studentapi.repository.StudentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.stream.Collectors;

@Service
public class StudentService {

    @Autowired
    private StudentRepository repo;

    public Student saveStudent(StudentDTO dto) {
        Student s = new Student();
        s.setName(dto.getName());
        s.setEmail(dto.getEmail());
        return repo.save(s);
    }

    public List<StudentDTO> getAllStudents() {
        return repo.findAll().stream().map(student -> {
            StudentDTO dto = new StudentDTO();
            dto.setName(student.getName());
            dto.setEmail(student.getEmail());
            return dto;
        }).collect(Collectors.toList());
    }
}
```
### 📄 6. Controller Layer
🔹 Purpose:
Handles HTTP requests (GET, POST, etc).

Talks to the Service layer.

Returns data to frontend in JSON format.

✅ StudentController.java
```cs
package com.example.studentapi.controller;

import com.example.studentapi.dto.StudentDTO;
import com.example.studentapi.entity.Student;
import com.example.studentapi.service.StudentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/students")
@CrossOrigin(origins = "http://localhost:3000")  // For React CORS
public class StudentController {

    @Autowired
    private StudentService service;

    @PostMapping
    public Student addStudent(@RequestBody StudentDTO studentDTO) {
        return service.saveStudent(studentDTO);
    }

    @GetMapping
    public List<StudentDTO> getAll() {
        return service.getAllStudents();
    }
}
```
### ⚙️ 7. application.properties
🔹 Purpose:
Database connection configuration.

Auto table generation and logs.

✅ application.properties
properties
```cs
spring.datasource.url=jdbc:mysql://localhost:3306/studentdb
spring.datasource.username=root
spring.datasource.password=yourpassword

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
server.port=8080
```
### 🚀 8. Main Application File
✅ StudentApiApplication.java
java
```cs
package com.example.studentapi;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StudentApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(StudentApiApplication.class, args);
    }
}
```
### 🧠 Quick Summary of Layers

Layer	Purpose

Entity	Maps class to DB table

DTO	Transfers data between layers/frontends

Repository	DB interaction using Spring JPA

Service	Business logic, conversion DTO ↔ Entity

Controller	REST API layer, handles HTTP requests

Config	Application and DB configuration

Main	Launches Spring Boot app



✅ Spring Boot Concepts Recap:

Concept	Status	Description

📁 Folder Structure	✅	Clean modular separation of layers

🔹 Entity Layer	✅	Maps Java class to DB table

🔸 DTO Layer	✅	Sends/receives safe, trimmed data

🧩 Repository Layer	✅	Interface for CRUD DB access

🛠 Service Layer	✅	Business logic & Entity-DTO conversion

🌐 Controller Layer	✅	Exposes REST APIs

⚙️ Configuration	✅	application.properties for DB, CORS, etc.

🚀 Main Class	✅	App starting point

You now have a solid backend that can:

Add students

Get all students

Connect to a MySQL database

Be easily consumed by a React frontend

🚀 Let's Move to React Now
We'll now create a simple React frontend to:

Add a student (name, email)

View list of students

It will talk to the Spring Boot API we just built.

Type next to start React part with:

Folder structure

Components

Axios API connection

Simple Tailwind/Bootstrap UI

Let’s go! 🎯

## 🌐 React Project – Student Management UI
### 🗂️ 1. Folder Structure (Vite + Axios + Tailwind)
pgsql
```cs
student-ui/
├── src/
│   ├── api/                   → Axios setup + API functions
│   │   └── studentApi.js
│   ├── components/            → Reusable components
│   │   ├── StudentForm.jsx
│   │   └── StudentList.jsx
│   ├── App.jsx                → Root component
│   ├── main.jsx               → React entry point
├── public/
│   └── index.html
├── package.json
├── tailwind.config.js
```
We’ll use Vite for fast bundling, Axios for HTTP requests, and Tailwind CSS for styling.

 ✅ 2. Step-by-Step Setup
### 🔧 2.1. Initialize Project with Vite + Tailwind
bash
```cs
npm create vite@latest student-ui --template react
cd student-ui
npm install
npm install axios
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
### 🛠️ 2.2. tailwind.config.js
js
```cs
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```
### 📄 2.3. Add Tailwind to index.css
css
```cs
/* src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```
### 🧩 3. API Layer – src/api/studentApi.js
js
```cs
import axios from 'axios';

const BASE_URL = 'http://localhost:8080/api/students';

export const addStudent = (student) => {
  return axios.post(BASE_URL, student);
};

export const fetchStudents = () => {
  return axios.get(BASE_URL);
};
```
### 🧾 4. Student Form – src/components/StudentForm.jsx
jsx
```cs
import React, { useState } from 'react';
import { addStudent } from '../api/studentApi';

const StudentForm = ({ onStudentAdded }) => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    await addStudent({ name, email });
    setName('');
    setEmail('');
    onStudentAdded(); // Refresh list
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-4 p-4 bg-white rounded shadow-md">
      <input
        type="text"
        value={name}
        placeholder="Name"
        onChange={(e) => setName(e.target.value)}
        className="border p-2 w-full"
        required
      />
      <input
        type="email"
        value={email}
        placeholder="Email"
        onChange={(e) => setEmail(e.target.value)}
        className="border p-2 w-full"
        required
      />
      <button type="submit" className="bg-blue-500 text-white px-4 py-2 rounded">
        Add Student
      </button>
    </form>
  );
};

export default StudentForm;
```
### 📋 5. Student List – src/components/StudentList.jsx
jsx
```cs
import React, { useEffect, useState } from 'react';
import { fetchStudents } from '../api/studentApi';

const StudentList = ({ refresh }) => {
  const [students, setStudents] = useState([]);

  useEffect(() => {
    fetchStudents().then((res) => setStudents(res.data));
  }, [refresh]);

  return (
    <div className="mt-6">
      <h2 className="text-xl font-bold mb-2">Student List</h2>
      <ul className="space-y-2">
        {students.map((s, idx) => (
          <li key={idx} className="p-2 bg-gray-100 rounded shadow-sm">
            <span className="font-medium">{s.name}</span> – {s.email}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default StudentList;
```
### ⚛️ 6. App.jsx – Connect Everything
jsx
```cs
import React, { useState } from 'react';
import StudentForm from './components/StudentForm';
import StudentList from './components/StudentList';

function App() {
  const [refresh, setRefresh] = useState(false);

  const triggerRefresh = () => setRefresh(!refresh);

  return (
    <div className="max-w-xl mx-auto mt-10 p-4">
      <h1 className="text-2xl font-bold text-center mb-6">Student Management</h1>
      <StudentForm onStudentAdded={triggerRefresh} />
      <StudentList refresh={refresh} />
    </div>
  );
}

export default App;
```
### 🏁 7. Run the Frontend
bash
```cs
npm run dev
```
Then open: http://localhost:5173

✅ Final Output:
💬 Add student name and email.

📜 Display list of students from Spring Boot backend.

💡 Key Concepts Covered in React

Concept	Description

🧩 Axios	For HTTP API calls

🔄 useEffect	Fetch students on load/refresh

📥 useState	Manage form and data states

🧱 Component-based	Form and list as separate components

🎨 Tailwind CSS	Quick, clean UI
