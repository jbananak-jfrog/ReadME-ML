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
  /* --- CONTAINER --- */
  .css-tabs {
    width: 100%;
    font-family: system-ui, -apple-system, sans-serif;
  }

  /* --- HIDE RADIO INPUTS --- */
  /* These control the logic but are invisible to the user */
  .css-tabs input[type="radio"] {
    display: none;
  }

  /* --- LABEL ROW (The Buttons) --- */
  .tab-labels {
    display: flex;
    gap: 10px;
    width: 100%;
    margin-bottom: 0;
  }

  /* --- BUTTON STYLING --- */
  .tab-label {
    flex: 1;
    background: #ffffff;
    border: 1px solid #e1e4e8;
    border-radius: 6px;
    padding: 15px 5px;
    cursor: pointer;
    font-size: 13px;
    font-weight: 600;
    color: #333;
    text-align: center;
    transition: all 0.2s ease;
    
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 60px;
    position: relative;
  }

  .tab-label:hover {
    background-color: #f6f8fa;
    border-color: #0366d6;
  }

  /* --- CONTENT BOXES --- */
  .tab-content {
    display: none; /* Hidden by default */
    width: 100%;
    background: #fff;
    border: 1px solid #e1e4e8;
    border-radius: 6px;
    padding: 30px;
    margin-top: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
    animation: fadeIn 0.3s ease;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(5px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* --- THE LOGIC (Magic CSS) --- */
  
  /* 1. When Radio 1 is checked, turn Label 1 dark blue */
  #tab-1:checked ~ .tab-labels label[for="tab-1"],
  #tab-2:checked ~ .tab-labels label[for="tab-2"],
  #tab-3:checked ~ .tab-labels label[for="tab-3"],
  #tab-4:checked ~ .tab-labels label[for="tab-4"],
  #tab-5:checked ~ .tab-labels label[for="tab-5"] {
    background-color: #2f3747;
    color: white;
    border-color: #2f3747;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  }

  /* 2. Add the little triangle pointer to the active label */
  #tab-1:checked ~ .tab-labels label[for="tab-1"]::after,
  #tab-2:checked ~ .tab-labels label[for="tab-2"]::after,
  #tab-3:checked ~ .tab-labels label[for="tab-3"]::after,
  #tab-4:checked ~ .tab-labels label[for="tab-4"]::after,
  #tab-5:checked ~ .tab-labels label[for="tab-5"]::after {
    content: "";
    position: absolute;
    bottom: -8px;
    left: 50%;
    transform: translateX(-50%);
    border-width: 8px 8px 0;
    border-style: solid;
    border-color: #2f3747 transparent transparent transparent;
    z-index: 10;
  }

  /* 3. Show the corresponding content when radio is checked */
  #tab-1:checked ~ .content-1,
  #tab-2:checked ~ .content-2,
  #tab-3:checked ~ .content-3,
  #tab-4:checked ~ .content-4,
  #tab-5:checked ~ .content-5 {
    display: block;
  }

  /* Mobile Stack */
  @media (max-width: 768px) {
    .tab-labels { flex-direction: column; }
    .tab-label::after { display: none !important; }
  }
</style>

<div class="css-tabs">
  <input type="radio" name="jfrog-tabs" id="tab-1" checked>
  <input type="radio" name="jfrog-tabs" id="tab-2">
  <input type="radio" name="jfrog-tabs" id="tab-3">
  <input type="radio" name="jfrog-tabs" id="tab-4">
  <input type="radio" name="jfrog-tabs" id="tab-5">

  <div class="tab-labels">
    <label for="tab-1" class="tab-label">Detect AI Usage +</label>
    <label for="tab-2" class="tab-label">Centralize & Govern +</label>
    <label for="tab-3" class="tab-label">Build & Deploy +</label>
    <label for="tab-4" class="tab-label">Monitor Performance +</label>
    <label for="tab-5" class="tab-label">Turn Data Into Features +</label>
  </div>

  <div class="tab-content content-1">
    <h3 style="margin-top:0;">Gain Full AI Visibility (Detect)</h3>
    <p>You cannot govern what you cannot see. JFrog automatically scans your repositories and builds to uncover every existing AI model and external API currently in your Artifactory. By revealing "Shadow AI", you can assess immediate risks and establish a clean, trusted baseline for your AI operations journey.</p>
  </div>

  <div class="tab-content content-2">
    <h3 style="margin-top:0;">Your Single Source of Truth for AI</h3>
    <p>Unify every AI asset, including commercial APIs (like OpenAI), open-source models (like Hugging Face), and MCP servers, into one secure, centralized hub. Provide developers with self-service access to approved tools while ensuring strict security and compliance.</p>
  </div>

  <div class="tab-content content-3">
    <h3 style="margin-top:0;">From Notebook to Production</h3>
   <p>Bridge the gap between experimentation and production with a simplified workflow to log, build, and deploy your custom models. By automating the transition from code to a production-ready artifact, you ensure reproducibility without the usual infrastructure headaches.</p>
  </div>

  <div class="tab-content content-4">
    <h3 style="margin-top:0;">Maintain Trust in Live Models</h3>
    <p>Models degrade over time as real-world data changes. JFrog tracks real-time model health and automatically detects data drift. By monitoring live traffic against your training baseline, you ensure your AI remains accurate and trustworthy without constant manual checking.</p>
  </div>

  <div class="tab-content content-5">
    <h3 style="margin-top:0;">Accelerate Feature Management</h3>
    <p>Simplify the data preparation process by transforming raw data into a centralized library of governed features. By defining your data logic once using simple SQL, you ensure the exact same features used for training are available for production, eliminating costly data mismatch bugs.</p>
  </div>

</div>
`}</HTMLBlock>

<br />

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
