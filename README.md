# GGCLIKS — ASP.NET Core MVC E-Commerce Platform

GGCLIKS is a modern, full-featured, university-viva-ready **E-Commerce Web Application** built with **ASP.NET Core MVC**, **Entity Framework Core Code First**, **PostgreSQL**, **ASP.NET Core Identity**, **Chart.js**, and **Google Gemini AI**.

Designed and structured specifically for **Microsoft Visual Studio 2022+**.

---

## 1. Project Overview

**GGCLIKS** is designed as a single, decoupled MVC web application that simulates a commercial retail storefront. It incorporates role-based access control, real-time AJAX validations, server-side catalog filtering and pagination, session-based shopping cart management, transactional order processing with inventory tracking, an interactive administrator dashboard with Chart.js, and a floating Google Gemini AI Shopping Assistant.

---

## 2. Key Features

### Customer Experience & Storefront
- **Modern Responsive Design**: Crafted using Bootstrap 5, Bootstrap Icons, responsive flexbox grids, and custom CSS with a persistent **Dark/Light Mode** toggle.
- **Product Catalog & Filtering**:
  - Full-text search on product names and descriptions.
  - Multi-criteria filtering by category, price range, minimum star rating, and in-stock availability.
  - Sorting by Newest, Price (Low to High), Price (High to Low), Highest Rated, and Name (A-Z).
  - Server-side pagination maintaining all active search/filter parameters across pages.
- **Product Details & Gallery**:
  - Image gallery carousel and interactive thumbnail picker.
  - Real-time stock status and low-stock alerts.
  - Related product recommendations within the same category.
- **5-Star Rating & Reviews**:
  - Interactive star picker UI with aggregate ratings calculation.
  - Add, update, or remove customer reviews.
- **Product Comments & Discussion**:
  - Q&A thread under each product with user attribution and timestamping.
- **Shopping Cart**:
  - Lightweight session-based shopping cart (`ISession`).
  - Add to cart via AJAX without full page reloads, update quantities, remove items, or clear cart.
  - Real-time cart counter badge in navigation header.
- **Order Placement & Tracking**:
  - Multi-step checkout with delivery address and phone number capture.
  - Automatic inventory deduction upon successful checkout.
  - Order confirmation receipt with breakdown.
  - User order history with real-time status badges (`Pending`, `Processing`, `Shipped`, `Delivered`, `Cancelled`).

### Authentication, Security & Real-Time Validation
- **ASP.NET Core Identity**:
  - Password hashing with PBKDF2/HMAC-SHA256 (no plain-text passwords).
  - Role-Based Access Control (`Admin` and `User` roles).
  - Secure authentication cookie with sliding expiration and CSRF anti-forgery tokens (`[ValidateAntiForgeryToken]`).
- **Live Password Strength Meter**:
  - Evaluates password length, uppercase, lowercase, numbers, and symbols in real-time as the user types.
  - Visual color-coded progress bar with Weak / Medium / Strong feedback.
- **Real-Time Username & Email Availability**:
  - Debounced AJAX queries to `/Account/CheckUsername` and `/Account/CheckEmail`.
  - Immediate visual feedback (green checkmark or red error) before form submission.
- **Password Reset**:
  - Identity security token generation and password reset flow.

### Administrator Console (`/Admin`)
- **Restricted Access**: Enforced via `[Area("Admin")]` and `[Authorize(Roles = "Admin")]`.
- **Analytics Dashboard**:
  - Stat cards: Total Users, Total Products, Total Orders, Total Revenue, and Total Reviews.
  - **4 Chart.js Visualizations** populated dynamically from PostgreSQL EF Core queries:
    1. **Sales Over Time**: 6-month monthly revenue line chart.
    2. **Orders by Status**: Interactive doughnut chart.
    3. **Top-Selling Products**: Bar chart displaying units sold per item.
    4. **Products by Category**: Categorical distribution pie chart.
