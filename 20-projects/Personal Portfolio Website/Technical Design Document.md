# Technical Design Document: Personal Portfolio Website

## 1. Architecture Overview

**Decision:** Static Site Architecture with optional Serverless Backend.

**Reasoning:** This architecture provides high performance, excellent security, low operational overhead, and cost-effectiveness, making it ideal for a personal portfolio website. It also naturally integrates with IaC, CI/CD, and SRE principles for robust deployment and management.

**Components:**
*   **Frontend:** Static assets (HTML, CSS, JavaScript, images).
*   **Content Delivery Network (CDN):** For global distribution, caching, and SSL termination.
*   **DNS:** Custom domain management.
*   **Optional Serverless Functions:** For dynamic functionalities like a contact form submission or fetching project data from a simple API.

### 1.1. CV Rendering Architecture
*   **Decision:** Build-time generation of CVs (generic and tailored) from Markdown data.
*   **Reasoning:** This approach leverages the static site generation capabilities, ensuring high performance, security, and scalability. Tailored CVs will be pre-generated at build time and deployed as static assets.
*   **Components:**
    *   **Markdown CV Files:** Stored in the repository.
    *   **CV Parser:** A build-time script/tool to parse Markdown CV files into structured data (e.g., JSON).
    *   **CV Renderer:** A build-time component that takes the parsed CV data and renders it into HTML pages, applying tailoring logic based on metadata.
    *   **Context Roots:** Tailored CVs will be deployed to specific sub-paths (e.g., `/job-x-cv/index.html`).

## 2. System Design - Frontend

**Decision:** Next.js (Static Export) with Tailwind CSS.

**Reasoning:** Next.js offers a powerful development experience, excellent performance characteristics (especially with static export), and a component-based structure. Tailwind CSS enables rapid UI development with a utility-first approach, ensuring responsiveness and maintainability.

**Structure:**
*   Component-based architecture for reusability and maintainability.
*   Clear separation of concerns (e.g., UI components, data fetching logic).
*   **CV Rendering:** Implement components to parse Markdown CV files (using a library like `gray-matter` for frontmatter and `remark` for Markdown parsing) and render them into structured HTML, applying tailoring logic based on frontmatter metadata.

## 3. System Design - Backend (Optional/Minimal)

**Decision:** AWS Lambda and API Gateway for any dynamic elements.

**Reasoning:** Serverless functions are highly scalable, cost-effective (pay-per-execution), and require minimal operational management, aligning well with SRE principles. AWS API Gateway provides a secure and scalable entry point for these functions.

**Example Use Cases:**
*   Contact form submission endpoint.
*   Simple API to fetch project details or blog posts from a lightweight database (e.g., DynamoDB).

## 4. Infrastructure as Code (IaC)

**Decision:** AWS as Cloud Provider, Terraform as IaC tool.

**Reasoning:** AWS is a leading cloud provider with a comprehensive suite of services, offering extensive options for hosting and managing web applications. Terraform is a cloud-agnostic and widely adopted IaC tool, demonstrating a valuable and transferable skill.

**Core AWS Resources to be provisioned via Terraform:**
*   **S3 Bucket:** Configured for static website hosting.
*   **CloudFront Distribution:** To serve content globally, cache assets, provide HTTPS termination, and improve performance/security.
*   **Route 53:** For managing DNS records and pointing a custom domain to the CloudFront distribution.
*   **AWS Certificate Manager (ACM):** To provision and manage SSL/TLS certificates for HTTPS on CloudFront.
*   **AWS Lambda & API Gateway:** (If optional backend is implemented) for serverless function deployment.

## 5. CI/CD Pipeline

**Decision:** GitHub Actions.

**Reasoning:** GitHub Actions provides tight integration with the project's Git repository, uses YAML-based workflows for pipeline-as-code, and is a widely used platform in modern DevOps practices.

**Key Pipeline Stages:**
*   **Build:**
    *   Install frontend dependencies (`npm install`).
    *   Build static assets (`npm run build` for Next.js static export).
    *   **Parse and Render CVs:** Execute a build script to parse Markdown CV files and generate static HTML files for generic and tailored CVs.
