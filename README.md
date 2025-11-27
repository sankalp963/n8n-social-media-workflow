# 🚀 n8n Social Media Automation Workflow

Automate social media post creation, design, and distribution using Airtable, Replicate (Flux3), Placid, and Google Drive—all orchestrated in n8n.

---

## 📚 Table of Contents

- [Introduction](#-introduction)
- [Tech Stack](#️-tech-stack)
- [Features](#-features)
- [Workflow Steps](#-workflow-steps)
- [Setup Instructions](#-setup-instructions)
- [Usage](#-usage)
- [Support & Contribution](#-support--contribution)

---

## ✨ Introduction

This workflow manages the journey of your social media post: from capturing ideas in Airtable, through AI-powered asset creation, branding with Placid, secure cloud upload, and sharing via Google Drive.  
Each step includes guidance and a screenshot for effortless setup.

---

## ⚙️ Tech Stack

- **n8n** – Visual workflow automation  
- **Airtable** – Database for post records  
- **Replicate (Flux3 API)** – AI creative asset generation  
- **Placid** – Branded social media template creation  
- **Google Drive** – Asset storage and sharing  

---

## 🔋 Features

- Detect and process new or updated post content instantly  
- Generate images/content using AI  
- Apply branded templates for posts  
- Centralize asset storage and secure sharing  
- Modular, extensible workflow logic  

---

## 🔁 Workflow Steps

### 1. Airtable Trigger

**Purpose**  
Detects new or updated Airtable records using the “last modified time” field. Starts the workflow for each change.

**Configuration Notes**  
- Set the Airtable base ID, table ID, and trigger field.

![Airtable Trigger Node](https://github.com/sankalp963/n8n-social-media-workflow/blob/a7ca0af0148b4fa61521c0eb82fd4a005e3198a0/images/airtable%20trigger.png)


---

### 2. Update Airtable Record

**Purpose**  
Updates the Airtable record status to `"scheduled"` and passes required fields (text, prompt, platform, assets) to the rest of the workflow.

**Configuration Notes**  
- Map the Status field and relevant metadata needed for downstream nodes.

![Update Airtable Record Node]()


---

### 3. Generate Creative Asset (Flux3 API)

**Purpose**  
Sends a POST request to Flux3 with the prompt and specifications to generate creative assets.

**Configuration Notes**  
- Request body: `prompt`, `width`, `height`.  
- Headers: `Accept` and `Content-Type` (keep API keys redacted or omitted in screenshots).

![Flux3 HTTP Request Node]()


---

### 4. Wait Node

**Purpose**  
Delays the workflow to allow time for asset generation or to support scheduled posting.

**Configuration Notes**  
- Configure time-based or event-based waiting as needed.

![Wait Node]()


---

### 5. Poll for Asset Completion (HTTP Request)

**Purpose**  
Polls the Flux3 endpoint to check job completion and fetches the creative asset when it’s ready.

**Configuration Notes**  
- Use the poll URL returned from the Flux3 job response.

![Poll Flux3 Result Node]()


---

### 6. Download Asset

**Purpose**  
Fetches the generated creative file so it can be used inside the template.

**Configuration Notes**  
- Download from the result URL.  
- Set the response format to `file`.

![Download Asset Node]()


---

### 7. Create Social Media Template (Placid API)

**Purpose**  
Sends the generated asset, logo, and footer text to Placid for branded social template creation.

**Configuration Notes**  
- Configure template layers: background image, logo, footer text.  
- Map inputs from Airtable fields and previous nodes.

![Placid API Node]()


---

### 8. Download Formatted Asset

**Purpose**  
Retrieves the finished template output (image/card) from Placid.

**Configuration Notes**  
- Map the output URL from Placid.  
- Set response type to `file`.

![Download Placid Result Node]()


---

### 9. Upload to Google Drive

**Purpose**  
Uploads the formatted asset to a designated Google Drive folder for storage and sharing.

**Configuration Notes**  
- Set destination folder ID.  
- Generate the file name dynamically (e.g., based on post title or timestamp).

![Google Drive Upload Node]()


---

### 10. Edit/Set Metadata Fields

**Purpose**  
Updates or sets metadata such as public share links before final distribution.

**Configuration Notes**  
- Use a Set/Edit Fields node to build a clean output object (e.g., `publicUrl`, `fileName`, `platforms`).

![Set/Edit Fields Node]()


---

### 11. Share File (Google Drive)

**Purpose**  
Makes the file publicly accessible and retrieves a share link for publishing.

**Configuration Notes**  
- Configure permissions so “anyone with the link” can view.  
- Map the resulting share URL for use in publishing or notifications.

![Google Drive Share Node]()


---

## ⚡ Setup Instructions

1. Clone this repository and open it in your IDE.  
2. Import `social-media-automation.json` into the n8n Editor.  
3. Configure credentials for Airtable, Replicate (Flux3), Placid, and Google Drive inside n8n.  
4. Use the provided screenshots as a reference for each node configuration.

---

## 📖 Usage

1. Activate the workflow in n8n.  
2. Add or update records in Airtable for new posts.  
3. The workflow will automatically generate assets, apply templates, upload files, and produce shareable links.

---

## 🤝 Support & Contribution

- Open GitHub issues for bugs or feature requests.  
- Submit pull requests with improvements to the workflow or documentation.  
- Share this workflow with the n8n community or contribute new templates built on top of it.  

---
