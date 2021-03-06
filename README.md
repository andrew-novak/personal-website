# personal-website

My own portfolio website. The project consist of two front-end websites:
- publicly visible content (**client/**) 
- a password-protected admin panel, dependent on the react-admin module (**admin/**)

both interacting with
- a shared NodeJS API (**api/**).

The project's root contains some scripts useful in development and deployment, as well as **devImgsApi/** that serves images when in development mode.

To list, create or delete admins use the package **https://github.com/andrew-novak/personal-website-admin-manager**.

### How to start it? (Development Mode)

1. Clone the repo:

```
git clone https://github.com/andrew-novak/personal-website
```

2. Go to the project's root:

```
cd personal-website
```

3. Run the npm script in the project root:

```
npm run install-all
```

to install all the **node modules** for:

- project root
- **api/** subdirectory
- **client/** subdirectory
- **admin/** subdirectory

4. Run the bash script:

```
bash ./scripts/dev-gen-api-env.sh
```

to create a development API's **.env** file with default values.

5. Run the npm script:

```
npm run all
```

### How to deploy it? (Production Mode)

1. Set environment variables, eg.:

```
PERSONAL_WEBSITE_API_ROOT_ROUTE=/api
PERSONAL_WEBSITE_API_PORT=4000
PERSONAL_WEBSITE_IMGS_ACCESS=<whole url to images>
PERSONAL_WEBSITE_IMGS_LOCAL=<whole local path to images>
PERSONAL_WEBSITE_JWT_SECRET=<your own secret>
PERSONAL_WEBSITE_MONGO_URL=<whole url to your mongodb>
```

2. Make sure you have a static content and proxy server set (eg. Nginx) that would
- serve the images directory **PERSONAL_WEBSITE_IMGS_LOCAL** (eg. **/usr/share/nginx/personal-website-images**) when accessing the url **PERSONAL_WEBSITE_IMGS_ACCESS** (eg. **https://example.com/images**)
- serve the client website (eg. serve **/usr/share/nginx/personal-website-client** when accessing **https://example.com**)
- serve the admin website (eg. serve **/usr/share/nginx/personal-website-admin** when accessing **https://example.com/admin**)
- proxy NodeJS API (eg. proxy to **http://localhost:4000** when accessing **https://example.com/api**)

Eg. serve:
```
/usr/share/nginx/personal-website-images
```
when accessing:
```
https://example.com/images
```


3. Clone the repo:

```
git clone https://github.com/andrew-novak/personal-website
```

4. Go to the project's root:

```
cd personal-website
```

5. Run the npm script in the project's root:

```
npm run install-all
```

to install all the **node modules** for:

- project root
- **api/** subdirectory
- **client/** subdirectory
- **admin/** subdirectory

6. Rename and move the **api/** directory to desired location.

7. Start the API:
An example using PM2. You are free to use any other method.
```
mv <path to api/> <new location and name>
pm2 start <path to renamed api/>/server.js --name <api name for PM2>
```
Example values:
```
mv ./api ~/personal-website-api
pm2 start ~/personal-website-api/server.js --name personal-website-api
```
8. Run:

```
node ./scripts/setApiUrl.js <full api url>
```
eg.
```
node ./scripts/setApiUrl.js https://example.com/api
```

to set the full API url for **client/** and **admin/** subprojects

9*. Add **homepage** properties to **client/package.json** and/or **admin/package.json** if any of them will be accessible through subroutes (eg. **https://example.com/admin**):

```
json -I -f admin/package.json -e "this.homepage=\"<full admin url>\""
```
eg.
```
json -I -f admin/package.json -e "this.homepage=\"https://example.com/admin\""
```

You can just skip this step if they are not accessible through subroutes (eg. **https://example.com** or **https://admin.example.com**).

10. Build **client/** and **admin/** subprojects:

```
npm run build-static
```

11. Rename the newly created **admin/build/** and **client/build/** directories, and move them to desired locations.

Eg.
```
mv ./client/build /usr/share/nginx/personal-website-client
mv ./admin/build /usr/share/nginx/personal-website-admin
```
