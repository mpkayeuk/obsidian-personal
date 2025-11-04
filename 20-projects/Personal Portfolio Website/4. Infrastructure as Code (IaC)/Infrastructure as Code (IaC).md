# Infrastructure as Code (IaC)

## Goal
To define and provision all necessary cloud infrastructure for the personal portfolio website using code. This ensures repeatability, consistency, version control, and automated deployment of the environment.

## Detailed Design Steps

### 4.1. Choose Cloud Provider
*   **Action:** Select a cloud platform for hosting the website.
*   **Decision:** Vercel.
*   **Reasoning:** Vercel is the ideal platform for Next.js applications, offering a seamless development and deployment experience.

### 4.2. Select IaC Tool
*   **Action:** Choose a tool to manage the infrastructure.
*   **Decision:** Terraform.
*   **Reasoning:** Terraform allows for the codification of Vercel resources, ensuring that the configuration is version-controlled and repeatable.

### 4.3. Define Core Infrastructure Components
*   **Action:** Identify and define the essential Vercel resources required for the website.
*   **Components:**
    *   **Vercel Project:** The main project configuration in Vercel.
    *   **Vercel Domain:** The custom domain for the website.
    *   **Vercel Environment Variables:** To store any secrets or configuration variables.

### 4.4. Write IaC Configuration
*   **Action:** Develop the Terraform configuration files (`.tf`) to provision the defined Vercel resources.
*   **Consideration:**
    *   **Modularity:** Organize code into reusable modules.
    *   **Variables:** Use variables for configurable parameters (e.g., domain name, project name).
    *   **State Management:** Configure remote state storage for collaboration and durability.

### 4.5. Implement Infrastructure Version Control
*   **Action:** Store IaC scripts in a version control system (e.g., Git).
*   **Consideration:** Treat infrastructure code like application code, with pull requests, reviews, and branching strategies.

### 4.6. Automate IaC Deployment
*   **Action:** Integrate IaC deployment into the CI/CD pipeline.
*   **Consideration:**
    *   **`terraform plan` / `pulumi preview`:** Run as part of CI to show proposed changes.
    *   **`terraform apply` / `pulumi up`:** Execute as part of CD for automated provisioning/updates.
    *   **Approval Gates:** Implement manual approval steps for critical infrastructure changes.
