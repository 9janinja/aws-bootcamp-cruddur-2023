# Week 1 — App Containerization

### STEPS
* Ensure your app runs locally first before containering
- Set Environmental Variables for the app.  <br>
    `export FRONTEND_URL = "*"` <br>
    `export BACKEND_URL = "*"`

- Run the app
    python3 -m flask run --host=0.0.0.0 --port=4567

- Include /api/activities/home to the flask app path