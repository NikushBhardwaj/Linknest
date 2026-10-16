# LinkNest

LinkNest is a personal link-sharing app built with React, Express, and MongoDB. Create a public profile, organize links, and share them from one page.


## Features

- Sign up with email OTP verification, sign in, and reset a forgotten password
- Create a public profile at `/<username>` and update its name, username, bio text, and email
- Add, edit, delete, and drag to reorder up to 50 profile links
- Upload and crop a profile image and upload a PDF resume (maximum 2 MB per file)
- Preview and share public profiles
- Delete an account
- Responsive interface with profile-loading skeletons

## Tech stack

- **Frontend:** React 19, Vite, React Router, Tailwind CSS, Context API, `@dnd-kit`, `react-easy-crop`, `react-pdf`, Lucide React, Typed.js
- **Backend:** Node.js, Express 5, MongoDB with Mongoose, JWT, bcryptjs, Nodemailer, Multer

## Project structure

```text
linknest/
├── backend/
│   ├── src/
│   │   ├── config/        # MongoDB and email configuration
│   │   ├── controllers/   # Authentication and profile logic
│   │   ├── middlewares/   # Authentication and file-upload middleware
│   │   ├── models/        # User and OTP models
│   │   ├── routes/        # Authentication and user API routes
│   │   ├── utils/
│   │   └── server.js
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── context/
│   │   ├── pages/
│   │   ├── styles/
│   │   └── utils/
│   └── package.json
└── README.md
```

## Requirements

- Node.js and npm
- A MongoDB connection string
- SMTP credentials for sending signup and password-reset OTP emails

## Run locally

Clone the repository and install the dependencies in both applications:

```bash
git clone https://github.com/NikushBhardwaj/Linknest.git
cd Linknest
```

In one terminal:

```bash
cd backend
npm install
```

Create `backend/.env`:

```dotenv
PORT=5000
MONGODB_URI=mongodb+srv://<username>:<password>@<cluster>/<database>
JWT_SECRET=<a-long-random-secret>
EMAIL_HOST=<smtp-host>
EMAIL_PORT=<smtp-port>
EMAIL_USER=<smtp-username>
EMAIL_PASS=<smtp-password>
EMAIL_FROM=<sender-email-address>
```

Start the API:

```bash
npm run dev
```

In a second terminal, install and start the frontend:

```bash
cd frontend
npm install
npm run dev
```

The frontend uses `http://localhost:5000` as its API by default. To use a different API URL, create `frontend/.env.local` and set:

```dotenv
VITE_API_BASE_URL=http://localhost:5000
```

Open the local URL printed by Vite. The frontend and backend are separate applications and should be run in separate terminals.

## API overview

The backend defaults to port `5000`.

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `POST` | `/api/auth/register` | Request a signup OTP |
| `POST` | `/api/auth/verify-signup-otp` | Verify OTP and create an account |
| `POST` | `/api/auth/login` | Sign in |
| `POST` | `/api/auth/forgot-password` | Request a password-reset OTP |
| `POST` | `/api/auth/verify-reset-otp` | Verify a password-reset OTP |
| `POST` | `/api/auth/reset-password` | Set a new password |
| `GET` | `/api/user/u/:username` | Read a public profile |
| `GET` | `/api/user/profile` | Read the signed-in user's profile |
| `PUT` | `/api/user/profile` | Update profile details and files |
| `PUT` | `/api/user/links` | Update profile links |
| `DELETE` | `/api/user/profile` | Delete the signed-in user's account |

Profile-management endpoints require a JWT bearer token.
