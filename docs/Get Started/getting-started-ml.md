---
title: Getting Started with AI/ML (MLOps)
excerpt: This page will help you get started with Machine Learning (MLOps).
hidden: false
link:
  new_tab: false
---
# Welcome to JFrog AI ML

<Image align="center" border={true} src="https://files.readme.io/24416c14ed7f479f1e433d68050b3ff4e7a64b08318759636b35055ec0cec97c-AI_ML_Diagram_for_JFrog_ML.png" className="border" />

You're looking at a map of how JFrog can help you govern and manage all your AI and ML assets. Read on to understand how JFrog secures your system, and helps prevent the entry of malicious or ??? assets into your software environment.

***

## ✍️ How Does it Work?

<HTMLBlock>{`
<style>
  /* 1. The Container: Forces 5 equal columns */
  .full-width-accordion {
    display: grid;
    grid-template-columns: repeat(5, 1fr); 
    gap: 10px;
    margin-bottom: 20px;
    width: 100%;
  }

  /* 2. CRITICAL: This command forces the 'details' tag to disappear 
     so the Header and Content become direct parts of the grid. 
     We use !important to ensure ReadMe doesn't override it. */
  .full-width-accordion details {
    display: contents !important;
  }

  /* 3. The Card Headers (Summaries) */
  .full-width-accordion summary {
    grid-row: 1;          /* Keep all headers on the top row */
    cursor: pointer;
    
    /* Box Styling */
    background: #ffffff;
    border: 1px solid #e1e4e8;
    border-radius: 6px;
    padding: 10px;
    text-align: center;
    font-weight: 600;
    font-size: 13px;
    list-style: none;     /* Hide triangle */
    
    /* Centering Text */
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 60px;     
    transition: all 0.2s ease-in-out;
  }

  /* Hover State */
  .full-width-accordion summary:hover {
    background: #f6f8fa;
    border-color: #0366d6;
  }

  /* 4. ACTIVE STATE (Dark Blue Highlight) */
  .full-width-accordion details[open] summary {
    background-color: #2f3747 !important;
    color: #ffffff !important;
    border-color: #2f3747 !important;
    box-shadow: 0 4px 6px rgba(0,0,0,0.2);
    position: relative;
    z-index: 10;          /* Bring to front */
  }
  
  /* Add a tiny pointer triangle at the bottom of the active card */
  .full-width-accordion details[open] summary::after {
    content: "";
    position: absolute;
    bottom: -6px;
    left: 50%;
    margin-left: -6px;
    border-width: 6px 6px 0;
    border-style: solid;
    border-color: #2f3747 transparent transparent transparent;
  }

  /* Remove default browser marker */
  .full-width-accordion summary::-webkit-details-marker { display: none; }

  /* 5. The Content Box (Full Width) */
  .full-width-accordion .accordion-content {
    grid-row: 2;           /* Force to the second row */
    grid-column: 1 / -1;   /* SPAN ALL 5 COLUMNS (Full Width) */
    
    background: #fff;
    border: 1px solid #e1e4e8;
    border-radius: 6px;
    padding: 30px;
    margin-top: 5px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    
    /* Ensure text readability */
    text-align: left;
    line-height: 1.6;
    width: 100%;           /* Force full width */
  }

  /* Mobile Responsive: Stack them if screen is small */
  @media (max-width: 768px) {
    .full-width-accordion {
      display: flex;
      flex-direction: column;
    }
    .full-width-accordion summary {
      min-height: 50px;
    }
  }
</style>

<div class="full-width-accordion">

  <details name="jfrog-ai">
    <summary>Detect AI Usage</summary>
    <div class="accordion-content">
      <h3 style="margin-top:0;">Gain Full AI Visibility</h3>
      <p>You cannot govern what you cannot see. JFrog automatically scans your repositories and builds to uncover every existing AI model and external API currently in your Artifactory. By revealing "Shadow AI", you can assess immediate risks and establish a clean, trusted baseline for your AI operations journey.</p>
    </div>
  </details>

  <details name="jfrog-ai">
    <summary>Centralize & Govern</summary>
    <div class="accordion-content">
      <h3 style="margin-top:0;">Centralize & Govern AI Assets</h3>
      <p>Gain Full AI Visibility. You cannot govern what you cannot see. JFrog automatically scans your repositories and builds to uncover every existing AI model and external API currently in your Artifactory.</p>
    </div>
  </details>

  <details name="jfrog-ai">
    <summary>Build & Deploy</summary>
    <div class="accordion-content">
      <h3 style="margin-top:0;">Build & Deploy Models</h3>
      <p>Gain Full AI Visibility. You cannot govern what you cannot see. JFrog automatically scans your repositories and builds to uncover every existing AI model and external API currently in your Artifactory.</p>
    </div>
  </details>

  <details name="jfrog-ai">
    <summary>Monitor Performance</summary>
    <div class="accordion-content">
      <h3 style="margin-top:0;">Monitor Model Performance</h3>
      <p>Gain Full AI Visibility. You cannot govern what you cannot see. JFrog automatically scans your repositories and builds to uncover every existing AI model and external API currently in your Artifactory.</p>
    </div>
  </details>

  <details name="jfrog-ai">
    <summary>Turn Data Into Features</summary>
    <div class="accordion-content">
      <h3 style="margin-top:0;">Turn Data Into Features</h3>
      <p>Gain Full AI Visibility. You cannot govern what you cannot see. JFrog automatically scans your repositories and builds to uncover every existing AI model and external API currently in your Artifactory.</p>
    </div>
  </details>

</div>
`}</HTMLBlock>

