---
title: "Building My Next.js Application: A Journey So Far"
description: "Technical Report and some story telling"
slug: building-my-first-next-js-site
date: 2024-10-07 
image: cover.jpeg
categories: 
  - Indie Development
  - Personal Journey
tags:
  - AI Tools 
  - Blueprint to the Mind 
weight: 1
---


Vision for the Application

The goal for this application goes beyond just a blog. I'm building it to serve multiple purposes, including:

Journal and Blog: A space to document my journey, my thoughts, and any interesting findings along the way.

Research and Brainstorming Hub: A place where I can manage my research, keep track of brainstorming sessions, and store all the insights I gather from different sources.

Conversation Tracking: I'm integrating this with my conversations with ChatGPT, turning it into a co-founder or co-worker of sorts—a place where I can look back at past discussions and ideas. It will be particularly useful for managing tangents and exploring ideas in depth. You can branch conversation threads off of each other, generate documents to save as artifacts, and have conversations from different contexts—pulling in different documents, information, or conversation history. It's like a fractal of conversations and explored thoughts, almost like Twitter for your brain, a blueprint of the mind, or a memory palace.

Knowledge Management Database: I want this app to act as a central repository for everything I learn and explore, making it easier to find information later and build upon existing knowledge.

Contextual Focus: It will help me hone in on specific contexts, offering tools to organize my work and thoughts depending on what I’m focusing on at any given time.

Task Tracker and Planner: I also plan to add functionality for managing tasks and planning ahead, so it doubles as a personal productivity tool.

Setting the Foundation

