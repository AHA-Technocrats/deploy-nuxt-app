## How to deploy the NuxtJS application freely on the github pages.

1. Create a new NuxtJS application
    - npx nuxi@latest init **project-name**  

2. Test your project running or not
    - Once you thing your project is running then good to go ahead
    - Create a new repository
    - Make sure that repository is public


3. Initialized the project with the latest github repository you created and pushed code.
    - Once code successfully pushed to the repository and if you have any .env variables configure on the github repository
    - Go to settings tag and click
    - In the left side there is a **Secrets and variables** tabs
    - Click there and one menu will appear where there more different tabs are available
        - Actions
        - Codespaces
        - Dependebot
    - Click on **Actions** a new page will appear where you can set .env variables and corresponding value

4. On the project directory create a folder with named called **.github**
    - Inside .github create a subdirectory called **workflows**
    - Inside **workflows** create a file called **build-and-deploy.yml**        
   - Inside **build-and-deploy.yml** paste below code and make changes based on branch and variables you configured on the repository

   ```javascript

        name: Build and Deploy
        on: 
        push:
            branches:
            - master // here write your preference branch name from where your project will be deployed
        permissions:
        contents: write
        jobs:
        build-and-deploy:
            concurrency: ci-${{ github.ref }}
            runs-on: ubuntu-latest
            strategy:
            matrix:
                node-version: [18]
            steps:
            - name: Checkout 🛎️
            uses: actions/checkout@v3

            - name: Use Node.js ${{ matrix.node-version }}
            uses: actions/setup-node@v3
            with:
                node-version: ${{ matrix.node-version }}

            - name: Install & Generate 🔧
            run: |
                npm ci
                npm run generate
            env:
                NUXT_APP_BASE_URL: ${{ vars.NUXT_APP_BASE_URL  }} // variables name you configured on the actions

            - name: Deploy 🚀
            uses: JamesIves/github-pages-deploy-action@v4
            with:
                folder: dist
    ```    
    - Now commit the code again and go to the repository
    - Now if everything ok in the github actions tab you will see your project deploying basically creating **gh-pages**
    - Once that is done go ahead.

5. Once project that **action workflow completed** go to the github repo again and  in the branches you will see **gh-pages** branch created
6. Go to the **settings** tab again and in the left side there is a tab called **pages**. Click on that then a new page will be open
   - There are many things you can see 
   - There is **Branch** text below there is selection option where all branches are listed.
   - from there select **gh-pages** branch and click on **save** button
   - In the **actions** tab you will see **gh-pages** workflow running.
   - Once that is completed you can get link from there.

**Congratulations! Your NuxtJS application is now deployed on GitHub Pages.**
