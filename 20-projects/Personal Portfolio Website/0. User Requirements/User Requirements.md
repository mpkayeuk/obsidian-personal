# User Requirements for Personal Portfolio Website

## Goal
To define the functional and non-functional requirements for the personal portfolio website from the perspective of its users (e.g., recruiters, hiring managers). This document serves as a foundation for design and development, ensuring the website meets user expectations.

## 0.1. Functional Requirements

### FR 0.1.1. Profile Display
*   The website MUST display a comprehensive "About Me" section, including professional summary, career aspirations, and a professional photo.

### FR 0.1.2. CV Data Parsing
*   The website MUST be able to parse CV data from a structured Markdown format (as defined in `0.1. CV Data Format/CV Data Format.md`).

### FR 0.1.3. Generic CV Rendering
*   The website MUST render a generic version of the CV at the `/cv` route.

### FR 0.1.4. Tailored CV Rendering
*   The website MUST be able to render tailored versions of the CV at specified context roots based on their filename (e.g., `/cv/job-x`), based on metadata within the CV Markdown file.
*   The tailoring mechanism MUST allow for inclusion/exclusion of sections, skills, or work experience highlights based on job-specific criteria.

### FR 0.1.5. Experience Showcase
*   The website MUST present detailed professional experience, including roles, responsibilities, and quantifiable achievements.

### FR 0.1.6. Project Portfolio
*   The website MUST showcase a portfolio of projects, with a strong emphasis on DevOps, SRE, Monitoring, and IaC implementations. 
*   Each project entry MUST include:
    *   Project title and brief description.
    *   Technologies used.
    *   My role and contributions.
    *   Challenges faced and solutions implemented.
    *   Links to code repositories (e.g., GitHub).
    *   Links to live demos or detailed write-ups (e.g., blog posts).
    *   Diagrams or screenshots illustrating architecture and key components.

### FR 0.1.7. Skills Matrix
*   The website MUST clearly list technical skills, categorized by domain (e.g., Cloud Platforms, IaC Tools, CI/CD, Monitoring, Languages).

### FR 0.1.8. Contact Information
*   The website MUST provide clear ways for users to contact me (e.g., email, LinkedIn profile link).

### FR 0.1.9. Blog/Articles (Optional)
*   The website MUST provide a section for blog posts or articles where I can share insights, tutorials, or case studies related to DevOps/SRE.

### FR 0.1.10. CV PDF Generation and Printing
*   Each CV page (both generic and tailored) MUST include a prominent download link for a PDF version of the CV.
*   The PDF version MUST be a high-quality, printable representation of the HTML CV.
*   The PDFs for all CVs MUST be generated at build time during the CI/CD process.
*   Each CV page MUST include a print button that allows users to print the CV directly from their browser, using the same print-friendly styling as the PDF.

## 0.2. Non-Functional Requirements

### NFR 0.2.1. Performance
*   The website MUST load within 2 seconds on a standard broadband connection.
*   The website SHOULD achieve a Lighthouse performance score of 90+ for desktop.

### NFR 0.2.2. Availability
*   The website MUST be available 99.9% of the time (excluding planned maintenance).

### NFR 0.2.3. Responsiveness
*   The website MUST be fully responsive and display correctly on various devices (desktop, tablet, mobile).

### NFR 0.2.4. Security
*   The website MUST use HTTPS for all communication.
*   The website MUST be protected against common web vulnerabilities (e.g., XSS, SQL Injection if applicable).

### NFR 0.2.5. Maintainability
*   The website's codebase MUST be well-structured and easy to maintain.
*   The infrastructure MUST be defined as code for easy updates and replication.

### NFR 0.2.6. Usability
*   The website MUST have intuitive navigation.
*   The content MUST be easy to read and understand.

### NFR 0.2.7. Accessibility
*   The website SHOULD adhere to WCAG 2.1 AA guidelines.
