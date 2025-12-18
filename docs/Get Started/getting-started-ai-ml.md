---
title: Getting Started with AI/ML in JFrog
excerpt: This page will help you get started with JFrog's ML and AI assets management.
hidden: false
link:
  new_tab: false
next:
  pages:
    - slug: jfrog-ai-catalog
      title: JFrog AI Catalog Overview
      type: basic
    - slug: get-started-with-jfrog-ml
      title: Get Started with JFrog ML
      type: basic
---
JFrog ML is a unified platform designed to streamline the entire machine learning lifecycle by integrating MLOps, security, and DevOps into a single system of record. It provides a robust, scalable, and secure foundation for building, deploying, and monitoring your models in production.

***

<Image align="center" border={true} src="https://files.readme.io/24416c14ed7f479f1e433d68050b3ff4e7a64b08318759636b35055ec0cec97c-AI_ML_Diagram_for_JFrog_ML.png" className="border" />

You're looking at a map of how JFrog can help you govern and manage all your AI and ML assets. Read on to understand how JFrog secures your system, and helps prevent the entry of malicious or unvetted assets into your software environment.

<HTMLBlock>{`
<style>
  /* --- CONTAINER --- */
  .css-tabs {
    width: 100%;
    font-family: system-ui, -apple-system, sans-serif;
  }

  /* --- HIDE RADIO INPUTS (The Logic) --- */
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

  /* --- INDIVIDUAL BUTTON STYLING --- */
  .tab-label {
    flex: 1;
    background: #ffffff;
    border: 1px solid #e1e4e8;
    border-radius: 6px;
    padding: 12px 5px;
    cursor: pointer;
    font-size: 13px;
    color: #333;
    text-align: center;
    transition: all 0.2s ease;
    
    /* Stack Title and Arrow Vertically */
    display: flex;
    flex-direction: column; 
    align-items: center;
    justify-content: center;
    gap: 6px; /* Space between Title and Arrow */
    
    min-height: 70px;
    position: relative;
    line-height: 1.2; 
  }

  .tab-label:hover {
    background-color: #f6f8fa;
    border-color: #0366d6;
  }

  /* --- ARROW STYLING --- */
  .tab-arrow {
    font-size: 12px;     /* Size of the arrow */
    color: #888;         /* Grey color by default */
    transition: transform 0.3s ease; /* Smooth rotation animation */
    display: block;      /* Ensures it sits on its own line */
    line-height: 1;
  }

  /* --- ACTIVE STATE STYLING --- */
  
  /* 1. Turn the active label dark blue */
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

  /* 2. Rotate the Arrow UP and turn it White when active */
  #tab-1:checked ~ .tab-labels label[for="tab-1"] .tab-arrow,
  #tab-2:checked ~ .tab-labels label[for="tab-2"] .tab-arrow,
  #tab-3:checked ~ .tab-labels label[for="tab-3"] .tab-arrow,
  #tab-4:checked ~ .tab-labels label[for="tab-4"] .tab-arrow,
  #tab-5:checked ~ .tab-labels label[for="tab-5"] .tab-arrow {
    transform: rotate(180deg); /* Flip upside down */
    color: #ffffff;            /* Turn white */
  }

  /* 3. Add the triangle pointer at the bottom */
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

  /* 4. Show the corresponding content block */
  #tab-1:checked ~ .content-1,
  #tab-2:checked ~ .content-2,
  #tab-3:checked ~ .content-3,
  #tab-4:checked ~ .content-4,
  #tab-5:checked ~ .content-5 {
    display: block;
  }

  /* --- CONTENT BOX STYLING --- */
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
    
    <label for="tab-1" class="tab-label">
      <strong>Detect AI Usage</strong>
      <span class="tab-arrow">&#9660;</span>
    </label>
    
    <label for="tab-2" class="tab-label">
      <strong>Centralize & Govern</strong>
      <span class="tab-arrow">&#9660;</span>
    </label>
    
    <label for="tab-3" class="tab-label">
      <strong>Build & Deploy</strong>
      <span class="tab-arrow">&#9660;</span>
    </label>
    
    <label for="tab-4" class="tab-label">
      <strong>Monitor Performance</strong>
      <span class="tab-arrow">&#9660;</span>
    </label>
    
    <label for="tab-5" class="tab-label">
      <strong>Turn Data Into Features</strong>
      <span class="tab-arrow">&#9660;</span>
    </label>
    
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
<script>
  document.addEventListener("DOMContentLoaded", function() {
    var firstTab = document.getElementById("tab-1");
    if (firstTab) {
      firstTab.checked = true;
    }
  });
</script>
`}</HTMLBlock>

***

## 🐸 Where to Start?

Start by searching for what you need - your JFrog AI ML guide walks you through key concepts, tutorials, or best practices. Either use the search bar or select from the options below.

<div class="green-shadow-cards">
  <Cards columns={2}>
    <Card title="Setting Up JFrog ML" icon="fa-star" href="https://jfrog-enterprise-group.readme.io/ai-ml/docs/setting-up-jfrog-ml" target="_self">
      <small>*How to set up your JFrog ML*</small>
    </Card>

    <Card title="AI Catalog" icon="fa-star" href="https://jfrog-enterprise-group.readme.io/ai-ml/docs/jfrog-ai-catalog" target="_self">
      (Including Shadow AI Detection)<br />
      <small>*Manage your AI Assets in a centralized hub for AI model discovery, governance, and deployment*</small>
    </Card>
  </Cards>
</div>

<br />
