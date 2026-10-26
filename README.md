# Company Management System

A web application built with **Laravel 12** to manage companies and their associated employees. It provides administrators with a clean interface for data entry, file uploads, automated email notifications via queue workers, and relational tracking.

---

## Detailed Features

### 1.  Company Management
- **Full CRUD Operations**: Create, view, update, and delete company records.
- **Logo Validation & Storage**:
  - Validates image files (`jpeg`, `png`, `jpg`, `gif`) with a strict minimum dimension rule of **100x100 pixels**.
  - Uploaded logos are saved to `storage/app/public/logos` and linked to the web via `php artisan storage:link`.
  - **Automatic Storage Cleanup**: When a company's logo is updated, the previous logo is automatically removed from disk to prevent orphaned files.
- **Background Email Notifications**:
  - Every time a new company is added, the application dispatches an asynchronous queue job (`SendCompanyCreatedEmail`).
  - Emails are sent via Laravel's Mailable system in the background, ensuring fast UI response times without blocking the user request.
- **Pagination**: Displays 10 companies per page for fast page loading and clean presentation.

### 2. Employee Management
- **Full CRUD Operations**: Manage employee details including first name, last name, email, and phone number.
- **Company Association**:
  - Every employee is linked to a parent company via a foreign key (`company_id`).
  - Company selection dropdowns are populated dynamically from existing companies.
- **Cascade Deletion**: Configured with database cascade delete—if a company is removed, all its corresponding employee records are cleaned up automatically.
- **Eager Loading Optimization**: Employee queries use `Employee::with('company')` to avoid N+1 query bottlenecks when displaying associated company information.
- **Pagination**: Lists 10 employees per page.

### 3.  Authentication & Access Control
- **Laravel Breeze Integration**: Provides secure authentication with session management, password resets, and account management.
- **Role-Based Setup**: Includes a `role` field on the `users` table to manage admin privileges.
- **Protected Routing**: Company and employee routes are guarded behind the `auth` middleware, preventing unauthorized access.
- **Pre-Configured Seeder**: Includes an `AdminSeeder` for instant access after initial database setup.

### 4.  UI & Asynchronous Interactions
- **AJAX-Driven Forms**: Create, update, and delete actions support AJAX requests with JSON responses for a responsive user experience.
- **SweetAlert2 Alerts**: Interactive confirmation dialogs before destructive actions (like deletions) and popup alerts for successful operations.
- **Dropzone.js Support**: Includes drag-and-drop file upload styling and handling.
- **Hybrid Styling**: Built using Tailwind CSS and Bootstrap 5 components for responsive layouts across desktop and mobile.

---

## Requirements

- **PHP**: `>= 8.2` (with `pdo`, `fileinfo`, and `gd` or `imagick` extensions enabled)
- **Composer**: `>= 2.x`
- **Node.js & npm**: Node 18+ and npm 9+
- **Database**: SQLite (default) or MySQL 8.0+

---

## Installation & Setup

1. **Install dependencies & build assets:**
   ```bash
   composer install
   npm install && npm run build
   ```

2. **Environment configuration:**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```
   *(On Windows PowerShell, run `copy .env.example .env`)*

3. **Database setup & seeding:**
   ```bash
   php artisan migrate --seed
   ```
   *This creates database tables and seeds the default admin account and sample companies.*

4. **Create storage symlink:**
   ```bash
   php artisan storage:link
   ```
   *Required for uploaded company logos to be publicly visible in the browser.*

5. **Run the application:**
   ```bash
   composer run dev
   ```
   *Runs the HTTP server, asset compiler, and queue worker concurrently. Alternatively, run them in separate terminals:*
   - `php artisan serve` (Web server at `http://localhost:8000`)
   - `php artisan queue:work` (Processes background email notifications)
   - `npm run dev` *(Optional during runtime if you already ran `npm run build`; only needed for hot-reloading frontend changes)*

---

## Default Admin Credentials

| Role | Email | Password |
|---|---|---|
| **Admin** | `admin@admin.com` | `password` |