*   **Test:**
    *   Run unit tests (e.g., Jest).
    *   Run linting and static analysis (e.g., ESLint, Prettier).
    *   Validate IaC configuration (`terraform validate`, `tflint`, `checkov`).
*   **IaC Plan (Manual Approval for Production):**
    *   Execute `terraform plan` to preview infrastructure changes.
    *   Require manual approval for `terraform apply` on production environments.
*   **Deploy:**
    *   Execute `terraform apply` to provision/update infrastructure.
    *   Synchronize static assets to S3 bucket (`aws s3 sync`).
    *   Invalidate CloudFront cache (`aws cloudfront create-invalidation`) to ensure new content is served.
*   **Deploy CVs:** Ensure generated CV HTML files are included in the S3 sync and CloudFront invalidation.

## 6. Monitoring & Observability

**Decision:** AWS CloudWatch for metrics and logs, Grafana for custom dashboards.

**Reasoning:** CloudWatch is AWS-native, providing seamless integration for collecting infrastructure metrics and centralizing logs. Grafana offers powerful and flexible visualization capabilities for creating custom, insightful dashboards.

**Key Monitoring Aspects:**
*   **Metrics:** CloudFront access logs (requests, 4xx/5xx errors), S3 bucket metrics (requests, data transfer), Lambda invocation metrics (duration, errors).
*   **Logging:** Centralized logging of application and infrastructure events in CloudWatch Logs.
*   **Alerting:** CloudWatch Alarms configured for critical thresholds (e.g., high 5xx error rates, increased latency, Lambda errors).
*   **Dashboards:** Grafana dashboards to visualize website availability, performance, and user traffic in real-time.

## 7. Site Reliability Engineering (SRE) Principles

**Decision:** Focus on Availability and Latency as primary SLOs.

**Reasoning:** For a portfolio website, ensuring the site is always accessible and loads quickly are paramount for user experience and showcasing reliability skills.

**SLOs & SLIs:**
*   **Availability SLO:** 99.9% uptime (measured by successful HTTP 2xx/3xx responses from CloudFront).
*   **Latency SLO:** 95th percentile page load time < 1 second (measured by CloudFront response time).
*   **Error Budget:** Derived from these SLOs, guiding decisions on feature development vs. reliability work.

**Automation:** Leveraging CI/CD for reliable deployments and IaC for consistent infrastructure provisioning reduces manual toil and improves system stability.

## 8. Testing

**Decision:** Jest for unit tests, Cypress for End-to-End (E2E) tests, `terraform validate` and `tflint` for IaC validation.

**Reasoning:** These tools are industry standards, providing comprehensive testing capabilities across different layers of the application and infrastructure.

**Integration:** All tests will be integrated into the GitHub Actions CI pipeline to ensure automated quality checks on every code change.

## 9. Security

**Decision:** Implement standard web security best practices, leveraging AWS services.

**Key Security Measures:**
*   **HTTPS:** Enforced via ACM and CloudFront for all traffic.
*   **Content Security Policy (CSP):** To mitigate XSS and other injection attacks.
*   **Dependency Security Scanning:** Automated scanning of third-party libraries (e.g., via Dependabot or Snyk integration).
*   **Infrastructure Security:** Least privilege IAM roles for deployment, restricted S3 bucket policies, CloudFront OAI/OAC for S3 access.
*   **Security Headers:** Implementation of HTTP security headers (e.g., HSTS, X-Content-Type-Options).

## 10. Interfaces and Integration Details

*   **Git Repository (GitHub):** Central source of truth for all application code and IaC configurations.
*   **GitHub Actions:** Orchestrates the entire CI/CD workflow, interacting with AWS APIs.
*   **AWS APIs:** Terraform and AWS CLI commands (within GitHub Actions) interact directly with AWS services.
*   **CloudFront & S3:** Frontend static assets are served from S3 via CloudFront.
*   **Route 53:** Manages custom domain resolution, pointing to the CloudFront distribution.
*   **AWS CloudWatch:** Collects and stores monitoring metrics and logs from all AWS services.
*   **Grafana:** Connects to CloudWatch to pull metrics and display custom dashboards.
*   **Optional Backend (Lambda/API Gateway):** If implemented, integrates with the frontend via standard HTTP API calls.
