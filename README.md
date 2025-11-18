# Student Management System - Laravel Project

## Description / Overview

This is a **Student Management System** built with Laravel framework as part of the ITPC 115 Midterm Examination. The system provides a comprehensive solution for managing colleges, sections, and students within an educational institution. It implements a hierarchical structure where colleges contain sections, and sections contain students, allowing for efficient organization and management of academic data.

The application features a clean, modern user interface with full CRUD (Create, Read, Update, Delete) functionality for all entities, along with advanced features like student transfers between sections, filtering, search capabilities, and pagination.

---

## Objectives

The main goals and learning outcomes of this project include:

1. **Master Laravel Framework Fundamentals** - Understand routing, controllers, models, and views
2. **Implement Database Design** - Create and manage relational database structures with migrations
3. **Apply MVC Architecture** - Separate concerns using Model-View-Controller pattern
4. **Develop CRUD Operations** - Implement Create, Read, Update, and Delete functionality
5. **Utilize Eloquent ORM** - Work with Laravel's powerful database abstraction layer
6. **Build Relationships** - Establish and manage one-to-many database relationships
7. **Implement Validation** - Ensure data integrity through form validation
8. **Create User-Friendly UI** - Design intuitive interfaces for data management
9. **Handle Error Management** - Implement proper error handling and logging
10. **Practice Best Coding Practices** - Follow Laravel conventions and coding standards

---

## Features / Functionality

### College Management
- **View All Colleges** - Display list of all colleges with student counts
- **Create College** - Add new colleges with name and abbreviation
- **Edit College** - Update existing college information
- **Delete College** - Remove colleges (with validation to prevent deletion if sections exist)
- **View College Details** - See individual college information with associated sections

### Section Management
- **Create Section** - Add new sections assigned to specific colleges
- **Edit Section** - Update section details including name, year, and college assignment
- **Delete Section** - Remove sections (with validation to prevent deletion if students exist)
- **View Section Details** - Display section information with enrolled students list
- **College Association** - Sections are linked to their parent college

### Student Management
- **View All Students** - Paginated list of all students with their details
- **Create Student** - Register new students with complete information
- **Edit Student** - Update student records including personal and academic details
- **Delete Student** - Remove student records from the system
- **View Student Profile** - Display individual student information
- **Move Student** - Transfer students between sections and colleges
- **Advanced Filtering** - Filter students by college and section
- **Search Functionality** - Search students by ID, name, or email
- **Pagination** - Navigate through large student lists efficiently

### System Features
- **Responsive Design** - Mobile-friendly user interface
- **Error Handling** - Comprehensive error management with user feedback
- **Data Validation** - Server-side validation for all forms
- **Unique Constraints** - Prevent duplicate student IDs and emails
- **Relationship Management** - Maintain data integrity across related entities
- **Dynamic Dropdowns** - Auto-populated form fields based on selections
- **Success Notifications** - User feedback for successful operations

---

## Installation Instructions

Follow these steps to set up and run the project:

### Prerequisites
- PHP >= 8.1
- Composer
- MySQL or MariaDB
- Node.js and NPM (for frontend assets)
- Git (optional)

### Step 1: Clone or Download the Project
```bash
# If using Git
git clone <repository-url>
cd simplelaravelproject-ssm-main

# Or download and extract the ZIP file
```

### Step 2: Install PHP Dependencies
```bash
composer install
```

### Step 3: Environment Configuration
```bash
# Copy the environment file
copy .env.example .env

# Generate application key
php artisan key:generate
```

### Step 4: Configure Database
Edit the `.env` file and update the database settings:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### Step 5: Create Database
Create a new database in MySQL:
```sql
CREATE DATABASE your_database_name;
```

### Step 6: Run Migrations
```bash
php artisan migrate
```

### Step 7: Install Frontend Dependencies (Optional)
```bash
npm install
npm run build
```

### Step 8: Start the Development Server
```bash
php artisan serve
```

The application will be available at: `http://localhost:8000`

---

## Usage

### Getting Started

1. **Access the Application**
   - Open your browser and navigate to `http://localhost:8000`
   - You will be redirected to the Colleges page

2. **Create a College**
   - Click on "Add New College" button
   - Fill in the college name (e.g., "College of Computer Studies")
   - Fill in the abbreviation (e.g., "CCS")
   - Click "Save" to create the college

3. **Create a Section**
   - Navigate to "Add New Section"
   - Select the college from the dropdown
   - Enter section code (e.g., "BSIT")
   - Enter year level (e.g., "3")
   - Enter section name (e.g., "3A")
   - Click "Save" to create the section

4. **Add Students**
   - Go to the Students page
   - Click "Add New Student"
   - Fill in student information:
     - Student ID (must be unique)
     - First Name, Middle Initial, Last Name
     - Email (must be unique)
     - Contact Number
     - Select College
     - Select Section
   - Click "Save" to register the student

5. **Filter and Search**
   - Use the college dropdown to filter students by college
   - Use the section dropdown to filter students by section
   - Use the search box to find students by name, ID, or email
   - Click "Apply Filters" to see results

6. **Move a Student**
   - From the Students list, click on a student
   - Click the "Move Student" button
   - Select the new college and section
   - Click "Move" to transfer the student

7. **Edit Records**
   - Click the "Edit" button on any record
   - Update the information
   - Click "Save" to apply changes

8. **Delete Records**
   - Click the "Delete" button on any record
   - Confirm the deletion
   - Note: You cannot delete colleges with sections or sections with students

---

## Screenshots or Code Snippets

### Database Schema