I recently embarked on a journey to build a web app using Next.js, and I wanted to share my progress so far. It's been a lot of experimenting, researching, and piecing together different technologies to create a modern, scalable web experience. The number of buzz words I learned is hilarious. I got the basics up and running almost immediately but setting up CORS and Authentication was a huge time drain. I'm glad I have a handle on it now though (assuming I'm not in the  Dunning Kruger effect lol). Please don't DDOS my api routes (praying hands emoji) So here's the report:

I started by setting up a Next.js project, making sure to incorporate TypeScript right from the start. TypeScript's type safety is invaluable, especially for building more robust applications. I also added Tailwind CSS to the project for quick styling, leveraging its utility-first approach to make my components clean and responsive.

Core Features

NextAuth for Authentication: One of the core aspects of my application is user authentication. I chose NextAuth because of its simplicity and flexibility. So far, I’ve managed to set up both an unprotected landing page and a protected page, ensuring that only authenticated users can access specific content.

Supabase for the Database: For database integration, I decided to use Supabase. It’s a great alternative to Firebase, offering a straightforward API and built-in PostgreSQL support. I haven’t fully connected it yet, but it's in the pipeline.

Exploring Templates: Challenges and Wins

Before settling on the current setup, I explored a few different templates and deployment options:

Hugo Site Deployed with GitHub Pages: I initially tried using a Hugo template for a static site. It deployed well with GitHub Pages, but adapting it to work with Netlify proved difficult, especially trying to get Netlify Identity to function correctly. Ultimately, it felt too rigid for my needs.

Next.js Site Deployed with Netlify: My next attempt was a basic Next.js blog deployed with Netlify. While it worked, it was very basic—lacking features like complex navigation, search, and categorization. Plus, I struggled to fully understand how to expand upon the chosen theme.

Starting Fresh with a Bare Bones Next.js App: Eventually, I decided to start with a bare bones Next.js setup and manually integrate everything I needed, including NextAuth for authentication. This approach allowed me to understand each component deeply, and I'll be hosting this version with Vercel for its streamlined deployment process.

Application Structure

Currently, the application has:

Dynamic Routes: These are key for creating unique pages like individual blog posts. I’ve set up dynamic routes to handle different user pages and content types.

SSR and SSG: I've implemented both Server-Side Rendering (SSR) and Static Site Generation (SSG) where needed. SSR is great for personalized content, while SSG helps with performance for static pages.

Reusable Layout Component: I created a layout.tsx file to maintain a consistent layout across pages, ensuring that shared elements like navigation and footer remain in one place.

Global Styles and Configs: I’ve set up a globals.css for shared styles and configured ESLint to keep my code consistent and clean. The next.config.mjs and package.json files help manage my dependencies and configurations.

Debugging Adventures: The Three-Period Bug

One particularly funny struggle I had was with a bug in my dynamic routing setup. I spent quite a bit of time trying to figure out why the route wasn't working as expected—only to realize that I needed three periods (...) instead of two (..) in the dynamic route definition. It was a simple typo, but it caused quite a headache until I figured it out. Definitely a good reminder to pay attention to those small details!

Setting Up CORS for Security

When deploying my API routes via serverless functions on Vercel, I faced the challenge of properly configuring CORS (Cross-Origin Resource Sharing) to ensure that only trusted domains can make requests.

Initially, I manually added CORS headers to the NextAuth route. Later, I realized that a more maintainable approach would be to use global middleware to add CORS headers for all API routes.

Here’s what I did:

Created a middleware.ts file to handle CORS for all API routes. This simplified the code in each individual API handler and ensured consistency across the project.

This middleware automatically adds the Access-Control-Allow-Origin header for requests from https://blueprint-of-the-mind.noahreardon.blog, allowing only this domain to access the server.

Testing this setup involved deploying to production and confirming that only authorized domains could access the API without encountering CORS issues. This setup greatly enhances the security of my application by preventing unauthorized domains from making API requests.

I also made sure to verify that my Vercel configuration wasn't interfering with CORS settings. Sometimes, default configurations may bypass or add headers, so I double-checked Vercel's edge settings to make sure everything was aligned.

Additionally, I ensured that NODE_ENV was correctly set in production, as this is crucial for determining the behavior of the middleware and the environment in which the application runs.

Local Development Setup

Right now, everything is running locally. I’m using VS Code as my main IDE, with Chrome for testing and GitHub Desktop for source control. This workflow allows me to iterate quickly and track changes effectively.

Issues We're Working Through

1. "Too Many Requests" Error

The HTTP 429 status code suggests that the app has exceeded its rate limit, either due to legitimate use or potentially from malicious requests.

To address this, we're planning to:

Implement rate limiting on the backend to prevent abuse.

Review access logs to identify any unusual activity.

Consider using a service like Cloudflare for DDoS protection and network-level rate limiting.

2. Session Storage and Security

The nextauth.message object in session storage is part of NextAuth’s internal mechanism. Storing sensitive data client-side could be vulnerable to XSS attacks.

To improve security, we're ensuring OAuth tokens are stored in HTTP-only cookies, which are inaccessible via JavaScript, providing enhanced security.

We are shifting session handling entirely server-side for better security. This involves keeping tokens in HTTP-only cookies and having minimal reliance on client-side storage, reducing exposure to attacks like XSS.

3. Implementing Content Security Policies (CSP)

We're adding a Content Security Policy (CSP) to mitigate security risks like cross-site scripting (XSS). The CSP configuration added is:

Additionally, we're adding COEP (Cross-Origin Embedder Policy) and COOP (Cross-Origin Opener Policy) headers for better security, especially for embedded resources:

These headers should be added in the next.config.mjs file or directly in the server response to ensure secure handling of cross-origin resources.

4. Adding Vercel Security Verification

To ensure only human users are visiting the site, I added Vercel Security features to help filter out bot traffic. This step aims to reduce unwanted or potentially harmful visits and enhance the overall security of the application.

Handling TypeScript Definitions and CORS Challenges

TypeScript type definitions threw me for a loop, especially when dealing with next-auth. Extending types for Session and JWT is a bit tricky and requires understanding how to properly augment the default module, particularly when adding custom fields like accessToken.

DefaultSession["user"] is used to extend the default user object that NextAuth provides. It ensures that all properties of the default user (such as name, email, etc.) are still included while adding custom properties like id. This allows us to add more fields without losing any of the existing fields that are provided by default, which is why it's used here.

Setting up CORS and authentication correctly is challenging. It’s hard to know when you’re truly "done" because there are so many potential security holes. It feels like configuring each piece in isolation, but they all need to work seamlessly together to avoid exposing vulnerabilities.

I also need to finalize the setup to store the accessToken server-side. This approach will allow me to securely call external services without exposing the token directly in client-side JavaScript.

Steps Taken So Far

Here’s a summary of the steps we’ve taken so far to develop the application:

Set Up Next.js with TypeScript and Tailwind CSS: Created the foundation for the project using TypeScript for type safety and Tailwind for utility-based styling.

Implemented Authentication with NextAuth: Added user authentication using NextAuth, which included setting up an unprotected landing page and a protected page for logged-in users.

Explored Templates and Made Key Decisions: Tried out various templates (like Hugo and Netlify) before deciding to start with a bare bones Next.js setup for more control.

Integrated NextAuth API Route: Created an API route for NextAuth by configuring route.ts under src/app/api/auth/[...nextauth] with authOptions to handle authentication.

Debugged TypeScript Errors with NextAuth: Faced a few issues, such as ensuring that authOptions was correctly imported and properly extending TypeScript types to accommodate custom user fields like user.id.

Extended NextAuth Types: Created a custom type definition file (next-auth.d.ts) under src/types to extend the default NextAuth types, ensuring that properties like user.id were recognized by TypeScript.

Configured ****************************************************************tsconfig.json: Updated the TypeScript configuration to include the custom types directory, ensuring all definitions were properly resolved during compilation.

Set Up CORS Headers: Since my serverless functions are hosted via Vercel, I needed to manually add CORS headers to ensure secure requests between my frontend and backend. This involves setting headers in API routes to allow requests only from trusted domains like https://blueprint-of-the-mind.noahreardon.blog. Properly configuring CORS prevents unauthorized requests and keeps data secure.

Implemented Global Middleware for CORS: Added a middleware.ts file to handle CORS globally, simplifying individual API route configurations and ensuring consistent CORS policy enforcement across all API endpoints.

Verified OAuth Redirect URIs: Updated the Google OAuth configuration with Authorized JavaScript Origins and Authorized Redirect URIs to allow seamless authentication flows both locally (http://localhost:3000) and in production (https://blueprint-of-the-mind.noahreardon.blog). This alignment is crucial for preventing CORS issues during the OAuth process.

Added Console Logs for Middleware Verification: To ensure the middleware.ts file is being invoked correctly, I added console logs that display when the middleware is triggered and which environment (development or production) is currently active. This helps verify that CORS settings are properly applied in both environments.

Double-Checked Vercel Configuration and Environment Variables: Verified that the Vercel configuration wasn’t interfering with CORS settings. Made sure that NODE_ENV was correctly set in production, as this is critical for determining the middleware behavior.

Secured OAuth Tokens: Made sure that OAuth tokens are stored securely. Tokens are handled server-side and stored in HTTP-only cookies, which are not accessible via JavaScript, providing an additional layer of security and reducing XSS vulnerability.

Postman Testing for API Security: Utilized Postman to test our CORS setup and verify the behavior of API routes. This helped ensure that only authorized domains could make requests, solidifying the security of our serverless functions.

Investigated Edge Function Behavior: Double-checked Vercel’s edge settings to ensure that no default configurations were bypassing our CORS middleware or adding unexpected headers.

Ensured Compatibility with OAuth Redirects: Ensured that middleware didn’t inadvertently block Google OAuth redirect requests during the authentication process, helping maintain smooth sign-in flows without compromising security.

Added a Favicon: To address the missing favicon issue, I added a favicon file (favicon.ico) to the public directory. This ensures that third-party tools like password managers or browser extensions can correctly retrieve the site icon.

Added Content Security Policy (CSP): Added the following Content Security Policy (CSP) to enhance security:

This was added in the next.config.mjs file to help prevent unauthorized access and reduce vulnerabilities like cross-site scripting (XSS).
19. Added COEP and COOP Headers: Added the following headers for enhanced security with embedded resources:

These were added in the next.config.mjs file to ensure that cross-origin resources are handled securely.
20. Added Vercel Security Features: Integrated Vercel Security to ensure only human visitors are accessing the site, reducing bot traffic and enhancing the security of the application.

Looking Ahead

Next up, I plan to fully integrate Supabase, add OAuth providers to the authentication setup, and start focusing on the content aspect of the site—like this blog! Once everything is polished locally, I'll be deploying the application on Vercel.

I also plan to explore Vercel’s KV database integration for enhanced storage capabilities, as it integrates nicely with Vercel and could be a good fit for managing some of the dynamic content aspects of my application.

It’s been exciting to put all these pieces together, and I’m looking forward to seeing where this journey takes me. Stay tuned for more updates as I continue building!

Technical Appendix: Folder Structure Overview

This section provides a detailed overview of the folder structure for developers who are interested in understanding the technical layout of the application.

Top-Level Folders and Files:

.next: This folder is auto-generated when you build your Next.js project. It contains all the output from the build process, including:

cache: Used by Next.js to optimize repeated builds.

server, static, types: These folders include the server-side code, static assets, and TypeScript type definitions used by Next.js during execution.

app-build-manifest.json, build-manifest.json, package.json, etc.: Metadata about your build and how it is optimized for production use.

node_modules: This is where all the dependencies and modules installed via npm or yarn are stored.

src: This is the main folder where your source code resides. Placing all the core code inside src helps in keeping the project organized. Let’s explore its subfolders:

app: The Next.js app directory follows the new App Router paradigm. It provides a new way of structuring your pages, including Server Components.

about/page.tsx: This is the component that defines the "About" page. The page.tsx naming convention tells Next.js that this is the main entry for the route.

api/auth/[...nextauth]/route.ts: This file is used to set up the NextAuth API route for authentication. The [...nextauth] dynamic segment captures various authentication endpoints (sign-in, sign-out, etc.).

fonts: Contains custom fonts (GeistMonoVF.woff, GeistVF.woff) used throughout the app, allowing you to have a unique typography style.

favicon.ico: This is the icon used in the browser tab for branding.

globals.css: This file contains global CSS styles that apply to the entire application, useful for setting typography, body styles, and shared utilities.

layout.tsx: This defines a reusable layout, like a template, that wraps around every page, including navigation bars, footers, and consistent design elements.

layout-client.tsx: It appears to be a client-side specific layout component, possibly used when SSR/SSG doesn’t cover the needs of certain pages.

page.tsx: This serves as the default home or landing page for your application.

components/button.tsx: Components are reusable pieces of UI. The button.tsx file likely contains a styled button component, ensuring consistency across the app.

lib, services, styles:

lib: Usually holds utility functions and reusable logic, such as helper methods for data transformation, API wrappers, etc.

services: Typically used for handling service-level logic like fetching data from APIs or communicating with external systems.

styles: Likely contains CSS files for styling individual components or managing custom Tailwind CSS configurations.

Config and Other Files:

.env.local: This file is used to store environment variables that are specific to your local development environment, like API keys or database URLs.

.eslintrc.json: Configuration file for ESLint, which helps enforce consistent code style and catch errors.

.gitattributes****, ********************************.gitignore: Files for Git configuration:

.gitattributes: Specifies Git handling for different types of files, like line endings.

.gitignore: Specifies files and directories that Git should ignore, such as build outputs or sensitive files.

next-env.d.ts: Automatically generated by Next.js to ensure TypeScript types for Next.js are available globally.

next.config.mjs: The main configuration file for Next.js. This can be used to configure custom webpack settings, environment variables, redirects, and more.

package.json: Contains metadata about the project, including scripts, dependencies, and configuration for running the application.

postcss.config.mjs: Configuration for PostCSS, often used with Tailwind CSS to process and optimize styles.

README.md: Typically used to provide an overview and instructions about the project for other developers.

tailwind.config.js: The configuration for Tailwind CSS, allowing you to customize the design system to meet your needs.

tsconfig.json: TypeScript configuration file, defining compiler options and path aliases to help TypeScript understand the structure.

Summary of Rationale:

Modularization and Reusability: Files such as button.tsx are stored in components to ensure UI elements are reusable, promoting clean and maintainable code.

Separation of Concerns: Grouping related files like APIs (api), components, services, and styles makes it easier to navigate the project and maintain code consistency.

App Router & Layout: The use of app and the layout.tsx leverages Next.js's new App Router feature for cleaner routing and consistent page design.

Organization for Scalability: Splitting logic into lib and services allows you to scale without creating tightly coupled, monolithic components.

Overall, your folder structure reflects an organized approach that balances flexibility and scalability, which will make future expansion and maintenance much easier. Let me know if you want to discuss any of these elements in more detail or make changes!