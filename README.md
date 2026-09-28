# AI FAQ Assistant API

A REST API built with Node.js, Express.js, MongoDB/Mongoose, JWT, bcrypt and Google's `@google/genai` SDK.

## Architecture

```text
ai-faq-assistant-api/
├── .env
├── .gitignore
├── package.json
├── README.md
└── src/
    ├── app.js
    ├── server.js
    ├── config/
    │   └── db.js
    ├── controllers/
    │   ├── aiController.js
    │   ├── authController.js
    │   └── faqController.js
    ├── middleware/
    │   ├── authMiddleware.js
    │   ├── errorMiddleware.js
    │   └── validationMiddleware.js
    ├── models/
    │   ├── FAQ.js
    │   └── User.js
    ├── routes/
    │   ├── aiRoutes.js
    │   ├── authRoutes.js
    │   └── faqRoutes.js
    ├── services/
    │   └── geminiService.js
    └── utils/
        └── helpers.js
```

## Requirements

- Node.js 20+
- MongoDB local instance or MongoDB Atlas
- Gemini API key

## Setup

1. Install dependencies:

```bash
npm install
```

2. Edit `.env` and set:

```env
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret
GEMINI_API_KEY=your_gemini_api_key
```

3. Start the API:

```bash
npm run dev
```

or

```bash
npm start
```

## API endpoints

### Auth

- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/me` — Bearer token required

### FAQs

- `GET /api/faqs` — Bearer token required
- `GET /api/faqs/:id` — Bearer token required
- `POST /api/faqs` — admin token required
- `PUT /api/faqs/:id` — admin token required
- `DELETE /api/faqs/:id` — admin token required

### AI

- `POST /api/ai/ask` — Bearer token required

Example:

```json
{
  "question": "How do I reset my password?"
}
```

The AI controller searches the FAQ collection first and passes the best matching FAQ records to Gemini as context.

## Making a user an admin

After registering a user, change that user's `role` from `user` to `admin` in MongoDB. The API intentionally does not expose public role assignment during registration.

## Security notes

- Never commit `.env` to Git.
- Use a long random `JWT_SECRET` in production.
- Keep `GEMINI_API_KEY` server-side.
- Restrict CORS to your frontend's origin in production.