#### Students Table Migration
```php
Schema::create('students', function (Blueprint $table) {
    $table->id();
    $table->string('student_id', 10)->unique();
    $table->string('fname', 150);
    $table->string('mi', 2)->nullable();
    $table->string('lname', 150);
    $table->string('email', 150)->unique();
    $table->string('contact', 20);
    $table->foreignId('college_id')->constrained('colleges');
    $table->foreignId('section_id')->constrained('sections');
    $table->timestamps();
});
```

### Model Relationships

#### Student Model
```php
class Student extends Model
{
    protected $fillable = [
        'student_id', 'fname', 'mi', 'lname', 
        'email', 'contact', 'college_id', 'section_id'
    ];

    // Student belongs to Section
    public function section() {
        return $this->belongsTo(Section::class);
    }

    // Student belongs to College
    public function college() {
        return $this->belongsTo(College::class);
    }

    // Accessor for full name
    public function getFullNameAttribute() {
        return trim("{$this->fname} {$this->mi} {$this->lname}");
    }
}
```

### Routing Example

```php
// College Routes
Route::get('/colleges', [CollegeController::class, 'index'])->name('colleges.index');
Route::get('/colleges/create', [CollegeController::class, 'create'])->name('colleges.create');
Route::post('/colleges', [CollegeController::class, 'store'])->name('colleges.store');
Route::get('/colleges/{college}', [CollegeController::class, 'show'])->name('colleges.show');
Route::get('/colleges/{college}/edit', [CollegeController::class, 'edit'])->name('colleges.edit');
Route::put('/colleges/{college}', [CollegeController::class, 'update'])->name('colleges.update');
Route::delete('/colleges/{college}', [CollegeController::class, 'delete'])->name('colleges.delete');

// Student Routes
Route::get('/students', [StudentController::class, 'index'])->name('students.index');
Route::get('/students/create', [StudentController::class, 'create'])->name('students.create');
Route::post('/students', [StudentController::class, 'store'])->name('students.store');
Route::get('/students/{student}/move', [StudentController::class, 'moveForm'])->name('students.moveForm');
Route::put('/students/{student}/move', [StudentController::class, 'move'])->name('students.move');
```

### Controller Logic Example

```php
public function store(Request $request) {
    $request->validate([
        'student_id' => 'required|unique:students,student_id|max:10',
        'lname' => 'required|string|max:150',
        'fname' => 'required|string|max:150',
        'mi' => 'nullable|string|max:2',
        'email' => 'required|email|max:150|unique:students,email',
        'contact' => 'required|max:20',
        'college_id' => 'required|exists:colleges,id',
        'section_id' => 'required|exists:sections,id'
    ]);

    $student = Student::create($request->all());

    return redirect()->route('students.index')
        ->with('success', 'Student created successfully!');
}
```

### Advanced Query with Filters

```php
$query = Student::with(['section', 'college']);

// Filter by college
if ($request->filled('college_id')) {
    $query->where('college_id', $request->college_id);
}

// Filter by section
if ($request->filled('section_id')) {
    $query->where('section_id', $request->section_id);
}

// Search functionality
if ($request->filled('search')) {
    $search = $request->search;
    $query->where(function($q) use ($search) {
        $q->where('student_id', 'like', '%' . $search . '%')
          ->orWhere('fname', 'like', '%' . $search . '%')
          ->orWhere('lname', 'like', '%' . $search . '%')
          ->orWhere('email', 'like', '%' . $search . '%');
    });
}

$students = $query->orderBy('lname')->orderBy('fname')->paginate(10);
```

---

## Project Structure

```
simplelaravelproject-ssm-main/
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       ├── CollegeController.php
│   │       ├── SectionController.php
│   │       └── StudentController.php
│   └── Models/
│       ├── College.php
│       ├── Section.php
│       └── Student.php
├── database/
│   └── migrations/
│       ├── 2025_09_25_190932_colleges.php
│       ├── 2025_09_25_191153_sections.php
│       ├── 2025_09_25_191156_students.php
│       └── [index migrations]
├── resources/
│   └── views/
│       ├── college/
│       ├── section/
│       └── student/
├── routes/
│   └── web.php
└── public/
    ├── css/
    └── js/
```

---

## Technologies Used

- **Backend Framework**: Laravel 11.x
- **PHP Version**: 8.1+
- **Database**: MySQL/MariaDB
- **Frontend**: Blade Templating Engine
- **CSS**: Custom CSS (app.css)
- **JavaScript**: Vanilla JS for dynamic interactions
- **ORM**: Eloquent
- **Validation**: Laravel Form Request Validation
- **Routing**: Laravel Router
- **Error Handling**: Laravel Exception Handler with Logging

---

## Key Concepts Implemented

### 1. MVC Architecture
- **Models**: College, Section, Student (Eloquent ORM)
- **Views**: Blade templates for all CRUD operations
- **Controllers**: Separate controllers for each entity

### 2. Database Design
- **One-to-Many Relationships**: College → Sections, Section → Students
- **Foreign Keys**: Proper referential integrity
- **Indexes**: Performance optimization for queries
- **Unique Constraints**: Student ID and email uniqueness

### 3. Laravel Features
- **Route Model Binding**: Automatic model resolution
- **Eloquent Relationships**: `belongsTo()` and `hasMany()`
- **Query Builder**: Complex filtering and searching
- **Pagination**: Built-in Laravel pagination
- **Validation**: Server-side form validation
- **Flash Messages**: Session-based notifications
- **Logging**: Error tracking with Laravel Log

---

## Contributors

**Student Name**: Delysha Grace Paz  
**Partner Name**: Alvin De Mesa  
**Course**: ITPC 115  
**Project**: Midterm Examination - Student Management System  
**Academic Year**: 2024-2025  
**Instructor**: Mr. Manny Rimorin Hortizuela

---

**Last Updated**: October 29, 2025
