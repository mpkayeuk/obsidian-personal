# DevOps & CI/CD

## Goal
To establish automated pipelines for building, testing, and deploying the personal portfolio website, embodying continuous integration and continuous delivery principles. This ensures rapid, reliable, and repeatable software releases.

## Detailed Design Steps

### 5.1. Choose CI/CD Platform
*   **Action:** Select a platform to host and execute the CI/CD pipelines.
*   **Decision:** A combination of GitHub Actions for PDF generation and Vercel for deployment.
*   **Reasoning:** This approach provides a robust and flexible CI/CD pipeline. GitHub Actions is used for the PDF generation, which requires a specific environment, while Vercel is used for its seamless and highly optimized deployment of Next.js applications.

### 5.2. Define CI Pipeline (Continuous Integration)
*   **Action:** Design the steps for the continuous integration process.
*   **GitHub Actions Workflow (PDF Generation):**
    *   **Trigger:** On push to the `main` branch.
    *   **Build:** Install dependencies and build the Next.js application (`npm run build`).
    *   **Generate PDFs:** Run a script to generate PDF versions of each CV page using Puppeteer.
    *   **Commit PDFs:** Commit the generated PDFs to the `public/cv/` directory.

### 5.3. Define CD Pipeline (Continuous Delivery/Deployment)
*   **Action:** Design the steps for the continuous delivery/deployment process.
*   **Vercel Deployment:**
    *   **Trigger:** On push to the `main` branch (after the GitHub Actions workflow has completed and pushed the new commit).
    *   **Deploy:** Vercel automatically detects the new commit and deploys the application, including the pre-generated PDFs, to its global CDN.

### 5.4. Implement Pipeline as Code
*   **Action:** Define CI/CD pipelines using configuration files (e.g., YAML) stored in the Git repository.
*   **Consideration:** Treat pipeline definitions as code, subject to version control, reviews, and testing.

### 5.5. Environment Management
*   **Action:** Define different deployment environments (e.g., `dev`, `staging`, `production`).
*   **Consideration:** Use environment-specific variables and configurations within the pipelines.

### 5.6. Notifications and Feedback
*   **Action:** Configure notifications for pipeline status (success/failure).
*   **Consideration:** Integrate with communication tools (e.g., Slack, email).