- **Catalog Management (CRUD)**:
  - Create, read, update, and delete categories with product association protection.
  - Create, read, update, and delete products with image uploads.
  - **Multi-Image Upload & Preview**: Upload main cover image plus multiple gallery images (`.jpg`, `.png`, `.webp`) with client-side preview and server-side MIME/size validation.
  - Manage existing gallery images with individual deletion.
- **Order Management**:
  - Filter orders by status and view complete item receipts and customer delivery details.
  - Change status (`Pending` &rarr; `Processing` &rarr; `Shipped` &rarr; `Delivered` &rarr; `Cancelled`).
- **User & Review Moderation**:
  - View registered customer accounts and roles.
  - Moderate or remove inappropriate product reviews.

### Google Gemini AI Chatbot
- **Architecture**: Razor View &rarr; JavaScript `fetch()` &rarr; `AIController` &rarr; `GeminiService` &rarr; Google Gemini API.
- Floating widget with auto-scroll chat history, quick recommendation chips, typing indicator, and markdown formatting.
- Secure server-side API key retrieval (never exposed in frontend code).
- Store-grounded system instructions with simulated fallback for offline/demo use.

---

## 3. Technology Stack

- **Backend Framework**: ASP.NET Core MVC (Targeting .NET 10 / 9 / 8)
- **Language**: C# 13
- **ORM**: Entity Framework Core (Code First)
- **Database**: PostgreSQL (Npgsql Entity Framework Core Provider)
- **Authentication**: ASP.NET Core Identity
- **Frontend**: Razor Views (`.cshtml`), HTML5, CSS3, JavaScript (ES6+), Bootstrap 5, Bootstrap Icons
- **Data Visualization**: Chart.js
- **Artificial Intelligence**: Google Gemini REST API
- **IDE**: Microsoft Visual Studio 2022+ / Visual Studio Code / dotnet CLI

---

## 4. MVC Architecture & Execution Flow

```text
Browser (Client)
   │
   │ HTTP GET / POST (Form, AJAX, JSON)
   ▼
Razor View (.cshtml) + JavaScript (site.js, chatbot.js, availability-check.js)
   │
   ▼
MVC Controller (ProductsController, AccountController, AIController, Admin Areas)
   │
   ▼
Service Layer (IFileService, ICartService, IGeminiService)
   │
   ▼
Entity Framework Core (ApplicationDbContext)
   │
   ▼
PostgreSQL Database: ggcliks
```

---

## 5. Directory Structure

