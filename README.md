# Monorepo Template - React + Node.js

A production-ready monorepo template that combines a React frontend with a Node.js/Express backend in a single repository. This template is optimized for easy deployment to Heroku while maintaining a clean development experience.

Demo: [https://cryptic-atoll-43628-af717b411752.herokuapp.com/](https://cryptic-atoll-43628-af717b411752.herokuapp.com/)

## Features

- **Frontend**: React with TypeScript and Vite for fast development
- **Backend**: Node.js with Express server
- **Monorepo Structure**: Frontend and backend in the same repository
- **Production Ready**: Configured for easy Heroku deployment
- **Development Tools**: Hot-reloading for both frontend and backend
- **API Integration**: Pre-configured CORS and API endpoints

## Project Structure

```
monorepo-template/
├── client/                 # React frontend application
│   ├── src/               # Source code
│   ├── public/            # Static assets
│   ├── package.json       # Frontend dependencies
│   └── vite.config.ts     # Vite configuration
├── server/                 # Node.js backend
│   └── index.js           # Express server entry point
├── package.json           # Root package.json with scripts
├── Procfile               # Heroku process configuration
└── README.md              # This file
```

## Prerequisites

- Node.js 20.x or higher
- npm or yarn package manager
- Git

## Running Locally

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd monorepo-template
```

### 2. Install Dependencies

Install root dependencies:
```bash
npm install
```

Install client dependencies:
```bash
cd client
npm install
cd ..
```

### 3. Start Development Server

From the root directory, run:
```bash
npm run dev
```

This will start:
- Backend server on http://localhost:5000
- Frontend development server on http://localhost:3000

The development script uses `concurrently` to run both servers simultaneously with hot-reloading enabled.

### 4. Test the API

Visit http://localhost:5000/api/hello to test the backend API endpoint.

## Available Scripts

- `npm start` - Starts the production server
- `npm run dev` - Starts both frontend and backend in development mode
- `npm run build` - Builds the frontend for production

## Deploying to Heroku

### Prerequisites

1. Create a Heroku account at https://heroku.com
2. Install the Heroku CLI: https://devcenter.heroku.com/articles/heroku-cli

### Deployment Steps

1. **Login to Heroku**
   ```bash
   heroku login
   ```

2. **Create a New Heroku App**
   ```bash
   heroku create your-app-name
   ```
   Replace `your-app-name` with your desired application name or leave blank to have heroku create one for you.

3. **Set Node.js Version** (Optional)
   The template is configured for Node.js 20.x in package.json. Heroku will automatically detect this.

4. **Deploy to Heroku**
   ```bash
   git add .
   git commit -m "Initial deployment"
   git push heroku main
   ```

5. **Open Your Deployed App**
   ```bash
   heroku open
   ```

### How Heroku Deployment Works

1. **Build Process**: Heroku automatically runs `npm run heroku-postbuild` which:
   - Installs client dependencies
   - Builds the React app for production
   - Creates optimized static files in `client/dist/`

2. **Server Configuration**: The Express server:
   - Serves the built React app from `client/dist/`
   - Handles API routes at `/api/*`
   - Falls back to serving the React app for client-side routing

3. **Procfile**: Tells Heroku to run `npm start` which starts the Express server

### Environment Variables

For production deployments, you can set environment variables on Heroku:

```bash
heroku config:set VARIABLE_NAME=value
```

The server automatically uses `process.env.PORT` provided by Heroku.

## API Endpoints

- `GET /api/hello` - Sample API endpoint returning a JSON message
- `GET /*` - Serves the React application

## Customization

### Adding New API Endpoints

Edit `server/index.js` to add new API routes:

```javascript
app.get('/api/your-endpoint', (req, res) => {
  res.json({ data: 'Your data here' });
});
```

### Modifying the Frontend

The React app is located in the `client/` directory. Edit files in `client/src/` to modify the frontend.

### Adding Environment Variables

1. For local development, create a `.env` file in the root directory
2. For Heroku, use `heroku config:set` as shown above

## Troubleshooting

### Port Already in Use

If you get a "port already in use" error, either:
- Kill the process using the port
- Change the port in `server/index.js` (for backend) or `client/vite.config.ts` (for frontend)

### Heroku Build Failures

- Check the build logs: `heroku logs --tail`
- Ensure all dependencies are in the correct package.json files
- Verify Node.js version compatibility

### Frontend Not Loading in Production

- Ensure `npm run build` completes successfully
- Check that `client/dist/` directory is created
- Verify the static file path in `server/index.js`
