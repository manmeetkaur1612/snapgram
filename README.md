# SnapGram

A full-stack social media application built with **React, TypeScript and Appwrite**.

SnapGram allows users to create accounts, publish image-based posts, interact with content and manage their profiles. I built the project to gain practical experience connecting a typed frontend application with authentication, database and storage services.

**Live demo:** https://social-media-app-six-lemon.vercel.app/

## Features

* User sign-up, sign-in and session management
* Create, edit and delete posts
* Image upload and storage
* User profiles and profile editing
* Like and unlike posts
* Save and unsave posts
* Search posts by caption
* View posts by user
* Infinite post loading
* Form validation
* Responsive interface

## Tech Stack

### Frontend

* React
* TypeScript
* Vite
* React Router
* Tailwind CSS

### Data & State

* TanStack React Query
* React Context
* Zod
* React Hook Form

### Backend Services

* Appwrite Authentication
* Appwrite Database
* Appwrite Storage

### Deployment

* Vercel

## Architecture

The application separates UI, server-state management and backend access into different layers.

```text
React UI
   │
   ▼
Components / Pages
   │
   ▼
React Query Hooks
   │
   ▼
Appwrite API Layer
   │
   ├── Authentication
   ├── Database
   └── File Storage
```

The Appwrite integration is kept in a dedicated API layer instead of calling backend services directly from UI components. React Query is used for asynchronous data fetching and mutations, while Zod is used for form validation.

## Some Engineering Decisions

### Cleaning up failed file operations

Creating a post requires more than one backend operation:

1. upload the image
2. obtain its preview URL
3. create the database record

If the database operation fails after the image has already been uploaded, leaving the uploaded file behind would create unused storage data.

The implementation therefore attempts to delete the uploaded image when a later step fails.

A similar approach is used when replacing images during post and profile updates: the old image is deleted only after the new database update succeeds.

### Separating backend operations

Authentication, post, storage and user operations are implemented in a dedicated Appwrite API module.

This keeps backend-specific logic away from React components and makes the data flow easier to understand and modify.

### Client-side validation

Forms use **Zod** together with **React Hook Form** so invalid input can be rejected before a backend request is made.

## Project Structure

```text
src/
├── _auth/          # Authentication pages
├── _root/          # Main application pages
├── components/     # Reusable UI components
├── constants/
├── context/        # Application context
├── hooks/
├── lib/
│   ├── appwrite/   # Backend/API integration
│   ├── react-query/
│   └── validation/
├── types/
├── App.tsx
└── main.tsx
```

## Running Locally

### 1. Clone the repository

```bash
git clone https://github.com/manmeetkaur1612/snapgram.git
cd snapgram
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure Appwrite

Create a `.env.local` file and provide the Appwrite configuration used by the application:

```env
VITE_APPWRITE_URL=
VITE_APPWRITE_PROJECT_ID=
VITE_APPWRITE_DATABASE_ID=
VITE_APPWRITE_STORAGE_ID=
VITE_APPWRITE_USER_COLLECTION_ID=
VITE_APPWRITE_POST_COLLECTION_ID=
VITE_APPWRITE_SAVES_COLLECTION_ID=
```

An Appwrite project with the corresponding database collections and storage bucket is required.

### 4. Start the development server

```bash
npm run dev
```

## What I Would Improve Next

Looking back at the project, there are several areas I would improve in a production version.

### 1. Consistent error handling

Several backend functions currently catch errors, log them and return `undefined`.

I would replace this with a consistent error strategy so callers can distinguish between expected failures and unexpected system errors and display useful feedback to users.

### 2. Stronger typing

Some query construction currently uses broad types such as `any[]`.

I would replace these with stricter TypeScript types so more mistakes are caught at compile time.

### 3. Automated testing

I would add tests around the most important workflows, particularly:

* authentication
* post creation
* failed uploads
* post updates
* save/unsave behavior
* data-fetching edge cases

### 4. Better observability

For a production application I would replace `console.log`-based error reporting with structured logging and monitoring so failures could be diagnosed from useful context.

### 5. More explicit failure handling for multi-step operations

Some actions involve both storage and database changes.

The current implementation includes manual cleanup logic, but I would make these workflows more explicit and systematically test partial-failure scenarios.

## What I Learned

The most useful part of building SnapGram was not an individual UI feature, but understanding how multiple application layers interact.

A feature can appear correct in isolation while the complete workflow still fails because of authentication state, asynchronous data fetching, storage operations or inconsistent backend state.

The project gave me practical experience debugging those interactions rather than treating the frontend and backend as independent pieces.

## Author

**Manmeet Kaur**

M.Sc. Computer Science — TU Darmstadt

GitHub: https://github.com/manmeetkaur1612
