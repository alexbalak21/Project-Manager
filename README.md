# Project-Manager
An app to help me manage my project add Ideas & todos

## Project overview

### I want to be able to add new project with github links, live demo links & techstack:
Data model :
-Project name (VARCHAR)
-Project img (VARCHAR or BLOB)
-Description (TEXT) : Idea the project description can come form github the server will fetch the project description form the repo on github.
-Start Date (DATE)
-Project github link (VARCHAR)
-Notes (project_id of notes table, many to one)
-Todos or Tasks (project_id of todos tavle, many to one)
Optional: projetc participants (user_id, one to many)
Need to think how I will implement it.

Optional :
Du date :

Advancement : I can track my tasks achivements trough a github api :
Task complemted.
Task to do.

I can add task category:
Lke todo.
Improvements.
Security.
Frontend
Backend.


### Notes :
Notes can be TEXT writtent a RICH TEXT EDITOR & I want to add an option to add a voice note & convert it to text on the fly with a microphone icon
Notes can become todos

Skill table :
I would have to implement the Skill table with:
-Skill name (VARCHAR)
skill_img (BLOB)



