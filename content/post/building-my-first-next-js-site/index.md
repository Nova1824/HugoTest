---
title: "Building My Next.js Application: A Journey So Far"
description: "Technical Report and Some Storytelling"
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

# Building My Next.js Application: A Journey So Far

## Vision for the Application

The goal for this application goes beyond just a blog. I'm building it to serve multiple purposes, including:

- **Journal and Blog**: A space to document my journey, my thoughts, and any interesting findings along the way.

- **Research and Brainstorming Hub**: A place where I can manage my research, keep track of brainstorming sessions, and store all the insights I gather from different sources.

- **Conversation Tracking**: I'm integrating this with my conversations with ChatGPT, turning it into a co-founder or co-worker of sorts. It will be particularly useful for managing tangents and exploring ideas in depth. Imagine branching conversation threads off each other, generating documents to save as artifacts, and having conversations from different contexts—pulling in different documents, information, or conversation history. It’s like a fractal of conversations, almost like Twitter for your brain or a memory palace.

- **Knowledge Management Database**: I want this app to act as a central repository for everything I learn and explore, making it easier to find information later and build upon existing knowledge.

- **Contextual Focus**: It will help me hone in on specific contexts, offering tools to organize my work and thoughts depending on what I’m focusing on at any given time.

- **Task Tracker and Planner**: I also plan to add functionality for managing tasks and planning ahead, so it doubles as a personal productivity tool.

## Setting the Foundation

I recently embarked on a journey to build a web app using Next.js, and I wanted to share my progress so far. It's been a lot of experimenting, researching, and piecing together different technologies to create a modern, scalable web experience. The number of buzzwords I’ve learned is hilarious. I got the basics up and running almost immediately, but setting up CORS and authentication was a huge time drain. I'm glad I have a handle on it now, though (assuming I’m not in the Dunning-Kruger effect 🤞). So here’s the report:

I started by setting up a Next.js project, incorporating TypeScript right from the start. TypeScript's type safety is invaluable, especially for building robust applications. I also added Tailwind CSS for quick styling, leveraging its utility-first approach to keep my components clean and responsive.

## Core Features

- **NextAuth for Authentication**: One of the core aspects of my application is user authentication. I chose NextAuth because of its simplicity and flexibility. So far, I’ve managed to set up both an unprotected landing page and a protected page, ensuring that only authenticated users can access specific content.

- **Supabase for the Database**: For database integration, I decided to use Supabase. It’s a great alternative to Firebase, offering a straightforward API and built-in PostgreSQL support. I haven’t fully connected it yet, but it's in the pipeline.

## Exploring Templates: Challenges and Wins

Before settling on the current setup, I explored a few different templates and deployment options:

- **Hugo Site Deployed with GitHub Pages**: I initially tried using a Hugo template for a static site. It deployed well with GitHub Pages, but adapting it to work with Netlify proved difficult, especially getting Netlify Identity to function correctly. Ultimately, it felt too rigid for my needs.

- **Next.js Site Deployed with Netlify**: My next attempt was a basic Next.js blog deployed with Netlify. While it worked, it was very basic—lacking features like complex navigation, search, and categorization. Plus, I struggled to fully understand how to expand upon the chosen theme.

- **Starting Fresh with a Bare-Bones Next.js App**: Eventually, I decided to start with a bare-bones Next.js setup and manually integrate everything I needed, including NextAuth for authentication. This approach allowed me to understand each component deeply, and I'll be hosting this version with Vercel for its streamlined deployment process.

## Application Structure

- **Dynamic Routes**: These are key for creating unique pages like individual blog posts. I’ve set up dynamic routes to handle different user pages and content types.

- **SSR and SSG**: I've implemented both Server-Side Rendering (SSR) and Static Site Generation (SSG) where needed. SSR is great for personalized content, while SSG helps with performance for static pages.

- **Reusable Layout Component**: I created a `layout.tsx` file to maintain a consistent layout across pages, ensuring shared elements like navigation and footer remain in one place.

- **Global Styles and Configs**: I’ve set up `globals.css` for shared styles and configured ESLint to keep my code consistent and clean. The `next.config.mjs` and `package.json` files help manage my dependencies and configurations.

## Debugging Adventures: The Three-Period Bug

One particularly funny struggle I had was with a bug in my dynamic routing setup. I spent quite a bit of time trying to figure out why the route wasn't working as expected—only to realize that I needed three periods (`...`) instead of two (`..`) in the dynamic route definition. It was a simple typo, but it caused quite a headache until I figured it out. Definitely a good reminder to pay attention to those small details!

## Setting Up CORS for Security

When deploying my API routes via serverless functions on Vercel, I faced the challenge of properly configuring CORS (Cross-Origin Resource Sharing) to ensure that only trusted domains can make requests.

