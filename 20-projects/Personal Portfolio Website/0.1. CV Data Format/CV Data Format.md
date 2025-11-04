# CV Data Format

## Goal
To define a structured, Markdown-based format for CV data that allows for easy creation, version control, and parsing by the website to generate both generic and tailored CVs. The format will be based on the JSON Resume schema to leverage existing standards and best practices.

## 0.1.1. Chosen Standard: JSON Resume (Markdown Adaptation)
*   **Decision:** The CV data will be stored in Markdown files, structured to align with the JSON Resume schema.
*   **Reasoning:** JSON Resume provides a well-defined, developer-friendly schema for CV data. Adapting this to Markdown allows for human-readable and version-controllable CV files, while still enabling programmatic parsing and rendering.

## 0.1.2. Markdown Structure and Mapping to JSON Resume

The Markdown file will be structured using headings and lists to represent the different sections and fields of the JSON Resume schema.

### Top-Level Sections (JSON Resume equivalent)

*   **`# Basics` (basics):**
    *   `## Name`: `basics.name`
    *   `## Label`: `basics.label` (e.g., "DevOps Engineer")
    *   `## Email`: `basics.email`
    *   `## Phone`: `basics.phone`
    *   `## Website`: `basics.url`
    *   `## Summary`: `basics.summary`
    *   `## Location`: `basics.location` (sub-sections for address, postalCode, city, countryCode, region)
    *   `## Profiles`: `basics.profiles` (list of social media profiles with network, username, url)

*   **`# Work Experience` (work):**
    *   Each work entry will be a new `## [Company Name]` section.
    *   `### Position`: `work.position`
    *   `### Website`: `work.website`
    *   `### Start Date`: `work.startDate` (YYYY-MM-DD)
    *   `### End Date`: `work.endDate` (YYYY-MM-DD or "Present")
    *   `### Summary`: `work.summary`
    *   `### Highlights`: `work.highlights` (list of achievements/responsibilities)

*   **`# Education` (education):**
    *   Each education entry will be a new `## [Institution Name]` section.
    *   `### Study Type`: `education.studyType` (e.g., "Bachelor's Degree")
    *   `### Area`: `education.area` (e.g., "Computer Science")
    *   `### Start Date`: `education.startDate`
    *   `### End Date`: `education.endDate`
    *   `### GPA`: `education.gpa`
    *   `### Courses`: `education.courses` (list of relevant courses)

*   **`# Skills` (skills):**
    *   Each skill category will be a new `## [Category Name]` section (e.g., "Cloud Platforms", "IaC Tools").
    *   `### Keywords`: `skills.keywords` (list of specific skills within the category).
    *   `### Level`: `skills.level` (e.g., "Master", "Advanced", "Intermediate").

*   **`# Awards` (awards):**
    *   `## [Award Title]`: `awards.title`
    *   `### Date`: `awards.date`
    *   `### Awarder`: `awards.awarder`
    *   `### Summary`: `awards.summary`

*   **`# Publications` (publications):**
    *   `## [Publication Title]`: `publications.name`
    *   `### Publisher`: `publications.publisher`
    *   `### Date`: `publications.releaseDate`
    *   `### Website`: `publications.website`
    *   `### Summary`: `publications.summary`

*   **`# Interests` (interests):**
    *   `## [Interest Name]`: `interests.name`
    *   `### Keywords`: `interests.keywords`

*   **`# References` (references):**
    *   `## [Reference Name]`: `references.name`
    *   `### Reference`: `references.reference` (the actual reference text)

## 0.1.3. Tailoring Mechanism
*   **Action:** Define how specific sections or entries can be included/excluded or modified for tailored CVs.
*   **Consideration:**
    *   **Metadata/Frontmatter:** Use YAML frontmatter at the top of the Markdown file to specify job-specific tags or flags (e.g., `tags: [devops, sre]`, `include_project: [project-a, project-b]`).
    *   **Conditional Rendering:** The website's rendering logic will use this metadata to filter and display relevant content.

## 0.1.4. Example Markdown CV File

```markdown
---
job_title: "Senior DevOps Engineer"
target_company: "ExampleCorp"
include_skills: ["AWS", "Terraform", "Kubernetes"]
exclude_work_highlights: ["legacy-system-maintenance"]
---

# Basics
## Name
John Doe
## Label
DevOps & SRE Specialist
## Email
john.doe@example.com
## Phone
+1 (555) 123-4567
## Website
https://johndoe.dev
## Summary
Highly motivated DevOps and SRE specialist with 10+ years of experience in building and maintaining scalable, reliable, and secure cloud infrastructure. Passionate about automation, observability, and continuous improvement.
## Location
### Address
123 Main St
### City
Anytown
### Postal Code
12345
### Country Code
US
### Region
State
## Profiles
*   Network: LinkedIn
    Username: johndoe
    URL: https://linkedin.com/in/johndoe
*   Network: GitHub
    Username: johndoe
    URL: https://github.com/johndoe

# Work Experience
## ExampleCorp
### Position
Senior DevOps Engineer
### Website
https://examplecorp.com
### Start Date
2020-01-01
### End Date
Present
### Summary
Led a team responsible for the design, implementation, and maintenance of cloud infrastructure on AWS. Implemented CI/CD pipelines for multiple microservices.
### Highlights
*   Designed and implemented a highly available Kubernetes cluster on AWS EKS using Terraform.
*   Reduced deployment time by 50% through automation of CI/CD pipelines with GitHub Actions.
*   Established comprehensive monitoring and alerting solutions using Prometheus and Grafana.

## Previous Company
### Position
Cloud Engineer
### Website
https://previouscompany.com
### Start Date
2015-06-01
### End Date
2019-12-31
### Summary
Managed cloud resources and supported development teams in deploying applications.
### Highlights
*   Migrated on-premise applications to AWS EC2 and S3.
*   Developed automation scripts for routine operational tasks.

# Education
## University of Technology
### Study Type
Master's Degree
### Area
Computer Science
### Start Date
2013-09-01
### End Date
2015-06-01
### GPA
3.8
### Courses
*   Advanced Cloud Computing
*   Distributed Systems
*   Software Architecture

# Skills
## Cloud Platforms
### Keywords
AWS, GCP, Azure
### Level
Master
## IaC Tools
### Keywords
Terraform, Pulumi, CloudFormation
### Level
Advanced
## CI/CD
### Keywords
GitHub Actions, GitLab CI, Jenkins, Argo CD
### Level
Master
## Monitoring
### Keywords
Prometheus, Grafana, ELK Stack, CloudWatch
### Level
Advanced
## Languages
### Keywords
Python, Go, JavaScript, Bash
### Level
Advanced
