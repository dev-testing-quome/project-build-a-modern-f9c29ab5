# Developer Setup Guide - project-build-a-modern

## Prerequisites

This guide assumes a basic understanding of software development concepts.

**Required Software Versions:**

* **Docker:**  Version 20.10.0 or higher (recommended for Option 1)
* **Docker Compose:** Version 1.29.0 or higher (recommended for Option 1)
* **Node.js:** Version 16 or higher (required for both options)
* **npm (or yarn):**  Package manager for Node.js (required for both options)
* **Python:** Version 3.9 or higher (required for Option 2)
* **PostgreSQL:** Version 13 or higher (recommended, can be managed by Docker)


**Development Tools:**

* Git (for version control)
* Text editor or IDE (see recommendations below)


**IDE Recommendations and Configurations:**

* **VS Code:** Highly recommended due to its excellent extension support for JavaScript, Python, and Docker.  Install extensions for Python, ESLint, Prettier, and Docker.
* **PyCharm (Professional Edition):**  Good for Python backend development.
* **WebStorm:** Good for frontend development.


## Local Development Setup

### Option 1: Docker Development (Recommended)

This option simplifies setup by containerizing the entire application.

1. **Clone Repository:**
   ```bash
   git clone <repository_url>
   cd project-build-a-modern
   ```

2. **Docker Setup Commands:**  Ensure Docker and Docker Compose are installed and running.

3. **Development `docker-compose.yml` Configuration:**  The repository should contain a `docker-compose.yml` file.  This file defines the services (database, backend, frontend).  A sample might look like this:

   ```yaml
   version: "3.9"
   services:
     db:
       image: postgres:13
       environment:
         - POSTGRES_USER=myuser
         - POSTGRES_PASSWORD=mypassword
         - POSTGRES_DB=mydatabase
       ports:
         - "5432:5432"
     backend:
       build: ./backend
       ports:
         - "8000:8000"
       depends_on:
         - db
       environment:
         - DATABASE_URL=postgres://myuser:mypassword@db:5432/mydatabase
         - SECRET_KEY=your_secret_key  # Replace with a strong secret key
     frontend:
       build: ./frontend
       ports:
         - "3000:3000"
       depends_on:
         - backend
   ```

4. **Hot Reload Setup:**  For the frontend (React, Vue, etc.), use a hot reload tool like `nodemon` (for backend) and the appropriate tool for your frontend framework (e.g., webpack dev server).  These tools automatically rebuild and refresh the application when you make code changes.  Configuration instructions will be specific to your frontend framework.  Often, it is handled by the build process defined within the frontend folder.


### Option 2: Native Development

This option requires installing dependencies directly on your system.

1. **Backend Setup:**
   ```bash
   python3 -m venv .venv  # Create a virtual environment
   source .venv/bin/activate  # Activate the virtual environment
   pip install -r requirements.txt  # Install backend dependencies
   ```

2. **Frontend Setup:**
   ```bash
   npm install  # Install frontend dependencies
   ```

3. **Database Setup:**
   Install PostgreSQL locally. Create a database named `mydatabase` (or as specified in your configuration) with a user `myuser` and password `mypassword`.


## Environment Configuration

**Required Environment Variables:**

* `DATABASE_URL`:  The connection string for your database.
* `SECRET_KEY`: A strong, randomly generated secret key for security.
* `STRIPE_SECRET_KEY`: Your Stripe secret key for payment processing.
* `STRIPE_PUBLISHABLE_KEY`: Your Stripe publishable key.
* `EMAIL_HOST`: Your email server host (e.g., smtp.gmail.com).
* `EMAIL_PORT`: Your email server port (e.g., 587).
* `EMAIL_HOST_USER`: Your email address.
* `EMAIL_HOST_PASSWORD`: Your email password.


**Local Development `.env` File Setup:** Create a `.env` file in the root directory of your project and add your environment variables:

```
DATABASE_URL=postgres://myuser:mypassword@localhost:5432/mydatabase
SECRET_KEY=your_very_secret_key
STRIPE_SECRET_KEY=sk_test_YOUR_STRIPE_SECRET_KEY
STRIPE_PUBLISHABLE_KEY=pk_test_YOUR_STRIPE_PUBLISHABLE_KEY
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_HOST_USER=your_email@gmail.com
EMAIL_HOST_PASSWORD=your_email_password
```

**Configuration for Different Environments:**  Use environment variables to manage settings for development, staging, and production.  You might use a `.env.development`, `.env.staging`, and `.env.production` files, or a more sophisticated configuration management system.


## Running the Application

**Start Commands for Development:**

* **Docker:** `docker-compose up -d`
* **Native:**  Start the backend server (e.g., `python manage.py runserver`) and the frontend development server (e.g., `npm start`).

**How to Access Frontend and Backend:** The frontend will typically be accessible at `http://localhost:3000` and the backend API at `http://localhost:8000` (adjust ports as needed based on your `docker-compose.yml` or server configuration).

**API Documentation Access:**  Use tools like Swagger or Postman to explore and test the backend API.  Documentation generation might be integrated into your backend framework (e.g., Django REST framework's browsable API).


## Development Workflow

**Git Workflow and Branching Strategy:** Use a Gitflow or GitHub Flow workflow.  Create feature branches for new features and bug fixes.

**Code Formatting and Linting Setup:** Use tools like Prettier and ESLint (frontend) and Black and Pylint (backend) to enforce consistent code style.

**Testing Procedures:**  Write unit tests for individual components and integration tests to test interactions between components.  Use a testing framework like pytest (Python) and Jest (JavaScript).

**Debugging Setup:** Use your IDE's debugger or command-line debuggers.


## Database Management

**Running Migrations:** Use the database migration tools provided by your ORM (e.g., `python manage.py migrate` for Django).

**Seeding Development Data:** Create scripts to populate the database with sample data for development.

**Database Reset Procedures:**  Develop scripts to easily reset the database to a clean state.


## Testing

**Running Unit Tests:**  Execute your unit tests using the appropriate test runner (e.g., `pytest` or `jest`).

**Running Integration Tests:**  Run integration tests to verify interactions between different parts of the application.

**Test Coverage Reports:** Generate test coverage reports to see how much of your code is covered by tests.


## Common Development Tasks

**Adding New API Endpoints:** Follow the conventions of your backend framework.  Include appropriate tests.

**Adding New Frontend Components:**  Use your frontend framework's component system.  Ensure the components are styled consistently.

**Database Schema Changes:**  Make schema changes using migrations to avoid data loss.

**Adding Dependencies:** Use `pip` (Python) or `npm` (Node.js) to add new dependencies.  Remember to update your `requirements.txt` or `package.json` file.


## Troubleshooting

**Common Setup Issues:** Check that all prerequisites are installed correctly and that your environment variables are set properly.

**Port Conflicts Resolution:**  Change the ports used by your application if there are conflicts.

**Dependency Issues:**  Carefully review error messages and ensure that your dependencies are compatible.

**Environment Variable Problems:** Double-check your `.env` file and ensure that the environment variables are correctly loaded.


## Contributing

**Code Style Guidelines:**  Adhere to the project's code style guidelines (e.g., PEP 8 for Python, Airbnb style guide for JavaScript).

**Pull Request Process:** Create pull requests for code changes.  Include clear descriptions and address any feedback received.

**Issue Reporting:**  Report bugs and feature requests using the project's issue tracker.  Provide clear and concise descriptions.


This guide provides a solid foundation. Specific instructions for your chosen technologies (e.g., React, Django, etc.) will be needed within the project's documentation and codebase. Remember to consult those resources as you work through the project.