Here’s what I did:

- **Created a `middleware.ts` file** to handle CORS for all API routes. This simplified the code in each individual API handler and ensured consistency across the project.

- **Testing** involved deploying to production and confirming that only authorized domains could access the API without encountering CORS issues. This setup greatly enhances the security of my application by preventing unauthorized domains from making API requests.

## Local Development Setup

Right now, everything is running locally. I’m using VS Code as my main IDE, with Chrome for testing and GitHub Desktop for source control. This workflow allows me to iterate quickly and track changes effectively.

## Issues We're Working Through

### 1. "Too Many Requests" Error

The HTTP 429 status code suggests that the app has exceeded its rate limit, either due to legitimate use or potentially from malicious requests.

- **Solutions**:
  - Implement rate limiting on the backend to prevent abuse.
  - Review access logs to identify any unusual activity.
  - Consider using a service like Cloudflare for DDoS protection and network-level rate limiting.

### 2. Session Storage and Security

The `nextauth.message` object in session storage is part of NextAuth’s internal mechanism. Storing sensitive data client-side could be vulnerable to XSS attacks.

- **Solutions**:
  - Ensure OAuth tokens are stored in HTTP-only cookies, which are inaccessible via JavaScript, providing enhanced security.
  - Shift session handling entirely server-side for better security, minimizing reliance on client-side storage.

### 3. Implementing Content Security Policies (CSP)

- Adding a Content Security Policy (CSP) to mitigate risks like cross-site scripting (XSS).
- Adding COEP (Cross-Origin Embedder Policy) and COOP (Cross-Origin Opener Policy) headers for better security, especially for embedded resources.

## Looking Ahead

Next up, I plan to fully integrate Supabase, add OAuth providers to the authentication setup, and start focusing on the content aspect of the site—like this blog! Once everything is polished locally, I'll be deploying the application on Vercel.

I also plan to explore Vercel’s KV database integration for enhanced storage capabilities, as it integrates nicely with Vercel and could be a good fit for managing some of the dynamic content aspects of my application.

It’s been exciting to put all these pieces together, and I’m looking forward to seeing where this journey takes me. Stay tuned for more updates as I continue building!

## Technical Appendix: Folder Structure Overview

This section provides a detailed overview of the folder structure for developers interested in understanding the technical layout of the application.

### Top-Level Folders and Files

- **.next**: Auto-generated when you build your Next.js project.
  - **cache**: Used by Next.js to optimize repeated builds.
  - **server, static, types**: Server-side code, static assets, and TypeScript type definitions used by Next.js during execution.

- **node_modules**: Dependencies installed via npm or yarn.

- **src**: Main folder where the source code resides.
  - **app**: The Next.js app directory follows the new App Router paradigm.
    - **about/page.tsx**: Defines the "About" page.
    - **api/auth/[...nextauth]/route.ts**: Sets up the NextAuth API route for authentication.
  - **fonts**: Contains custom fonts (e.g., GeistMonoVF.woff).
  - **favicon.ico**: The browser tab icon.
  - **globals.css**: Global CSS styles for typography, body styles, and shared utilities.
  - **layout.tsx**: Reusable layout wrapping around every page, including navigation bars and footers.

- **components**: Reusable UI pieces, like `button.tsx`.

- **lib, services, styles**:
  - **lib**: Utility functions and reusable logic.
  - **services**: Handles service-level logic like fetching data from APIs.
  - **styles**: CSS files for styling individual components.

### Config and Other Files

- **.env.local**: Stores environment variables for local development, like API keys.
- **.eslintrc.json**: Configuration for ESLint to enforce code style.
- **.gitignore**: Specifies files and directories that Git should ignore.
- **next.config.mjs**: Main configuration for Next.js, including custom webpack settings.
- **package.json**: Metadata about the project, including scripts and dependencies.
- **tailwind.config.js**: Configuration for Tailwind CSS.
- **tsconfig.json**: TypeScript configuration for compiler options and path aliases.

### Summary of Rationale

- **Modularization and Reusability**: Files like `button.tsx` are stored in components to ensure UI elements are reusable, promoting clean and maintainable code.
- **Separation of Concerns**: Grouping related files (e.g., APIs, components, services) makes navigation easier and keeps the codebase consistent.
- **App Router & Layout**: The use of `app` and `layout.tsx` leverages Next.js's new App Router feature for cleaner routing and consistent page design.
- **Scalability**: Splitting logic into `lib` and `services` helps scale without creating tightly coupled, monolithic components.

Overall, the folder structure reflects an organized approach balancing flexibility and scalability, making future expansion and maintenance much easier.