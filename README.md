# Agentic-AI-Building-Blocks-Project
### Agentic AI Building Blocks using Amazon Bedrock

Agentic AI weather assistant, powered by Amazon Bedrock, that orchestrates API calls to retrieve real-time weather data and generates clear, human-friendly forecast summaries.

Agentic AI evolves beyond static AI "encyclopedias" into systems that think, plan, and act on complex challenges. This article unpacks its core elements through a Bedrock-powered weather assistant built with Python

#### What is Agentic AI?:
Agentic AI refers to advanced AI systems that operate autonomously to achieve goals, going beyond simple responses by perceiving environments, reasoning through problems, planning actions, and learning from outcomes. Unlike traditional AI, which reacts to prompts, agentic AI proactively handles multi-step tasks with minimal human input.Agentic AI shifts from "Question → Answer" to a dynamic "**Problem → Plan → Action → Result**" cycle. Key sources identify three essential traits defining true agentic systems:

**Autonomy**<br/>Agentic AI makes independent decisions and executes workflows without constant oversight, selecting tools and adapting strategies dynamically. This enables it to manage complex processes like diagnostics or optimizations solo

**Reactivity**<br/>These systems respond to real-time changes by interpreting new data, self-correcting errors, and refining plans iteratively. They maintain context awareness to handle dynamic environments effectively.

**Proactivity**<br/>Agentic AI anticipates needs, initiates actions toward objectives, and orchestrates multi-step sequences without prompts. It breaks down high-level goals into executable steps, often across systems

### Our Weather Assistant Agent Example
Today, we're building an agent showcasing all three key characteristics:

**The Challenge:** Users need weather info, but:
* Data requires coordinates and specific APIs<br/>
* Locations demand varied lookup strategies
* Raw responses are technical and complex

**Our Agent's Solution** :
* Understands any location format (city, ZIP, coordinates)
* Maps to precise latitude/longitude
* Generates National Weather Service API calls
* Fetches real-time forecasts
* Processes JSON into readable summaries
* Presents user-readable friendly results

### Why Amazon Bedrock for Agentic AI?
Amazon Bedrock is a fully managed service ideal for building agentic AI due to its serverless architecture, multi-model support, and enterprise features tailored for autonomous agents
* Multiple AI Models: Access Claude, Nova, Llama, and other leading models<br/>
* Managed Infrastructure: No servers to manage
* Enterprise Security: Built-in compliance and data protection
* Easy Integration: Works seamlessly with other AWS services
* Cost Effective: Pay only for what you use
  
Claude Sonnet specifically excels at:<br/>
* Complex reasoning and planning
* Processing structured data
* Generating human-like responses

### Environment Setup
**Your Own AWS Account Setup**<br/> To build the weather agent in your own AWS account, follow these steps to configure your local development environment and AWS permissions.
  
 **Prerequisites**<br/>Before starting, ensure you have the following:
 * Python 3.7+ installed locally.
 * An AWS account with appropriate administrative or developer permissions.
 * The AWS CLI installed on your machine.

**Step 1: Set Up Your Development Environment**<br/>You'll need a code editor or IDE to write and run Python code during this workshop. Choose one of these options
  
  **Recommended IDEs:**<br/>
  
  **Visual Studio Code (Free)**<br/>Download: https://code.visualstudio.com/ , Install the Python extension for better code support , Lightweight and excellent for Python  development

   **JetBrains PyCharm**<br/>Download: https://www.jetbrains.com/pycharm/download/ , Full-featured Python IDE with excellent debugging tools , Great for more complex development

  **Other Options:**<br/>
    * **Sublime Text** - Fast and lightweight<br/>
    * **Jupyter Notebook** - Good for interactive development

 **Step 2: Ensure Python is Installed**<br/>
   Once your editor is ready, verify your Python installation in the terminal using python --version. On macOS, you may specifically need to use python3 and pip3.

 **Step 3: Set Up AWS Credentials**
  You must provide your agent with access to AWS services using one of two methods
 * **AWS CLI (Recommended):**<br/> Run aws configure in your terminal and enter your Access Key ID, Secret Access Key, and set the default region to us-west-2.
 * **Environment Variables:** <br/>Manually export AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, and AWS_DEFAULT_REGION in your terminal session.

 **Step 4: Enable Claude 4.5 Sonnet in Amazon Bedrock**<br/>
  This is a critical step; without it, your agent will not function.<br/>
   * Log into the Amazon Bedrock console.
   * Ensure you are in the us-west-2 (Oregon) region.
   * Navigate to Model Access and request access for Claude 4.5 Sonnet.
   * Ensure your IAM user has the permissions

 **Step 5: Create Project Directory and install Dependencies**<br/>
  Create a dedicated workspace for your agent and set up a virtual environment to manage packages:
   * **Create a directory:** _mkdir agentic-ai-workshop && cd agentic-ai-workshop_.
   * **Initialize a Virtual Environment**: _Run python -m venv .venv_ and activate it. You should see (.venv) in your terminal prompt once active.
   * **Install Packages:** Create a _requirements.txt_ file (or install directly) containing the necessary libraries like boto3, requests, and streamlit

  **Step 6: Verify Your Setup**<br/>
    In your IDE terminal, run these commands:<br/>
     <br/>Test AWS connection<br/>
     _aws sts get-caller-identity_<br/>
     <br/>Test Python imports<br/>
      _python -c "import boto3, streamlit, requests; print('✅ All packages installed')"_

  ### Understanding Our Agent Architecture
  <br/>**How the National Weather Service APIs Work**
  <br/>The National Weather Service (NWS) offers free weather data using a required two-step API process because the U.S. is organized into a grid, with individual forecast offices responsible for specific grid areas.

  * **Points API:** This is the first step (https://api.weather.gov/points/{lat},{lon}). It takes latitude and longitude coordinates as input and identifies which NWS forecast office covers that specific location, along with the necessary grid coordinates. For example, coordinates for Seattle map to the "SEW" office at grid 124,67.<br/>
* **Forecast API:** This is the second step (https://api.weather.gov/gridpoints/{office}/{gridX},{gridY}/forecast). It uses the specific office and grid data obtained from the Points API to retrieve the detailed weather forecast in a technical JSON format.

  **How Our Weather Agent Works**
  <br/>Our weather assistant follows a simple 4-step process: **User Input → AI Planning → API Calls → AI Summary → Response**

    [!img]()
  
