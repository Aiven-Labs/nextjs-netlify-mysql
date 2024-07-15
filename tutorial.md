# Robust and Secure: Deploy Scalable JAM Stack Applications on Netlify with MySQL and Aiven for Caching

This tutorial guides you through deploying a [Netlify](https://www.netlify.com/) web application with an [Aiven for MySQL](https://aiven.io/mysql) and [Aiven for Caching](https://aiven.io/caching) backend.

The sample application used in this tutorial is a cooking recipe library displaying open source [recipe data](https://www.kaggle.com/datasets/thedevastator/better-recipes-for-a-better-life). It's built in [Next.js](https://nextjs.org/), which can be deployed to Netlify with ease, and connects to PostgreSQL with [Prisma](https://www.prisma.io/) and Aiven for Caching with [ioredis](https://www.npmjs.com/package/ioredis).

The source code for the application is available on GitHub at https://github.com/Aiven-Labs/nextjs-netlify-mysql.

# Before you begin

Before starting the tutorial, do the following if you haven't already:

1. Sign up to [Aiven](https://console.aiven.io/signup?referral_code=v3tvatqoyzbu3yp2oiy9).
1. Sign up to [Netlify](https://app.netlify.com/signup).
1. Install the [GitHub CLI](https://github.com/cli/cli#installation).
1. Install the [Netlify CLI](https://docs.netlify.com/cli/get-started/).

# Step 1. Create free MySQL and Aiven for Caching services

Follow the instructions below to create your free MySQL and Aiven for Caching services with Aiven. You can read more about the Aiven free plans in the [Aiven documentation](https://docs.aiven.io/docs/platform/concepts/free-plan).

1. Login to [Aiven Console](https://console.aiven.io).
1. In the project you want to create a service in, go to **Services**.
1. On the **Services** page, click **Create service**.
1. Select the service you want to create, either **MySQL** or **Aiven for Caching**.
1. Select **DigitalOcean** as the cloud provider and choose the region.

   We recommend using `do-nyc` to minimise latency, as this region will be closest to where the Netlify functions are deployed if using their free plan. However, the Aiven free plans are currently available in the following regions and you can pick whichever you like:

   - EMEA: `do-ams` (Amsterdam), `do-ldn` (London), `do-fra` (Frankfurt)
   - Americas: `do-nyc` (New York), `do-sfo` (San Francisco), `do-tor` (Toronto)
   - APAC: `do-blr` (Bangalore)

1. Select **Free** plan.
1. Click **Create free service**.

# Step 2. Get the application code

To deploy the application to Netlify you'll need to have your own GitHub repository for [Aiven-Labs/nextjs-netlify-mysql](https://github.com/Aiven-Labs/nextjs-netlify-mysql). See the relevant [GitHub CLI instructions to fork the repository](https://docs.github.com/en/get-started/quickstart/fork-a-repo?tool=cli#forking-a-repository).

Fork and clone the forked repository with GitHub CLI

```
gh repo fork https://github.com/Aiven-Labs/nextjs-netlify-mysql.git --clone --remote
```

You can name the repository by adding `--fork-name <name>` to the command.

# Step 3. Deploy the application

Follow the instructions below to configure continuous deployment. This will automatically deploy your changes.

1. Navigate to the `nextjs-netlify-mysql` directory on your local machine:

   ```
   cd nextjs-netlify-mysql
   ```

1. Sign into your Netlify account to obtain the access token:

   ```
   netlify login
   ```

1. Configure continuous deployment:

   Start and follow the automated setup process. Choose to create a new site, and then select the default options for other prompts. Note that you will be required to authorize Netlify with GitHub. You can do so either through app.netlify.com or with a GitHub personal access token.

   ```
   netlify init
   ```

1. Open the Netlify site admin user interface:

   ```
   netlify open --admin
   ```

1. Configure the site in Netlify:

   1. Go to **Site configuration**.
   1. Go to **Environment variables**.
   1. Add variable `DATABASE_URI`:

      Copy the PostgreSQL database URI from the `Service Overview` page in the Aiven console and paste it in as the `DATABASE_URI` value.

      - To create a new database, replace `defaultdb` with the new database name.
      - To configure the schema for the application, add `&schema=public` to the end of the URI.

      Example URI below:

      ```
      mysql://user:password@host:port/defaultdb?sslmode=require&schema=public
      ```

   1. Add variable `REDIS_URI`:

      Copy the Redis URI from the `Service Overview` page in the Aiven console and paste it in as the `REDIS_URI` value.

      Example URI below:

      ```
      rediss://user:password@host:port
      ```

1. Deploy the site:

   Go to the **Deploys** section in Netlify and trigger the deployment by clicking the `Trigger deploy` -button.

   **NOTE:** The database migrations and seed are done during the Netlify deployment process. Make sure you have the PostgreSQL database up and running when deploying to achieve a working environment on the first try.

1. Visit the deployed site:

   Open the site from the Netlify admin user interface or from the command line with:

   ```
    netlify open --site
   ```

Congratulations, you have now successfully deployed your application. Any subsequent changes you push will automatically be deployed to your site.

# Step 4. Use the application

This example application has a **Home** page with some relevant links and a **Recipes** page where you can browse and inspect recipes. You can like recipes that seem interesting and filter the list to show only liked ones. Furthermore, you can see interesting statistics from all of the recipes and those that are liked.

Aiven for PostgreSQL is used as the database for storing all recipes and whether they are liked or not. Aiven for Caching is optional for the app to function, but when configured statistics are cached to offer faster response time. You are able to toggle Caching on and off. The database response times are shown in the app to demonstrate the difference.

# For more information, see

- [Aiven documentation](https://docs.aiven.io)
- [Aiven for MySQL documentation](https://aiven.io/docs/products/mysql)
- [Aiven for Caching documentation](https://aiven.io/docs/products/caching)
- [Netlify CLI Command list](https://cli.netlify.com/)
