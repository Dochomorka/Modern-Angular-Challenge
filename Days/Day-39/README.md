# Day 39 — Build and Deployment

Day 39 is about turning your Angular app into production-ready files and deploying it. Angular’s CLI reference says `ng build` compiles the app into a `dist/` output folder, and Angular’s deployment docs explain that the built files are static and can be served by a web server or hosting platform [web:401][web:404][web:407].

## Goal

By the end of this day, you should be able to:

- Build an Angular app for production.
- Understand what goes into the `dist/` folder.
- Use build configurations for different environments.
- Serve the built app locally.
- Deploy to a static hosting target.
- Recognize what changes between development and production.

## Why This Matters

A working app in development still needs production packaging. Angular’s CLI and deployment docs show that production builds optimize the app for serving, and environment-specific build settings help you control behavior across development, staging, and production [web:398][web:399][web:401].

This matters because:
- Production builds are smaller and faster.
- Static hosting is simple and reliable.
- Environment configs keep secrets and settings separate.
- Deployment is the final step before real users see the app.

## Build Basics

Angular CLI uses `ng build` to create production output in a `dist/` directory. Angular build documentation also supports environment-specific configurations, which let you customize values for different deployments [web:401][web:407][web:399].

Common build ideas:
- Development build for fast iteration.
- Production build for optimized output.
- Separate config for staging or preview.
- Static files ready for hosting.

## Local Preview

After building, you can serve the output locally to verify the production bundle before deployment. MDN’s Angular build guide describes serving the compiled `dist` output with a static server such as `http-server` [web:398].

## Deployment Targets

Angular’s deployment docs and CLI references indicate that the built app can be deployed to static hosts or platforms that serve files from `dist/` [web:404][web:401][web:406].

Common targets:
- Netlify.
- Vercel.
- Firebase Hosting.
- Cloudflare Pages.
- Any static web server.

## Practical Example

```bash
ng build --configuration production
```

This generates optimized files for deployment, and Angular’s docs describe the build output as static assets that can be hosted on a server or platform that serves files [web:401][web:398][web:404].

## Environment Configuration

Angular supports different named build configurations, such as development and staging. That makes it easier to point the same app at different APIs or settings depending on where you deploy it [web:399].

Example use cases:
- Development API endpoint.
- Staging API endpoint.
- Production API endpoint.

## Best Practices

- Always test the production build locally.
- Keep environment settings separate.
- Use the correct build configuration for the target.
- Deploy only the compiled output, not source code.
- Confirm routing works on your host.

## Easy Challenges

- Run a production build.
- Inspect the `dist/` output.
- Serve the build locally.
- Change one build configuration.
- Deploy a small test app to a static host.

## Medium Challenges

- Add a staging configuration.
- Compare dev and production builds.
- Verify routes on a deployed host.
- Automate build steps in a script.
- Check environment values after deployment.

## Hard Challenges

- Deploy a routed Angular app to static hosting.
- Configure a host for client-side routing fallback.
- Add multiple environment builds.
- Create a deployment checklist.
- Validate the app in a production-like preview.

## Reflection Questions

- What does `ng build` produce?
- Why is a production build different from development?
- What should you test before deploying?
- How do environment configurations help?
- Why do static hosts work well for Angular apps?

## Day Deliverable

Create one deployment flow that includes:

- One production build command.
- One local preview step.
- One deployment target.
- One environment configuration.
- One note about what changed in production.

## Suggested Practice Flow

1. Run a production build.
2. Inspect the output folder.
3. Preview it locally.
4. Choose a deployment target.
5. Upload or connect the build output.
6. Complete the easy, medium, and hard challenges.

## Note for Day 40

Next day covers **Capstone Project and Review**, which is the final step in putting everything together.
