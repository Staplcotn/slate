# A New Project

1. Make a new Folder with a `package.json`

2. Make two more folders inside named `api` and `app`

### The App

Instructions from [here](https://create-react-app.dev/docs/adding-typescript/).

Inside the `app` folder, run `yarn create react-app my-app --template typescript`

### The Api

1. `yarn init -y && yarn add @scca/stapl-api express cors`

2. `yarn add -D typescript ts-node-dev`

3. Add this script to your `package.json`:

` "scripts": { "build": "tsc", "start": "node index.js", "startDev": "ts-node-dev -r dotenv/config index.ts" }`

It should run or whatever

### Considerations around your hostname

Because the current plan is to host these on Google Cloud, we're using the Load Balancer in a way that requires you to know the name of your app while your programming.