```text
ggChick/
│
├── Areas/
│   └── Admin/
│       ├── Controllers/
│       │   ├── DashboardController.cs     # Real-time metrics & Chart.js API
│       │   ├── ProductsController.cs      # Product CRUD + image uploads
│       │   ├── CategoriesController.cs    # Category CRUD
│       │   ├── OrdersController.cs        # Order status management
│       │   ├── UsersController.cs         # Registered user lists
│       │   └── ReviewsController.cs       # Review moderation
│       └── Views/
│           ├── Shared/_AdminLayout.cshtml # Admin navbar & Chart.js setup
│           ├── Dashboard/Index.cshtml     # Charts & metric widgets
│           ├── Products/                  # Index, Create, Edit, Delete
│           ├── Categories/                # Index, Create, Edit, Delete
│           ├── Orders/                    # Index, Details
│           ├── Users/Index.cshtml
│           └── Reviews/Index.cshtml
│
├── Controllers/
│   ├── HomeController.cs                  # Hero, featured products, reviews
│   ├── AccountController.cs               # Auth, live checks, password reset
│   ├── ProductsController.cs              # Search, filter, sort, details
│   ├── CartController.cs                  # Session shopping cart
│   ├── OrdersController.cs                # Checkout & tracking
│   ├── ReviewsController.cs               # 5-star ratings & reviews
│   ├── CommentsController.cs              # Product discussion Q&A
│   └── AIController.cs                    # Gemini AI chat endpoint
│
├── Data/
│   ├── ApplicationDbContext.cs            # EF Core DbContext & Fluent API
│   └── DbInitializer.cs                   # Seed data (Roles, Users, Catalog)
│
├── Migrations/
│   └── 20260923200700_InitialCreate.cs    # EF Core PostgreSQL Migration
│
├── Models/
│   ├── ApplicationUser.cs                 # Extended Identity User
│   ├── Category.cs                        # Category model
│   ├── Product.cs                         # Product with ratings & stock
│   ├── ProductImage.cs                    # Multiple gallery images
│   ├── Review.cs                          # 5-star review entity
│   ├── Comment.cs                         # Discussion entity
│   ├── Order.cs                           # Customer order entity
│   ├── OrderItem.cs                       # Individual order items
│   └── Enums/OrderStatus.cs              # OrderStatus enum
│
├── Services/
│   ├── IFileService.cs & FileService.cs   # File upload security & storage
│   ├── ICartService.cs & CartService.cs   # Session cart management
│   └── IGeminiService.cs & GeminiService.cs # Google Gemini AI integration
│
├── ViewModels/
│   ├── Account/                           # Register, Login, Reset ViewModels
│   ├── Products/                          # ProductList, Details, Filter ViewModels
│   ├── Cart/                              # CartItem, CartViewModel
│   ├── Orders/                            # Checkout, OrderList ViewModels
│   └── Admin/                             # DashboardStats, ChartData ViewModels
│
├── Views/
│   ├── Home/ (Index, Privacy)
│   ├── Account/ (Login, Register, ForgotPassword, ResetPassword, AccessDenied)
│   ├── Products/ (Index, Details)
│   ├── Cart/ (Index)
│   ├── Orders/ (Checkout, Confirmation, Index, Details)
│   └── Shared/
│       ├── _Layout.cshtml                 # Global navbar, dark mode, footer
│       ├── _ChatbotPartial.cshtml         # Floating Gemini AI widget
│       └── _ValidationScriptsPartial.cshtml
│
├── wwwroot/
│   ├── css/ (site.css, admin.css, chatbot.css)
│   ├── js/ (site.js, password-strength.js, availability-check.js, chatbot.js, admin-charts.js)
│   ├── images/ (products SVG vector graphics & placeholders)
│   └── uploads/
│       └── products/                      # Uploaded product files
│
├── Program.cs                             # Services DI & middleware pipeline
├── appsettings.json                       # PostgreSQL & Gemini settings
└── ggChick.slnx / ggChick.csproj          # Visual Studio Solution & Project
```

---

## 6. PostgreSQL Database Setup

1. Open **pgAdmin** or PostgreSQL CLI (`psql`).
2. Create a new database named **`ggcliks`**:
   ```sql
   CREATE DATABASE ggcliks;
   ```
3. Verify that your PostgreSQL service is running on `localhost:5432`.

---

## 7. Connection String Configuration

Open [`appsettings.json`](file:///c:/Users/munta/source/repos/ggChick/ggChick/appsettings.json) and replace `YOUR_PASSWORD` with your local PostgreSQL password:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=ggcliks;Username=postgres;Password=YOUR_PASSWORD"
  },
  "Gemini": {
    "ApiKey": "YOUR_GEMINI_API_KEY",
    "Model": "gemini-1.5-flash"
  }
}
```

> **Note**: If `YOUR_PASSWORD` is left as default, the application will boot with graceful fallback logging without crashing, allowing you to update your password at any time.

---

## 8. EF Core Migrations

The Initial Migration (`InitialCreate`) has already been generated.

To apply migrations and create tables in PostgreSQL:

### Using Visual Studio Package Manager Console:
```powershell
Update-Database
```

### Using .NET CLI (Terminal):
```bash
dotnet ef database update --project ggChick/ggChick.csproj
```

Upon successful connection, `DbInitializer` will automatically seed:
- Roles (`Admin`, `User`)
- Demo Administrator account
- Demo Customer account
- 4 Default Categories (Electronics, Fashion, Accessories, Home & Living)
- 8 Seeded Products with descriptions, ratings, and gallery images
- Sample reviews and initial orders for the dashboard charts

---

## 9. How to Open and Run in Visual Studio

1. Open **Microsoft Visual Studio 2022** (or newer).
2. Select **Open a project or solution**.
3. Select `c:\Users\munta\source\repos\ggChick\ggChick.slnx`.
4. Ensure the startup project is set to **`ggChick`**.
5. Build the solution using **Build &rarr; Build Solution** (`Ctrl + Shift + B`).
6. Press **`F5`** (Debug) or **`Ctrl + F5`** (Without Debugging).
7. Visual Studio will launch your default web browser to `https://localhost:7198` or `http://localhost:5247`.

