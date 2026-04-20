# Hosting a Static Website on Azure Storage with CDN Integration

**Created by:** Siddhant Bisht  
**Created on:** 03 August 2025

---

## Overview

This project demonstrates how to host a static website using Azure Blob Storage with CDN (Content Delivery Network) integration via Azure Front Door & CDN, and optionally attach a custom domain with HTTPS.

---

## Steps Performed

### 1. Create an Azure Storage Account
Create a new Azure Storage Account from the Azure Portal.

### 2. Enable Static Website Hosting
Locate the **Static Website** option in the left pane of the Storage Account and enable it.

### 3. Prepare Your Website Files
Prepare your HTML, CSS, and other static files. Ensure all files (including `style.css`) are in the **same folder/location** — not in subfolders — because Azure Blob containers allow uploading files, not entire nested folder hierarchies directly.

> **Note:** An error was initially made by placing `style.css` in a subfolder. Make sure all files are at the same level before uploading.

A custom **error page** (e.g., `404.html`) was also created so that instead of a generic or technical error like "404 Not Found", users see a branded, friendly page. Benefits of a custom error page:
- Keeps users on the site longer
- Allows adding internal links (home, blog, contact) to guide users back
- Reduces bounce rate
- Improves search ranking

### 4. Upload Files to the `$web` Container
After enabling static website hosting, Azure automatically creates a special Blob container called **`$web`**. Upload all your website files into this container.

### 5. Access the Website via Primary Endpoint
Once the upload is complete, navigate to **Static Website** in the left pane of the Storage Account. You will see a **Primary Endpoint URL**. Copy this URL and open it in a browser to view your hosted website.

### 6. Add a CDN Endpoint via Azure Front Door & CDN
Navigate to **Front Door & CDN** in the Storage Account. Create a CDN endpoint for your static website.

**Why CDN?**  
CDN copies the website's static files (HTML, CSS, JS, images) to **edge servers located around the world**. When a user visits the website, content is served from the **nearest CDN location** rather than from the origin Storage Account region. This:
- Reduces latency
- Results in faster load times for global users

Copy the CDN endpoint URL. The website will be accessible from this endpoint URL as well.

### 7. Add a Custom Domain (Optional)
You can attach a **custom domain** to your CDN endpoint to make the website look more professional and SEO-friendly.

Steps:
1. Navigate to your CDN endpoint.
2. Find the **Custom Domain** option.
3. Add your custom domain name.
4. In your **DNS provider**, add a **CNAME record** pointing your custom domain to the CDN endpoint URL.
5. Enable HTTPS — you can either:
   - Upload and use a **custom SSL certificate**, or
   - Use **Azure-managed HTTPS** (automatically provisioned by Azure)

Your website will then be accessible from your custom domain.

---

## Architecture

```
User Request
    |
    v
[Custom Domain (DNS CNAME)]
    |
    v
[Azure Front Door & CDN - Edge Servers Worldwide]
    |
    v
[Azure Blob Storage - $web Container]
    |
    +--> index.html
    +--> style.css
    +--> error.html
    +--> (other static assets)
```

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Azure Blob Storage | Stores and hosts static website files |
| Azure Static Website Hosting | Serves files from the `$web` container via a public endpoint |
| Azure Front Door & CDN | Distributes content globally via edge servers for low latency |
| Custom Domain + SSL/HTTPS | Provides a branded, secure URL for the website |
| HTML / CSS / JS | Static website files |

---

## Prerequisites

- An active **Azure Subscription**
- A **Storage Account** created in Azure Portal
- Static website files (HTML, CSS, JS, images) ready for upload
- (Optional) A registered **custom domain** with access to its DNS settings

---

## Configuration Details

- **Static Website Hosting:** Enabled from the Storage Account left pane
- **Index document name:** Typically `index.html`
- **Error document path:** Custom error page (e.g., `error.html`) for friendly 404 responses
- **Container used:** `$web` (auto-created by Azure on enabling static website hosting)
- **CDN:** Configured via **Front Door & CDN** section within the Storage Account
- **Custom Domain DNS:** CNAME record pointing custom domain → CDN endpoint URL
- **HTTPS:** Azure-managed or custom SSL certificate via CDN endpoint

---

## Reference Links

- [Azure Static Website Documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)
- [GitHub - Siddhantbisht9](https://github.com/Siddhantbisht9)
