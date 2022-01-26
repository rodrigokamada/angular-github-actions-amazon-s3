# Angular GitHub Actions Amazon S3


Application example built with [Angular](https://angular.io/) 13 and hosted on [Amazon S3 (Simple Storage Service)](https://aws.amazon.com/s3/) using [GitHub Actions](https://github.com/actions).

This tutorial was posted on my [blog](https://rodrigo.kamada.com.br/blog/hospedando-uma-aplicacao-angular-no-amazon-s3-usando-o-github-actions) in portuguese and on the [DEV Community](https://dev.to/rodrigokamada/hosting-an-angular-application-on-amazon-s3-using-github-actions-3h6g) in english.


[![Website](https://shields.braskam.com/v1/shields?name=website&format=rectangle&size=small&radius=5)](https://rodrigo.kamada.com.br)
[![LinkedIn](https://shields.braskam.com/v1/shields?name=linkedin&format=rectangle&size=small&radius=5)](https://www.linkedin.com/in/rodrigokamada)
[![Twitter](https://shields.braskam.com/v1/shields?name=twitter&format=rectangle&size=small&radius=5&socialAccount=rodrigokamada)](https://twitter.com/rodrigokamada)



## Prerequisites


Before you start, you need to install and configure the tools:

* [git](https://git-scm.com/)
* [Node.js and npm](https://nodejs.org/)
* [Angular CLI](https://angular.io/cli)
* IDE (e.g. [Visual Studio Code](https://code.visualstudio.com/))



## Getting started


### Create and configure the account on the Amazon S3

**1.** Let's create and configure the account. Access the site [https://aws.amazon.com/s3/](https://aws.amazon.com/s3/) and click on the button *Get Started with Amazon S3*.

![Amazon S3 - Home page](https://res.cloudinary.com/rodrigokamada/image/upload/v1642851691/Blog/angular-github-actions-amazon-s3/amazon-s3-step1.png)

**2.** Click on the option *Root user*, fill in the field *Root user email address* and click on the button *Next*.

![Amazon S3 - Sign in](https://res.cloudinary.com/rodrigokamada/image/upload/v1642852101/Blog/angular-github-actions-amazon-s3/amazon-s3-step2.png)

Note:

* If you don't have an Amazon account, Do steps 1 to 9 of the post *[Authentication using the Amazon Cognito to an Angular application](https://github.com/rodrigokamada/angular-amazon-cognito)* in the session *Create and configure the account on the Amazon Cognito*.

**3.** Fill in the field *Security check* and click on the button *Submit*.

![Amazon S3 - Security check](https://res.cloudinary.com/rodrigokamada/image/upload/v1642852700/Blog/angular-github-actions-amazon-s3/amazon-s3-step3.png)

**4.** Fill in the field *Password* and click on the button *Sign in*.

![Amazon S3 - Root user sign in](https://res.cloudinary.com/rodrigokamada/image/upload/v1642852700/Blog/angular-github-actions-amazon-s3/amazon-s3-step4.png)

**5.** Click on the button *Create bucket*.

![Amazon S3 - Buckets](https://res.cloudinary.com/rodrigokamada/image/upload/v1642854836/Blog/angular-github-actions-amazon-s3/amazon-s3-step5.png)

**6.** Fill in the field *Bucket name*, click on the option *Block all public access* to uncheck this option, *I acknowledge that the current settings might result in this bucket and the objects within becoming public.* and click on the button *Create bucket*.

![Amazon S3 - Create bucket](https://res.cloudinary.com/rodrigokamada/image/upload/v1642869510/Blog/angular-github-actions-amazon-s3/amazon-s3-step6.png)

**7.** Click on the link *angular-github-actions-amazon-s3* with the bucket name.

![Amazon S3 - Buckets](https://res.cloudinary.com/rodrigokamada/image/upload/v1642866388/Blog/angular-github-actions-amazon-s3/amazon-s3-step7.png)

**8.** Click on the link *Properties*.

![Amazon S3 - Bucket objects](https://res.cloudinary.com/rodrigokamada/image/upload/v1642866647/Blog/angular-github-actions-amazon-s3/amazon-s3-step8.png)

**9.** Click on the button *Edit*.

![Amazon S3 - Bucket properties](https://res.cloudinary.com/rodrigokamada/image/upload/v1642867044/Blog/angular-github-actions-amazon-s3/amazon-s3-step9.png)

**10.** Click on the options *Enable*, *Host a static website*, fill in the fields *Index document*, *Error document - optional* and click on the button *Save changes*.

![Amazon S3 - Edit static website hosting](https://res.cloudinary.com/rodrigokamada/image/upload/v1642867461/Blog/angular-github-actions-amazon-s3/amazon-s3-step10.png)

**11.** Copy the URL and the region displayed, in my case, the URL `http://angular-github-actions-amazon-s3.s3-website-sa-east-1.amazonaws.com` and the region `sa-east-1` were displayed because the URL will used to access the Angular application and the region will used in the GitHub Actions file setting and click on the link *Permissions*.

![Amazon S3 - Bucket properties](https://res.cloudinary.com/rodrigokamada/image/upload/v1642901966/Blog/angular-github-actions-amazon-s3/amazon-s3-step11.png)

**12.** Click on the button *Edit*.

![Amazon S3 - Bucket permissions](https://res.cloudinary.com/rodrigokamada/image/upload/v1642873124/Blog/angular-github-actions-amazon-s3/amazon-s3-step12.png)

**13.** Fill in the fields *Policy* with the content below and click on the button *Save changes*.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Principal": "*",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::angular-github-actions-amazon-s3/*"
      ]
    }
  ]
}
```

![Amazon S3 - Bucket policy](https://res.cloudinary.com/rodrigokamada/image/upload/v1642872376/Blog/angular-github-actions-amazon-s3/amazon-s3-step13.png)

**14.** Ready! Account created and configured and bucket created and configured.

![Amazon S3 - Bucket permissions](https://res.cloudinary.com/rodrigokamada/image/upload/v1642873644/Blog/angular-github-actions-amazon-s3/amazon-s3-step14.png)

**15.** Click on the menu *Services*, *Security, Identity & Compliance* and *IAM*.

![Amazon S3 - Menu IAM](https://res.cloudinary.com/rodrigokamada/image/upload/v1642874455/Blog/angular-github-actions-amazon-s3/amazon-s3-step15.png)

**16.** Click on the link *Users*.

![Amazon S3 - IAM dashboard](https://res.cloudinary.com/rodrigokamada/image/upload/v1642875468/Blog/angular-github-actions-amazon-s3/amazon-s3-step16.png)

**17.** Click on the button *Add users*.

![Amazon S3 - Users](https://res.cloudinary.com/rodrigokamada/image/upload/v1642875822/Blog/angular-github-actions-amazon-s3/amazon-s3-step17.png)

**18.** Fill in the field *User name*, click on the option *Access key - Programmatic access* and click on the button *Next: Permissions*.

![Amazon S3 - Set user details](https://res.cloudinary.com/rodrigokamada/image/upload/v1642876438/Blog/angular-github-actions-amazon-s3/amazon-s3-step18.png)

**19.** Click on the options *Attach existing policies directly*, *AmazonS3FullAccess* and click on the button *Next: Tags*.

![Amazon S3 - Set premissions](https://res.cloudinary.com/rodrigokamada/image/upload/v1642890482/Blog/angular-github-actions-amazon-s3/amazon-s3-step19.png)

**20.** Click on the button *Next: Review*.

![Amazon S3 - Set tags](https://res.cloudinary.com/rodrigokamada/image/upload/v1642890751/Blog/angular-github-actions-amazon-s3/amazon-s3-step20.png)

**21.** Click on the button *Create user*.

![Amazon S3 - Review](https://res.cloudinary.com/rodrigokamada/image/upload/v1642890930/Blog/angular-github-actions-amazon-s3/amazon-s3-step21.png)

**22.** Copy the *Access key ID* displayed, in my case, the *Access key ID* `AKIAUAM34QRRRQ5AZD2A` was displayed, click on the link *Show*, copy the *Secret Access key* displayed because the *Access key ID* and *Secret Access key* will be configured in the GitHub repository and click on the button *Close*.

![Amazon S3 - Add user success](https://res.cloudinary.com/rodrigokamada/image/upload/v1642892908/Blog/angular-github-actions-amazon-s3/amazon-s3-step22.png)

**23.** Ready! Access keys created.



### Create the account and the repository on the GitHub


**1.** Let's create the account and the repository. Do steps 1 to 6 of the post *[Hosting an Angular application on GitHub Pages using GitHub Actions](https://github.com/rodrigokamada/angular-github-actions)* in the session *Create and configure the account on the GitHub*.

**2.** Click on the menu *Settings*.

![GitHub - Code](https://res.cloudinary.com/rodrigokamada/image/upload/v1642897425/Blog/angular-github-actions-amazon-s3/github-step2.png)

**3.** Click on the side menu *Secrets*.

![GitHub - Settings](https://res.cloudinary.com/rodrigokamada/image/upload/v1642897653/Blog/angular-github-actions-amazon-s3/github-step3.png)

**4.** Click on the button *New repository secret*.

![GitHub - Secrets](https://res.cloudinary.com/rodrigokamada/image/upload/v1642897851/Blog/angular-github-actions-amazon-s3/github-step4.png)

**5.** Fill in the fields *Name*, *Value* and click on the button *Add secret* to configure the key with the *Access key ID*.

![GitHub - Add access key ID](https://res.cloudinary.com/rodrigokamada/image/upload/v1642898255/Blog/angular-github-actions-amazon-s3/github-step5.png)

**6.** Click on the button *New repository secret*.

![GitHub - Secrets](https://res.cloudinary.com/rodrigokamada/image/upload/v1642899086/Blog/angular-github-actions-amazon-s3/github-step6.png)

**7.** Fill in the fields *Name*, *Value* and click on the button *Add secret* to configure the key with the *Secret Access key*.

![GitHub - Add secret access key](https://res.cloudinary.com/rodrigokamada/image/upload/v1642899576/Blog/angular-github-actions-amazon-s3/github-step7.png)

**8.** Ready! Access keys configured.

![GitHub - Secrets configured](https://res.cloudinary.com/rodrigokamada/image/upload/v1642899804/Blog/angular-github-actions-amazon-s3/github-step8.png)



### Create the Angular application

**1.** Let's create the application with the Angular base structure using the `@angular/cli` with the route file and the SCSS style format.

```powershell
ng new angular-github-actions-amazon-s3 --routing true --style scss
CREATE angular-github-actions-amazon-s3/README.md (1074 bytes)
CREATE angular-github-actions-amazon-s3/.editorconfig (274 bytes)
CREATE angular-github-actions-amazon-s3/.gitignore (548 bytes)
CREATE angular-github-actions-amazon-s3/angular.json (3363 bytes)
CREATE angular-github-actions-amazon-s3/package.json (1096 bytes)
CREATE angular-github-actions-amazon-s3/tsconfig.json (863 bytes)
CREATE angular-github-actions-amazon-s3/.browserslistrc (600 bytes)
CREATE angular-github-actions-amazon-s3/karma.conf.js (1449 bytes)
CREATE angular-github-actions-amazon-s3/tsconfig.app.json (287 bytes)
CREATE angular-github-actions-amazon-s3/tsconfig.spec.json (333 bytes)
CREATE angular-github-actions-amazon-s3/.vscode/extensions.json (130 bytes)
CREATE angular-github-actions-amazon-s3/.vscode/launch.json (474 bytes)
CREATE angular-github-actions-amazon-s3/.vscode/tasks.json (938 bytes)
CREATE angular-github-actions-amazon-s3/src/favicon.ico (948 bytes)
CREATE angular-github-actions-amazon-s3/src/index.html (314 bytes)
CREATE angular-github-actions-amazon-s3/src/main.ts (372 bytes)
CREATE angular-github-actions-amazon-s3/src/polyfills.ts (2338 bytes)
CREATE angular-github-actions-amazon-s3/src/styles.scss (80 bytes)
CREATE angular-github-actions-amazon-s3/src/test.ts (745 bytes)
CREATE angular-github-actions-amazon-s3/src/assets/.gitkeep (0 bytes)
CREATE angular-github-actions-amazon-s3/src/environments/environment.prod.ts (51 bytes)
CREATE angular-github-actions-amazon-s3/src/environments/environment.ts (658 bytes)
CREATE angular-github-actions-amazon-s3/src/app/app-routing.module.ts (245 bytes)
CREATE angular-github-actions-amazon-s3/src/app/app.module.ts (393 bytes)
CREATE angular-github-actions-amazon-s3/src/app/app.component.scss (0 bytes)
CREATE angular-github-actions-amazon-s3/src/app/app.component.html (23364 bytes)
CREATE angular-github-actions-amazon-s3/src/app/app.component.spec.ts (1151 bytes)
CREATE angular-github-actions-amazon-s3/src/app/app.component.ts (237 bytes)
✔ Packages installed successfully.
    Successfully initialized git.
```

**2.** Change the `package.json` file and add the scripts below.

```json
  "build:prod": "ng build --configuration production",
  "test:headless": "ng test --watch=false --browsers=ChromeHeadless"
```

**3.** Run the test with the command below.

```powershell
npm run test:headless

> angular-github-actions-amazon-s3@1.0.0 test:headless
> ng test --watch=false --browsers=ChromeHeadless

⠙ Generating browser application bundles (phase: setup)...22 01 2022 22:11:29.773:INFO [karma-server]: Karma v6.3.11 server started at http://localhost:9876/
22 01 2022 22:11:29.774:INFO [launcher]: Launching browsers ChromeHeadless with concurrency unlimited
22 01 2022 22:11:29.781:INFO [launcher]: Starting browser ChromeHeadless
✔ Browser application bundle generation complete.
22 01 2022 22:11:37.472:INFO [Chrome Headless 97.0.4692.71 (Linux x86_64)]: Connected on socket 2NkcZwzLS5MNuDnMAAAB with id 98670702
Chrome Headless 97.0.4692.71 (Linux x86_64): Executed 3 of 3 SUCCESS (0.117 secs / 0.1 secs)
TOTAL: 3 SUCCESS
```

**4.** Run the application with the command below. Access the URL `http://localhost:4200/` and check if the application is working.

```powershell
npm start

> angular-github-actions-amazon-s3@1.0.0 start
> ng serve

✔ Browser application bundle generation complete.

Initial Chunk Files   | Names         |  Raw Size
vendor.js             | vendor        |   2.00 MB | 
polyfills.js          | polyfills     | 339.24 kB | 
styles.css, styles.js | styles        | 213.01 kB | 
main.js               | main          |  53.27 kB | 
runtime.js            | runtime       |   6.90 kB | 

                      | Initial Total |   2.60 MB

Build at: 2022-01-23T01:13:06.355Z - Hash: e7a502736b12e783 - Time: 6256ms

** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **


✔ Compiled successfully.
```

**5.** Build the application with the command below.

```powershell
npm run build:prod

> angular-github-actions-amazon-s3@1.0.0 build:prod
> ng build --configuration production

✔ Browser application bundle generation complete.
✔ Copying assets complete.
✔ Index html generation complete.

Initial Chunk Files           | Names         |  Raw Size | Estimated Transfer Size
main.464c3a39ed8b2eb2.js      | main          | 206.10 kB |                56.11 kB
polyfills.f007c874370f7293.js | polyfills     |  36.27 kB |                11.56 kB
runtime.8db60f242f6b0a2b.js   | runtime       |   1.09 kB |               603 bytes
styles.ef46db3751d8e999.css   | styles        |   0 bytes |                       -

                              | Initial Total | 243.46 kB |                68.25 kB

Build at: 2022-01-23T01:14:01.354Z - Hash: cab5f3f1681e58fd - Time: 11304ms
```

**6.** Let's create and configure the file with the GitHub Actions flow. Create the `.github/workflows/gh-pages.yml` file.

```powershell
mkdir -p .github/workflows
touch .github/workflows/gh-pages.yml
```

**7.** Configure the `.github/workflows/gh-pages.yml` file with the content below.

```yaml
name: GitHub Pages

on:
  push:
    branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: sa-east-1

    - name: Checkout
      uses: actions/checkout@v2

    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: 14

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm run test:headless

    - name: Build
      run: npm run build:prod

    - name: Deploy
      if: success()
      run: aws s3 sync ./dist/angular-github-actions-amazon-s3 s3://angular-github-actions-amazon-s3
```

Notes:

* The `aws-access-key-id` and `aws-secret-access-key` settings were done in the GitHub repository.
* The `aws-region` setting is the bucket region.
* The `./dist/angular-github-actions-amazon-s3` setting is the application build folder.
* The `s3://angular-github-actions-amazon-s3` setting is the bucket name.

**8.** Syncronize the application on the GitHub repository that was created.

![GitHub - Repository](https://res.cloudinary.com/rodrigokamada/image/upload/v1642902812/Blog/angular-github-actions-amazon-s3/angular-github-actions-amazon-s3-step8.png)

**9.** Ready! After synchronizing the application on the GitHub repository, the GitHub Actions build the application and synchronize with Amazon S3 bucket. Access the URL [http://angular-github-actions-amazon-s3.s3-website-sa-east-1.amazonaws.com/](http://angular-github-actions-amazon-s3.s3-website-sa-east-1.amazonaws.com/) and check if the application is working. Replace the URL values with your bucket name and region.

![Angular GitHub Actions Amazon S3](https://res.cloudinary.com/rodrigokamada/image/upload/v1642902979/Blog/angular-github-actions-amazon-s3/angular-github-actions-amazon-s3-step9.png)


### Validate the run of the GitHub Actions flow

**1.** Let's validate the run of the GitHub Actions flow. Access the repository [https://github.com/rodrigokamada/angular-github-actions-amazon-s3](https://github.com/rodrigokamada/angular-github-actions-amazon-s3) created and click on the link *Actions*.

![GitHub Actions - Repository](https://res.cloudinary.com/rodrigokamada/image/upload/v1642903246/Blog/angular-github-actions-amazon-s3/github-actions-step1.png)

**2.** Click on the flow runned.

![GitHub Actions - Workflows](https://res.cloudinary.com/rodrigokamada/image/upload/v1642903427/Blog/angular-github-actions-amazon-s3/github-actions-step2.png)

**3.** Click on the job *deploy*.

![GitHub Actions - Jobs](https://res.cloudinary.com/rodrigokamada/image/upload/v1642903880/Blog/angular-github-actions-amazon-s3/github-actions-step3.png)

**4.** Click on each step to validate the run.

![GitHub Actions - Steps](https://res.cloudinary.com/rodrigokamada/image/upload/v1642903976/Blog/angular-github-actions-amazon-s3/github-actions-step4.png)

**5.** Ready! We validate the run of the GitHub Actions flow.



## Cloning the application

**1.** Clone the repository.

```powershell
git clone git@github.com:rodrigokamada/angular-github-actions-amazon-s3.git
```

**2.** Install the dependencies.

```powershell
npm ci
```

**3.** Run the application.

```powershell
npm start
```
