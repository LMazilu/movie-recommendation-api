# movie-recommendation-api

AI-powered NestJS backend that generates personalized movie/series/cartoon suggestions from user preferences, then enriches results with poster data and stores recommendation history per user.

## What the backend actually does
This API is not only CRUD/auth: it includes a recommendation pipeline driven by AI.

Core flow:
1. The user sends preferences (mood, genres, content type, favorite movie, target years).
2. The backend builds a structured prompt and calls OpenAI Chat Completions (`gpt-4o`).
3. The AI response is parsed into normalized film objects.
4. Each title is enriched via OMDB (poster URL and metadata lookup).
5. Recommendations are saved to the user profile in MongoDB (history capped to latest 20).
6. The frontend receives the generated list + mood result.

## Main capabilities
- JWT authentication (email/password)
- Google login support (Firebase token -> app JWT)
- Forgot/change password endpoints
- Role-based guards (`admin` vs standard user)
- AI topic-based quick recommendations
- AI final personalized recommendations
- User recommendation history retrieval
- Swagger docs and request validation

## Tech stack
- NestJS + TypeScript
- MongoDB + Mongoose
- OpenAI API (recommendation generation)
- OMDB API (movie poster/data enrichment)
- Passport JWT + custom guards/decorators
- Helmet + validation pipe + CORS
- Docker / Docker Compose

## Main API areas
- `POST /auth/register`
- `POST /auth/login`
- `POST /auth/google-login`
- `POST /auth/forgot-password`
- `POST /auth/change-password`
- `POST /api/recommendations` (protected, personalized AI recommendation)
- `POST /api/recommendations/:topic` (topic-based AI recommendation)
- `GET /api/recommendations/:email` (protected, own/admin history)
- `GET /users` / `GET /users/:email` / `PUT /users/:email` (admin-protected)

Swagger:
- `GET /api/docs`

## Startup defaults
On startup, the app auto-creates demo accounts if missing:
- `admin@example.com` / `admin`
- `user@example.com` / `user`

## Environment variables
Copy `.env.example` to `.env` and configure at least:
- `PORT` (optional, default `3030`)
- `MONGO_URI`
- `JWT_SECRET`
- `OPENAI_API_KEY`
- `OMDB_API_KEY`
- `BCRYPT_SALT_ROUNDS`
- `FORGOT_PASSWORD_BASE` (for reset flow)

## Run locally
```bash
npm install
npm run start:dev
```

Backend:
- `http://localhost:3030`
- Swagger: `http://localhost:3030/api/docs`

## Run full stack with Docker
From this backend root:

```bash
npm run docker
```

This compose setup starts:
- backend on `3030`
- frontend (`movie-recommendation-web`) on `3000`
- mongo on `27017`

Stop:
```bash
npm run docker:down
```

## Testing
```bash
npm test
npm run test:e2e
npm run test:cov
```

