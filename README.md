# virtuo
Login:
1  Email: badmusabdulsalam955@gmail.com
   Password: SXb4k4hkd$k6ih*
2  Email: wasiusamad123@gmail.com
   Password: r97E3LG$YKT8xqP

Web App Task Management 
This project is a Laravel-based web application for managing projects and tasks. Each user can create projects, assign tasks with priorities (low, medium, high) to projects, and manage these tasks. Only project owners can add, update, or delete tasks for their projects.

Table of Contents
Features
Requirements
Installation
Database Setup
Routes
Authorization
Task Priority Filtering
Optimization
License
Features
Users can create projects and assign tasks to them.
Tasks have priorities (low, medium, high).
Only the project owner can create, update, or delete tasks.
Filtering tasks based on priority.
Requirements
PHP 8.x or higher
Composer
Node.js and npm
MySQL or compatible database
Installation
Clone the Repository

bash
Copy code
git clone <repository-url>
cd <project-directory>
Install Dependencies

bash
Copy code
composer install
npm install
npm run dev
Environment Setup

Duplicate .env.example and rename it to .env. Update the following fields in the .env file with your database and application information:

plaintext
Copy code
DB_DATABASE=your_database
DB_USERNAME=your_username
DB_PASSWORD=your_password
Generate Application Key

bash
Copy code
php artisan key:generate
Run Migrations

bash
Copy code
php artisan migrate
Run the Application

Start the development server:

bash
Copy code
php artisan serve
Database Setup
The database schema includes tables for users, projects, and tasks. Each task is associated with a specific project. The primary tables are as follows:

Projects Table

id: Primary key.
user_id: Foreign key referencing the user who created the project.
title: The title of the project.
Tasks Table

id: Primary key.
project_id: Foreign key referencing the associated project.
title: The title of the task.
description: Detailed description of the task.
priority: Enum for task priority (low, medium, high).
status: Active or inactive status.
Routes
Below are some essential routes in the application:

Route	Method	Description
/projects	GET	Show all projects
/projects/{id}	GET	Show a specific project
/projects	POST	Store a new project
/projects/{id}	PUT	Update a specific project
/projects/{id}	DELETE	Delete a specific project
/tasks	GET	Show all tasks
/tasks/{id}	GET	Show a specific task
/tasks	POST	Store a new task
/tasks/{id}	PUT	Update a specific task
/tasks/{id}	DELETE	Delete a specific task
/tasks/{priority}	GET	Show tasks filtered by priority
Authorization
Only project owners can create, update, or delete tasks within their projects. This is enforced in the TaskPolicy policy. To allow task creation or modification, the policy checks that the authenticated user’s ID matches the user_id of the project associated with the task.

Task Priority Filtering
To filter tasks by priority, a custom route (/tasks/{priority}) is created. The controller method validates the priority and retrieves tasks accordingly.

Example usage:

php
Copy code
public function tasks($priority)
{
    if (!in_array($priority, ['low', 'medium', 'high'])) {
        return response()->json(['error' => 'Invalid priority level'], 400);
    }

    $tasks = Task::where('priority', $priority)->get();
    return response()->json(['tasks' => $tasks], 200);
}
Optimization
With thousands of tasks, database query efficiency is essential. Here are some strategies used to optimize query performance:

Indexes: Indexes are added to foreign keys (user_id, project_id) to speed up lookups.
Eager Loading: To avoid the N+1 query problem, related models are eagerly loaded when fetching projects and tasks.
Pagination: For large data sets, pagination is implemented in routes to reduce data retrieval load.
License
This project is open-source and available for use under the MIT License.
