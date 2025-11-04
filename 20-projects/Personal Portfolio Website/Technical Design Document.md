# Technical Design Document: Personal Portfolio Website

## 1. Architecture Overview

**Decision:** Vercel for hosting and deployment, leveraging its native support for Next.js.

**Reasoning:** Vercel is the ideal platform for Next.js applications, offering a seamless development and deployment experience. It provides automatic HTTPS, a global CDN, serverless functions, and tight integration with GitHub, which aligns perfectly with the project's requirements for performance, security, and a streamlined CI/CD workflow.

**Components:**
*   **Frontend:** Next.js application.
*   **Deployment Platform:** Vercel.
*   **Serverless Functions:** Vercel Functions for any backend logic.
*   **DNS:** Vercel DNS for custom domain management.

### 1.1. CV Rendering Architecture
*   **Decision:** Build-time generation of CVs (generic and tailored) from Markdown data stored in a dedicated `cv/` directory.
*   **Reasoning:** This approach leverages the static site generation capabilities of Next.js, ensuring high performance, security, and scalability. Tailored CVs will be pre-generated at build time and deployed as static assets.
*   **Components:**
    *   **Markdown CV Files:** Stored in the `cv/` directory within the repository (e.g., `cv/generic.md`, `cv/job-x.md`).
    *   **CV Parser:** A build-time script using `gray-matter` and `remark` to parse Markdown CV files into structured JSON objects. This script will be integrated into the Next.js build process.
    *   **CV Renderer:** A React component in the Next.js application that takes the parsed CV data and renders it into an HTML page. This component will apply tailoring logic based on the frontmatter of each CV file.
    *   **Context Roots:** The build process will generate a page for each CV file. The `generic.md` will be rendered at the `/cv` route, and tailored CVs will have their own routes based on their filenames (e.g., `cv/job-x.md` becomes `/cv/job-x`).

### 1.2. PDF Generation Architecture
*   **Decision:** Use a GitHub Actions workflow to generate PDFs from the HTML CV pages at build time.
*   **Reasoning:** This approach provides full control over the build environment, allowing the use of the standard Puppeteer library without any compromises. It also separates the PDF generation process from the Vercel deployment.
*   **Components:**
    *   **GitHub Actions Workflow:** A workflow that is triggered on pushes to the main branch. This workflow will build the Next.js application, and then run a script to generate the PDFs.
    *   **PDF Generation Script:** A script that uses the standard Puppeteer library to launch a headless browser, navigate to each CV page, and generate a PDF.
    *   **Print-Specific CSS:** A separate CSS file (`print.css`) will be created to style the CV for printing.
    *   **PDF Storage:** The generated PDFs will be committed to the `public/cv/` directory in the Git repository.

## 2. System Design - Frontend

**Decision:** Next.js (Static Export) with Tailwind CSS.

**Reasoning:** Next.js offers a powerful development experience, excellent performance characteristics (especially with static export), and a component-based structure. Tailwind CSS enables rapid UI development with a utility-first approach, ensuring responsiveness and maintainability.

**Structure:**
*   Component-based architecture for reusability and maintainability.
*   Clear separation of concerns (e.g., UI components, data fetching logic).
*   **CV Rendering:** Implement components to parse Markdown CV files (using a library like `gray-matter` for frontmatter and `remark` for Markdown parsing) and render them into structured HTML, applying tailoring logic based on frontmatter metadata.

## 3. System Design - Backend (Optional/Minimal)

**Decision:** Vercel Functions for any dynamic elements.

**Reasoning:** Vercel Functions are serverless functions that are automatically deployed with the Next.js application. They are highly scalable, cost-effective, and require no additional infrastructure management, making them ideal for a contact form or other simple API endpoints.

**Example Use Cases:**
*   Contact form submission endpoint.
*   Simple API to fetch project details or blog posts.

## 4. Infrastructure as Code (IaC)

**Decision:** Terraform to manage Vercel resources.

**Reasoning:** Using Terraform to manage Vercel projects and domains allows for the codification of the entire infrastructure, ensuring that the Vercel configuration is version-controlled and repeatable.

**Core Vercel Resources to be provisioned via Terraform:**
*   **Vercel Project:** The main project configuration in Vercel.
*   **Vercel Domain:** The custom domain for the website.
*   **Vercel Environment Variables:** To store any secrets or configuration variables.

## 5. CI/CD Pipeline

**Decision:** A combination of GitHub Actions for PDF generation and Vercel for deployment.

**Reasoning:** This approach provides a robust and flexible CI/CD pipeline. GitHub Actions is used for the PDF generation, which requires a specific environment, while Vercel is used for its seamless and highly optimized deployment of Next.js applications.

**Key Pipeline Stages:**
*   **GitHub Actions Workflow (PDF Generation):**
    *   **Trigger:** On push to the `main` branch.
    *   **Build:** Install dependencies and build the Next.js application (`npm run build`).
    *   **Generate PDFs:** Run a script to generate PDF versions of each CV page using Puppeteer.
    *   **Commit PDFs:** Commit the generated PDFs to the `public/cv/` directory.
*   **Vercel Deployment:**
    *   **Trigger:** On push to the `main` branch (after the GitHub Actions workflow has completed and pushed the new commit).
    *   **Deploy:** Vercel automatically detects the new commit and deploys the application, including the pre-generated PDFs, to its global CDN.

## 6. Monitoring & Observability

**Decision:** Vercel Analytics and Vercel Logs.

**Reasoning:** Vercel provides built-in analytics and logging for all deployments. Vercel Analytics gives insights into website traffic and performance, while Vercel Logs provides real-time logs from serverless functions and the build process.

**Key Monitoring Aspects:**
*   **Analytics:** Track page views, visitors, and other web vitals.
*   **Logging:** Monitor build outputs and serverless function logs for errors and debugging.

## 7. Site Reliability Engineering (SRE) Principles

**Decision:** Rely on Vercel's managed infrastructure and SRE practices.

**Reasoning:** Vercel manages the underlying infrastructure, including the CDN, serverless functions, and security. This allows the project to benefit from Vercel's expertise in SRE without having to manage the infrastructure directly.

**SLOs & SLIs:**
*   **Availability:** Vercel provides a 99.99% uptime SLA for Enterprise customers. For a personal project, the standard Vercel infrastructure is more than sufficient.
*   **Latency:** Vercel's global CDN ensures low latency for users around the world.

## 8. Testing

**Decision:** Jest for unit tests, Cypress for End-to-End (E2E) tests, `terraform validate` and `tflint` for IaC validation.

**Reasoning:** These tools are industry standards, providing comprehensive testing capabilities across different layers of the application and infrastructure.

**Integration:** All tests will be integrated into the GitHub Actions CI pipeline to ensure automated quality checks on every code change.

## 9. Security

**Decision:** Leverage Vercel's built-in security features.

**Key Security Measures:**
*   **HTTPS:** Automatically enabled for all deployments.
*   **DDoS Mitigation:** Vercel's infrastructure is protected against DDoS attacks.
*   **Environment Variables:** Use Vercel's environment variable management for secrets.
*   **Dependency Security Scanning:** Use GitHub's Dependabot to scan for vulnerabilities in dependencies.

## 10. Interfaces and Integration Details

*   **Git Repository (GitHub):** Central source of truth for all application code and IaC configurations.
*   **Vercel:** The deployment platform, connected to the GitHub repository.
*   **Terraform:** To manage Vercel resources as code.