---

## 10. Google Gemini API Setup

To enable real Gemini AI responses:
1. Obtain a free API key from [Google AI Studio](https://aistudio.google.com/).
2. Add your key to `appsettings.json`:
   ```json
   "Gemini": {
     "ApiKey": "AIzaSyYourActualKeyHere...",
     "Model": "gemini-1.5-flash"
   }
   ```
   *Or use Visual Studio User Secrets:*
   ```bash
   dotnet user-secrets set "Gemini:ApiKey" "AIzaSy..." --project ggChick/ggChick.csproj
   ```
3. If no key is supplied or the default placeholder is detected, the built-in intelligent fallback assistant provides instant answers regarding store products and policies.

---

## 11. Demo Login Credentials

| Role | Username / Email | Password | Access Level |
| :--- | :--- | :--- | :--- |
| **System Admin** | `admin@ggcliks.com` *(or `admin`)* | `Admin@123` | Full Admin Console, Chart.js, Category & Product CRUD, Order Status, Moderation |
| **Standard User** | `user@ggcliks.com` *(or `customer`)* | `User@123` | Storefront browsing, Cart, Checkout, Order Tracking, 5-Star Reviews, Comments |

---

## 12. Common Errors and Solutions

### 1. `28P01: password authentication failed for user "postgres"`
- **Cause**: The password in `appsettings.json` does not match your PostgreSQL installation password.
- **Solution**: Open `appsettings.json` and change `Password=YOUR_PASSWORD` to your actual PostgreSQL password.

### 2. `3D000: database "ggcliks" does not exist`
- **Cause**: The PostgreSQL database has not been created yet.
- **Solution**: In pgAdmin or psql, run `CREATE DATABASE ggcliks;`, then run `Update-Database` in Visual Studio Package Manager Console.

### 3. Image uploads fail with 5MB error
- **Cause**: Uploaded file exceeds the 5MB safety limit.
- **Solution**: Upload images smaller than 5MB in `.jpg`, `.png`, or `.webp` format.

---

## 13. University Viva / Presentation Talking Points

When presenting this project:
1. **Architecture**: Explain the Model-View-Controller pattern separation. Emphasize that business logic is encapsulated in `Services/` (`FileService`, `CartService`, `GeminiService`) and the database layer uses `Entity Framework Core` with `ApplicationDbContext`.
2. **Security**: Mention that passwords are never stored in plain text and are secured by ASP.NET Core Identity. Highlight Anti-Forgery tokens (`@Html.AntiForgeryToken()`) protecting all state-altering POST requests.
3. **Real-time UX**: Highlight the debounced AJAX availability checking and the dynamic password strength calculator implemented in Vanilla JavaScript.
4. **Data Analytics**: Showcase the Admin Dashboard and explain how the 4 Chart.js charts pull real aggregated data from PostgreSQL using LINQ queries.
5. **AI Integration**: Explain the server-to-server proxy pattern where API keys remain protected on the backend while serving real-time customer queries through the floating assistant.
