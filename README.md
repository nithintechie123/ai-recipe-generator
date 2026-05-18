# AI Recipe Generator

AI Recipe Generator is a full-stack recipe app that turns food photos and ingredient lists into recipe ideas. Upload an image, let the backend analyze the ingredients, generate a recipe, and save favorites for later.

## Features

- Upload a food or ingredient photo
- Detect ingredients with AI
- Add or remove ingredients manually
- Choose dietary preferences
- Generate full recipes with ingredients, instructions, nutrition, and serving ideas
- Get multiple recipe suggestions
- Save, search, filter, view, and delete saved recipes

## Tech Stack

- Frontend: React, Vite, React Router, Axios
- Backend: Node.js, Express, MongoDB, Mongoose, Multer
- AI: Groq SDK

## System Architecture

```text
User
  |
  v
React + Vite Client
  |
  |  HTTP requests through /api proxy
  v
Express Server
  |
  +-- Multer handles image uploads
  |
  +-- Groq SDK generates ingredient analysis and recipes
  |
  +-- Mongoose reads and writes saved recipes
  v
MongoDB
```

### Application Flow

1. The user uploads a food image or manages ingredients in the React client.
2. The client sends requests to the Express API through the Vite `/api` proxy.
3. Image uploads are processed by Multer before reaching the recipe controller.
4. The recipe controller calls the Groq API to analyze ingredients or generate recipe content.
5. Generated recipes are returned to the client for display.
6. When a user saves a recipe, the server stores it in MongoDB through Mongoose.
7. Saved recipes can be fetched, filtered, opened by ID, or deleted from the client.

### Main Responsibilities

- `client/src/pages` contains route-level screens such as Home, Recipe Result, Saved Recipes, and Recipe Detail.
- `client/src/components` contains reusable UI pieces such as the uploader, filters, loader, recipe cards, and recipe display.
- `client/src/context/RecipeContext.jsx` centralizes recipe state and API calls for the frontend.
- `server/routes/recipeRoutes.js` defines API endpoints.
- `server/controllers/recipeController.js` handles recipe generation, image analysis, saving, fetching, and deleting recipes.
- `server/models/Recipe.js` defines the saved recipe schema.
- `server/config/db.js` connects the backend to MongoDB.
- `server/config/gemini.js` configures the Groq SDK client.

## Project Structure

```text
ai-recipe-generator/
  client/   React + Vite frontend
  server/   Express API and MongoDB models
```

## Getting Started

### 1. Install dependencies

```bash
cd client
npm install

cd ../server
npm install
```

### 2. Configure environment variables

Create `server/.env`:

```env
PORT=5000
MONGODB_URI=your_mongodb_connection_string
GROQ_API_KEY=your_groq_api_key
```

### 3. Run the backend

```bash
cd server
npm run dev
```

The API runs at `http://localhost:5000`.

### 4. Run the frontend

Open a second terminal:

```bash
cd client
npm run dev
```

The app runs at `http://localhost:5173`.

## Available Scripts

### Client

```bash
npm run dev      # Start Vite dev server
npm run build    # Build production frontend
npm run preview  # Preview production build
npm run lint     # Run ESLint
```

### Server

```bash
npm run dev      # Start server with nodemon
npm start        # Start server with node
```

## API Routes

Base path: `/api/recipes`

- `POST /analyze` - Analyze uploaded image
- `POST /generate` - Generate one recipe
- `POST /suggestions` - Generate recipe suggestions
- `POST /save` - Save a recipe
- `GET /saved` - Get saved recipes
- `GET /saved/:id` - Get one saved recipe
- `DELETE /saved/:id` - Delete a saved recipe

Health check:

- `GET /api/health`

## Notes

- The frontend dev server proxies `/api` requests to `http://localhost:5000`.
- Keep `.env` files out of git. Add `.env.example` if you want to share required variable names safely.