<br />



Start by creating <Anchor label="**Guides**" target="_blank" href="https://docs.readme.com/main/docs/creating-and-managing-guides">**Guides**</Anchor> - your API's instruction manual where you can walk users through key concepts, tutorials, or best practices.

With ReadMe's MDX editor, you can combine Markdown and custom JSX components like `<Card>`, `<Tab>`, and `<Accordion>` for richer content and better structure.

You can even [build your own custom **Components**](/docs/getting-started#/settings/custom-components/start) to reuse across your docs.

<Cards columns={3}>
  <Card title="Explore the Component Marketplace" href="https://github.com/readmeio/marketplace/tree/main/components" icon="fa-store" target="_blank">
    Drop in and customize components.
  </Card>

  <Card title="MDX (Markdown + JSX)" href="https://docs.readme.com/main/docs/mdx" icon="fa-code">
    Learn more about MDX to build interactive components.
  </Card>

  <Card title="Custom MDX Components" href="https://docs.readme.com/main/docs/building-custom-mdx-components" icon="fa-wrench">
    Build your own components to reuse anywhere.
  </Card>
</Cards>

Looking for a branded entry point? Enable a **<Anchor label="Landing Page" target="_blank" href="https://docs.readme.com/main/docs/landing-page">Landing Page</Anchor>** to welcome your developers and direct them to key docs.

***

## 🤖 Add AI to Your Dev Hub

AI is built into ReadMe to help you and your users move faster. Slide the panel open by hitting **:sparkles:AI** in your top navigation bar.

* **AI Agent**  
  Our built-in AI agent is your sidekick for drafting documentation, translating pages, and applying style guides.

* **MCP Server**  
  Generate an **MCP** server to convert your API documentation into a structured resource that AI assistants can understand and interact with programmatically.

* **AI-Powered Search**  
  Enable AI Search to help developers ask questions about your product and instantly receive an answer.

* **Open in Other AI Services**  
  Let your developers open your docs in tools like ChatGPT, Claude, or other LLMs, using context from your API and `llms.txt` configuration.

***

## 🌿 Edit, Preview, and Publish in Branches

<Anchor label="Branches" target="_blank" href="https://docs.readme.com/main/docs/branches">Branches</Anchor> bring Git-style workflows to your documentation process. Use them to:

* Draft changes across multiple pages without publishing immediately
* Review and preview updates before they go live
* Share edits with teammates for feedback
* Safely iterate without disrupting your production docs

You can create a branch directly from the Versions & Branches menu, save edits into a new branch, or sync with GitHub and automatically reflect branches across both platforms.

Once you're ready, merge the branch back into a live version. Enterprise plans can control who has merge access, and you'll always get a conflict check before merging.

***

## 🔁 Sync with Git

Whether you write docs in ReadMe's editor or your local dev environment, ReadMe fits into your workflow:

* **[Bi-Directional Sync with GitHub](https://docs.readme.com/main/docs/bi-directional-sync)**  
  Connect a GitHub repo to your project and work in branches. Changes in Git or ReadMe will stay in sync—perfect for staging and code review workflows.

* **Sync Your OpenAPI Files**  
  Use [`rdme`](https://docs.readme.com/docs/rdme#upload) or the ReadMe API to push your OpenAPI spec and keep your API Reference up to date automatically.

***

## 👀 Understand Your Developers

Want to know how developers are actually using your API and docs?

* **<Anchor label="My Developers" target="_blank" href="https://docs.readme.com/main/docs/developer-dashboard">My Developers</Anchor>** gives you real-time visibility into who is visiting your docs, what endpoints they're using, and where they're getting stuck.
* Segment usage by key users or cohorts to monitor engagement and spot issues before they turn into support tickets.
* To set up My Developers, you'll first authenticate logged-in users with the <Anchor label="Personalized Docs Webhook" target="_blank" href="https://docs.readme.com/main/docs/personalized-docs-webhook">Personalized Docs Webhook</Anchor>, then integrate the [Metrics SDK](https://docs.readme.com/main/docs/sending-api-logs) to send API logs to ReadMe.

***

## 🚀 Next Steps

* Start writing: create your first Guide or sync in your OAS file.
* [Connect GitHub for bi-directional sync](/docs/getting-started#/settings/git-connection/github).
* Set up your <Anchor label="MCP server" target="_blank" href="https://docs.readme.com/main/docs/mcp-servers">MCP server</Anchor>.
* Visit **My Developers** in the top navigation bar to start understanding your audience.

***

<Callout icon="💼" theme="default">
  Need support for multiple products, advanced permissions, or deeper customization?  
  Our enterprise features are built for teams managing business-critical APIs at scale. [Let's talk.](mailto:growth@readme.io)
</Callout>

## 💬 Need Help?

Our team is here to support you. If you get stuck, [email us](mailto:support@readme.io) or open the Intercom widget on any page to chat with someone from our team. We've also got a <Anchor label="Slack community" target="_blank" href="https://readme.com/slack">Slack community</Anchor> if you want to say hi to the team and connect with other ReadMe users!
