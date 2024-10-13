# CTP Project Starter

> [!CAUTION]
> This repo is actively being updated as of October 13, 2024. You can begin using and cloning the repo once this message is removed.

A full stack web application starter template for building projects with React, Express.js, and Sequelize.js

**Current version:** 3.0.0 (Oct 2023)

## Stack

> Node.js v20 or V22 LTS is recommended

_Backend API_

- express.js (v4.21.1)
- sequelize.js (v6.37.3)
- PostgreSQL (v14 recommended)

_Frontend React client_

- Based on `vite`
  - pre-configured to work with the api
- Bootstrap (v5)
  - added to `/frontend-client/index.html` (_optional_ can be removed)
- React Router (v6)

## Development Setup

Each team member will need to do this on their local machine.

### Ensure you have PostgreSQL installed

- Check if you have PostgreSQL installed
  - ✅ versions 10-14 should work
  - 🚫 version 15 has not been tested
- If you need to install PostgreSQL see the [installing PostgreSQL guides](https://github.com/CUNYTechPrep/guides#postgresql)

### Create a PostgreSQL user and database

The project-starter template expects the following for local development:

- PostgreSQL User/Role
  - name: `ctp_user`
  - password: `ctp_pass`
- PostgreSQL Database
  - name: `ctp_appdb_development`

#### For Windows/pgAdmin users

If you are on Windows and installed **pgAdmin** follow our [pgAdmin guide](https://github.com/CUNYTechPrep/guides/blob/master/pgAdmin-create-user-db.md) to create a user in PostgreSQL named `ctp_user` with the password `ctp_pass` and a database named `ctp_appdb_development`.

#### For Mac/Linux users

Create a user in PostgreSQL named `ctp_user` with the password `ctp_pass`:

> This only needs to be done one time on your machine
> You can create additional users if you want to.

```
createuser -P -s -e ctp_user
```

Create a separate db for this project:

```
createdb -h localhost -U ctp_user ctp_appdb_development
```

> You will create a DB for each project you start based on this repo. For other projects change `ctp_appdb_development` to the new apps database name.

### Running the app locally

For local development you will need two terminals open, one for the api-backend and another for the react-client.

_Clone_ this app, then:

```bash
# backend-api terminal 1
cp .env.example .env
npm install
npm run dev
```

```bash
# frontend-client terminal 2
cd frontend-client
npm install
npm run dev
```

- backend-api will launch at: http://localhost:8080
- frontend-client will launch at: http://localhost:5173

> In production you will only deploy a single app. The React client will build into static files that will be served from the backend.

## Deployment

The following are hosting options that have a free tier for deploying apps based on this project-starter. Each option has it's pro's and con's.

Students can also get education credits for using Heroku through the [GitHub Student Developer Pack](https://education.github.com/pack)

### Hosting on [Render.com](https://render.com/) (_recommended_)

1. Create an account by clicking the **Get Started** button

- It's recommended to Sign up using your **Github** account for easy linking to project repos.
- The **Individual** account type does NOT require a credit card

2. Navigate to the [Dashboard](https://dashboard.render.com/)
3. Create a PostgreSQL Database

- Click the **New +** button at the top of the page
- Select **PostgreSQL** from the drop down menu
- Provide a **Name** for your projects database
- Choose a **Region** closest to you or your users.
- Choose **Instance Type**: Free
- You can leave the optional settings empty
- Click on the **Create Database** button
- Your database will be ready to use in 1-5 minutes.
- Once the database is active, make note of where to get the Connection details, such as "**Internal Database URL**" and "**External Database URL**"

4. Create a Web Service

- Click the **New +** button at the top of the page
- Select **Web Service** from the drop down menu
- Click on the **"Build and deploy from a Git repository"** option and click **Next**
- Connect to your project's repository on Github
- Provide a **Name** for your projects web app
- Choose the same **Region** as you chose for your database (_important for db connectivity_)
- Choose the **Branch** with the code you want to deploy (usually `main`)
- Leave the **Root Directory** empty
- Choose **Runtime**: Node
- Set **Build Command**: `npm install && npm run build`
- Set **Start Command**: `npm start`
- Choose **Instance Type**: `Free`
- Expand the **Advanced** options
- Add **Environment Variables**
  - key: `SESSION_SECRET` = value: click on the **Generate** button
  - key: `DATABASE_URL` = value: copy the "**Internal Database URL**" from your step 3.
  * Do NOT add the `PORT` variable (Render will set this for you)
- Click the "**Create Web Service**" button
- Your application will be live in 1-5 minutes

### Hosting on [Railway.app](https://railway.app/)

1. Create a Starter account using your Github username
   - You get $5 in credit a month for free and do not have to provide a credit card
2. Verify your account by answering Railways questions
3. Create a **"New Project"**
4. Select **"Deploy from Github repo"**
   - follow instruction to link your project repo to railway
5. Click **"Deploy now"**
   - your app will fail, but we will fix it in the next steps
6. Add a PostgreSQL Database to your Railway project
   - click the **"+ New"** button at the top right of the project
   - click **"Database >"**
   - click **"Add PostgreSQL"**
   - to add a PostgreSQL Database to your project
7. Add environment variables if you need any
   - Do not add the `PORT` variable (Railway will set this for you)

Your app will now be live and auto deployed on new commits. If it's not working you may need to restart the app manually in the Railway UI.

### Hosting on Heroku (no longer free)

> NOTE: Heroku is no longer free, but these instructions still work. We recommend getting started with render.com or railway.app

1. Create a Heroku account (_if you don't have one_)
2. Install the [heroku cli](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) (_if you don't already have it_)

- Requires that you have `git` installed

```bash
# login with the cli tool
heroku login
```

#### Create a Heroku project

Next, `cd` into this project directory and create a project:

```bash
# replace `cool-appname` with your preferred app name
heroku create cool-appname

# add a free PostgreSQL database to your heroku project
heroku addons:create heroku-postgresql:hobby-dev
```

> This will make your app accessible at https://cool-appname.herokuapp.com (_if the name is available_).

> You only need to do this once per app

#### Add Environment Variables

Any environment variables your app needs will be available through your heroku project's settings page.

> NOTE: _Heroku calls them **Config Vars**_

- Go to the dashboard page here: https://dashboard.heroku.com/apps
- Click on the Settings tab
- Click `Reveal Config Vars`
- Add any environment variables you have in your `.env` file

#### Deploying the app

Whenever you want to update the deployed app run this command.

```bash
git push heroku main
```

> This command deploys your main branch. You can change that and deploy a different branch such as: `git push heroku development`
