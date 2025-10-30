# Infrastructure as Code (IaC)

## Goal
To define and provision all necessary cloud infrastructure for the personal portfolio website using code. This ensures repeatability, consistency, version control, and automated deployment of the environment.

## Detailed Design Steps

### 4.1. Choose Cloud Provider
*   **Action:** Select a cloud platform for hosting the website.
*   **Consideration:**
    *   **AWS:** Amazon Web Services (S3 for static hosting, CloudFront for CDN, Route 53 for DNS).
    *   **GCP:** Google Cloud Platform (Cloud Storage, Cloud CDN, Cloud DNS).
    *   **Azure:** Microsoft Azure (Blob Storage, Azure CDN, Azure DNS).
    *   **Other:** Vercel, Netlify (often simpler for static sites).

### 4.2. Select IaC Tool
*   **Action:** Choose a tool to manage the infrastructure.
*   **Consideration:**
    *   **Terraform:** Cloud-agnostic, widely adopted, declarative.
    *   **Pulumi:** Uses general-purpose programming languages (Python, TypeScript, Go, C#).
    *   **Cloud-specific tools:** AWS CloudFormation, Azure Resource Manager, Google Cloud Deployment Manager.

### 4.3. Define Core Infrastructure Components
*   **Action:** Identify and define the essential cloud resources required for the website.
*   **Consideration (for a static site example):**
    *   **Storage:** S3 bucket (AWS) or Cloud Storage bucket (GCP) for hosting static files.
    *   **Content Delivery Network (CDN):** CloudFront (AWS) or Cloud CDN (GCP) for global distribution, caching, and improved performance/security.
    *   **DNS Management:** Route 53 (AWS) or Cloud DNS (GCP) for custom domain mapping.
    *   **SSL/TLS Certificate:** AWS Certificate Manager or Google-managed SSL certificates for HTTPS.

### 4.4. Write IaC Configuration
*   **Action:** Develop the IaC scripts (e.g., Terraform `.tf` files, Pulumi programs) to provision the defined infrastructure.
*   **Consideration:**
    *   **Modularity:** Organize code into reusable modules.
    *   **Variables:** Use variables for configurable parameters (e.g., domain name, bucket names).
    *   **State Management:** Configure remote state storage (e.g., S3 backend for Terraform) for collaboration and durability.

### 4.5. Implement Infrastructure Version Control
*   **Action:** Store IaC scripts in a version control system (e.g., Git).
*   **Consideration:** Treat infrastructure code like application code, with pull requests, reviews, and branching strategies.

### 4.6. Automate IaC Deployment
*   **Action:** Integrate IaC deployment into the CI/CD pipeline.
*   **Consideration:**
    *   **`terraform plan` / `pulumi preview`:** Run as part of CI to show proposed changes.
    *   **`terraform apply` / `pulumi up`:** Execute as part of CD for automated provisioning/updates.
    *   **Approval Gates:** Implement manual approval steps for critical infrastructure changes.
