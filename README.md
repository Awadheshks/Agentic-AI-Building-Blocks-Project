# Agentic-AI-Building-Blocks-Project
### Agentic AI Building Blocks using Amazon Bedrock

![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/2fd8b5baaeb9fe25fa6961ede54437174187c7f0/assets/Bedrock-Main.png)


Agentic AI weather assistant—powered by Amazon Bedrock and Python—that orchestrates API calls for real-time weather data, generates human-friendly forecasts, and demonstrates the evolution from static AI "encyclopedias" to systems that think, plan, and act on complex challenges.
This workshop unpacks its core elements through a Bedrock-powered weather assistant built with Python.

## What is Agentic AI?
Agentic AI refers to advanced AI systems that operate autonomously to achieve goals, going beyond simple responses by perceiving environments, reasoning through problems, planning actions, and learning from outcomes. Unlike traditional AI, which reacts to prompts, agentic AI proactively handles multi-step tasks with minimal human input.Agentic AI shifts from "Question → Answer" to a dynamic "**Problem → Plan → Action → Result**" cycle. Key sources identify three essential traits defining true agentic systems:

**Autonomy**<br/>Agentic AI makes independent decisions and executes workflows without constant oversight, selecting tools and adapting strategies dynamically. This enables it to manage complex processes like diagnostics or optimizations solo

**Reactivity**<br/>These systems respond to real-time changes by interpreting new data, self-correcting errors, and refining plans iteratively. They maintain context awareness to handle dynamic environments effectively.

**Proactivity**<br/>Agentic AI anticipates needs, initiates actions toward objectives, and orchestrates multi-step sequences without prompts. It breaks down high-level goals into executable steps, often across systems

## Our Weather Assistant Agent Example
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

## Why Amazon Bedrock for Agentic AI?
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

## Environment Setup
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

  ## Understanding Our Agent Architecture
  **How the National Weather Service APIs Work**
  <br/>The National Weather Service (NWS) offers free weather data using a required two-step API process because the U.S. is organized into a grid, with individual forecast offices responsible for specific grid areas.

  * **Points API:** This is the first step (https://api.weather.gov/points/{lat},{lon}). It takes latitude and longitude coordinates as input and identifies which NWS forecast office covers that specific location, along with the necessary grid coordinates. For example, coordinates for Seattle map to the "SEW" office at grid 124,67.<br/>
* **Forecast API:** This is the second step (https://api.weather.gov/gridpoints/{office}/{gridX},{gridY}/forecast). It uses the specific office and grid data obtained from the Points API to retrieve the detailed weather forecast in a technical JSON format.

  ## How Our Weather Agent Works
  Our weather assistant follows a simple 4-step process: **User Input → AI Planning → API Calls → AI Summary → Response**

    ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/03%20Workflow-II.png)

   ### Understanding the Flow
    The diagram shows how our agent processes weather requests:<br/>
     1. **User inputs location** (city, ZIP, or coordinates)
     2. **Claude AI analyzes** and identifies required coordinates + API endpoints
     3. **Two sequential API calls:** Gridpoint lookup → Forecast office data → Weather details
     4. **Claude processes** raw JSON into human-readable format
     5. **User receives** clear, friendly weather forecast
 
    ### The 4 Steps in Detail
     **Step 1: User Input**<br/>The agent offers high flexibility, handling diverse location inputs without rigid formatting. Users can provide<br/>
        * **City names:** "New York" or "Seattle, WA"<br/>
        * **ZIP codes:** "94085"<br/>
        * **Informal descriptions:** "downtown Portland"<br/>
       * **Coordinates:** "47.6062, -122.3321"

  
     **Step 2: AI Planning**<br/>In this phase, Claude 4.5 Sonnet serves as the "brain," analyzing input to craft a precise technical strategy<br/>
     * Determines the coordinates for the location
     * Identifies the correct National Weather Service API endpoints
     * Plans the sequence of API calls needed
  
     **Step 3: API Calls**<br/> The agent executes the plan through two sequential API requests to the National Weather Service:
     * **Points API:** https://api.weather.gov/points/{lat},{lon} to get forecast office and grid coordinates.
     * **Forecast API:** Uses the returned URL for detailed weather data 

     **Step 4: AI Summary**
      <br/>Claude converts the raw JSON weather data into a human-friendly format:
      <br/>**Raw data** (complex JSON with technical details) ↓ Friendly response: "Today: Partly cloudy with a high of 72°F. Wind: West at 5-10 mph"

  **Why This Architecture Works**
    <br/>Intelligent: The AI figures out coordinates and API calls instead of hardcoding them Flexible: Handles different location formats and weather data variations      Reliable: Includes error handling for network issues and invalid locations Scalable: The same pattern works for other APIs and domains.

  ### Building the Command-Line Agent
  <br/>Now let's build our AI agent step by step! We'll create a python program that demonstrates all the core agentic AI patterns.

  #### Step 1: Establishing the Amazon Bedrock Connection
  First, let's create the foundation - connecting to Claude 4.5 Sonnet.
  Implementation: Create a file named _weather_agent_cli.py_ within your _a_gentic-ai-workshop_ directory, and add this code:<br/>
```
# Import necessary libraries
import boto3        # AWS SDK for Python - allows us to interact with AWS services
import json         # For handling JSON data
import subprocess   # For running system commands like curl
import time         # For adding delays and timing operations
from datetime import datetime  # For timestamps and date operations

def call_claude_sonnet(prompt):
    """
    This function sends a prompt to Claude 4.5 Sonnet and gets a response.
    This is the "brain" of our agent - where the AI thinking happens.
    
    Args:
        prompt (str): The question or instruction we want to send to Claude
    
    Returns:
        tuple: (success: bool, response: str) - success status and Claude's response or error message
    """
    # Create a connection to Amazon Bedrock service
    # Bedrock is AWS's service for accessing AI models like Claude
    bedrock = boto3.client(
        service_name='bedrock-runtime',  # Specify we want the runtime version for making AI calls
        region_name='us-west-2'          # AWS region - using us-west-2 as specified
    )
    
    try:
        # Send our prompt to Claude and get a response
        response = bedrock.converse(
            # Specify which version of Claude we want to use
            modelId='us.anthropic.claude-sonnet-4-5-20250929-v1:0',  # Claude 4.5 Sonnet
            
            # Format our message - Claude expects messages in a specific structure
            messages=[
                {
                    "role": "user",                    # We are the user asking a question
                    "content": [{"text": prompt}]      # Our actual question/prompt
                }
            ],
            
            # Configure how Claude should respond
            inferenceConfig={
                "maxTokens": 2000,    # Maximum length of response (tokens ≈ words)
                "temperature": 0.7   # Creativity level (0=very focused, 1=very creative)
            }
        )
        
        # Extract the actual text response from Claude's response structure
        # The response comes nested in a complex structure, so we dig down to get the text
        return True, response['output']['message']['content'][0]['text']
        
    except Exception as e:
        # If something goes wrong, return an error message
        return False, f"Error calling Claude: {str(e)}"

Test our connection to Claude
if __name__ == "__main__":
    print("🧪 Testing connection to Claude 4.5 Sonnet...")
    success, response = call_claude_sonnet("Hello! Are you working today?")
    
    if success:
        print("✅ Connection successful!")
        print(f"Claude says: {response}")
    else:
        print("❌ Connection failed!")
        print(f"Error: {response}")
```

**Test this code:**<br/>

Now, in the terminal window, run this:<br/>
 _python weather_agent_cli.py_

 You should see Claude's response confirming the connection works!
 
 ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/04.Test-Bedrock%20Connection-Claude.png)

**What's Happening Here?**<br/>
* Amazon Bedrock Client: Establishes link to Bedrock in us-west-2
* Model Selection: We specify Claude 4.5 Sonnet as our AI model
* Message Format: We structure our prompt in the format Claude expects
* Configuration: We set parameters for response length and creativity
* Error Handling: Captures and manages connection issues

** Make sure to REMOVE the code underneath # Test our connection to Claude from the previous step before pasting the code below!

 #### Step 2: Building Agent Logic
 
 Now let's add the core agent functionality. Add these functions to your _weather_agent_cli.py_ file:

```
def execute_curl_command(url):
    """
    Execute a curl command to fetch data from an API.
    This is how our agent "acts" in the real world - making HTTP requests.
    
    Args:
        url (str): The URL to fetch data from
    
    Returns:
        tuple: (success: bool, response: str) - success status and API response or error message
    """
    try:
        # Use curl command to make HTTP request
        # curl is a command-line tool for making HTTP requests
        result = subprocess.run(
            ['curl', '-s', url],  # -s flag makes curl silent (no progress info)
            capture_output=True,   # Capture the output so we can process it
            text=True,            # Return output as text (not bytes)
            timeout=30            # Give up after 30 seconds
        )
        
        # Check if the command was successful
        if result.returncode == 0:
            return True, result.stdout
        else:
            return False, f"Curl command failed: {result.stderr}"
            
    except subprocess.TimeoutExpired:
        return False, "Request timed out after 30 seconds"
    except Exception as e:
        return False, f"Error executing curl: {str(e)}"

def generate_weather_api_calls(location):
    """
    Use Claude to intelligently generate National Weather Service API calls for a given location.
    This is where the "agentic" magic happens - AI planning the API calls.
    
    Args:
        location (str): The location provided by the user
    
    Returns:
        tuple: (success: bool, api_calls: list) - success status and list of API URLs or error message
    """
    # Create a detailed prompt that teaches Claude how to generate NWS API calls
    prompt = f"""
You are an expert at working with the National Weather Service (NWS) API.

Your task: Generate the NWS API URL to get weather forecast data for "{location}".

Instructions:
1. First, determine the approximate latitude and longitude coordinates for this location
2. Generate the NWS Points API URL: https://api.weather.gov/points/{{lat}},{{lon}}

For the coordinates, use your knowledge to estimate:
- Major cities: Use well-known coordinates
- ZIP codes: Estimate based on the area
- States: Use approximate center coordinates
- In case a location description is provided instead of a location name, please use the most likely city and state name as the location for the coordinates

Example for Seattle:
https://api.weather.gov/points/47.6062,-122.3321

Example for largest city in USA:
Based on your knowledge, you will establish location is New York City
https://api.weather.gov/points/40.7128,-74.0060

Now generate the API call (Points API) for the established location. 
Return ONLY the complete Points API URL, nothing else.
Format: https://api.weather.gov/points/LAT,LON
"""
    
    print(f"🧠 AI is analyzing '{location}' and generating weather API calls...")
    success, response = call_claude_sonnet(prompt)
    
    if success:
        # Clean up the response - sometimes Claude adds extra text
        api_url = response.strip()
        # Make sure we got a valid URL
        if api_url.startswith('https://api.weather.gov/points/'):
            return True, [api_url]  # Return as list for consistency
        else:
            return False, f"AI generated invalid URL: {api_url}"
    else:
        return False, response

def get_forecast_url_from_points_response(points_json):
    """
    Extract the forecast URL from the NWS Points API response.
    
    Args:
        points_json (str): JSON response from the Points API
    
    Returns:
        tuple: (success: bool, forecast_url: str) - success status and forecast URL or error message
    """
    try:
        data = json.loads(points_json)
        forecast_url = data['properties']['forecast']
        return True, forecast_url
    except (json.JSONDecodeError, KeyError) as e:
        return False, f"Error parsing Points API response: {str(e)}"

def process_weather_response(raw_json, location):
    """
    Use Claude to convert raw NWS API JSON into a human-readable weather summary.
    This is where AI processes complex data into useful information.
    
    Args:
        raw_json (str): Raw JSON response from NWS API
        location (str): Original location for context
    
    Returns:
        tuple: (success: bool, summary: str) - success status and processed summary or error message
    """
    prompt = f"""
You are a weather information specialist. I have raw National Weather Service forecast data for "{location}" that needs to be converted into a clear, helpful summary for a general audience.

Raw NWS API Response:
{raw_json}

Please create a weather summary that includes:
1. A brief introduction with the location
2. Current conditions and today's forecast
3. The next 2-3 days outlook with key details (temperature, precipitation, wind)
4. Any notable weather patterns or alerts
5. Format the response to be easy to read and understand

Make it informative and practical for someone planning their activities. Focus on being helpful and clear.
"""
    
    print(f"📊 AI is processing weather data and creating summary...")
    success, response = call_claude_sonnet(prompt)
    
    return success, response

```


**What's Happening Here?**<br/>
* **execute_curl_command()** :The agent's "hands"—performs actual HTTP requests to external APIs, fetching raw weather data from NWS endpoints.
* **generate_weather_api_calls()**:The agent's "planning brain"—analyzes location input, determines correct grid points, and constructs precise API call sequences.
* **get_forecast_url_from_points_response()**:Parses the initial Points API response to extract the specific forecast office URL needed for the second API call.
* **process_weather_response()**:The agent's "analysis brain"—transforms complex, technical JSON weather data into clear, user-friendly summaries and forecasts.

### Step 3: Main Agent Workflow
  Now let's tie everything together with the main agent workflow. Add this to your file:
  ```
  def run_weather_agent():
    """
    Main function that orchestrates our AI agent.
    This demonstrates the complete agentic workflow.
    """
    print("🌤️ Welcome to the Weather AI Agent!")
    print("This agent uses Claude 4.5 Sonnet to help you get weather forecasts.")
    print("=" * 60)
    
    while True:
        # Get user input
        location = input("\n🔍 Enter a location name or description (or 'quit' to exit): ").strip()
        
        if location.lower() in ['quit', 'exit', 'q']:
            print("👋 Thanks for using the Weather Agent!")
            break
            
        if not location:
            print("❌ Please enter a location name or description.")
            continue
            
        print(f"\n🚀 Starting weather analysis for '{location}'...")
        print("-" * 40)
        
        # Step 1: AI generates the Points API URL
        print("Step 1: 🧠 AI Planning Phase")
        success, api_calls = generate_weather_api_calls(location)
        
        if not success:
            print(f"❌ Failed to generate API calls: {api_calls}")
            continue
            
        points_url = api_calls[0]
        print(f"✅ Generated Points API URL: {points_url}")
        
        # Step 2: Execute the Points API call
        print("\nStep 2: 🔗 Points API Execution")
        print("Fetching location data from National Weather Service...")
        success, points_response = execute_curl_command(points_url)
        
        if not success:
            print(f"❌ Failed to fetch points data: {points_response}")
            continue
            
        print(f"✅ Received points data")
        
        # Step 3: Extract forecast URL from Points response
        print("\nStep 3: 📍 Extracting Forecast URL")
        success, forecast_url = get_forecast_url_from_points_response(points_response)
        
        if not success:
            print(f"❌ Failed to extract forecast URL: {forecast_url}")
            continue
            
        print(f"✅ Forecast URL: {forecast_url[:60]}...")
        
        # Step 4: Execute the Forecast API call
        print("\nStep 4: 🌦️ Forecast API Execution")
        print("Fetching weather forecast data...")
        success, forecast_response = execute_curl_command(forecast_url)
        
        if not success:
            print(f"❌ Failed to fetch forecast data: {forecast_response}")
            continue
            
        print(f"✅ Received {len(forecast_response)} characters of forecast data")
        
        # Step 5: AI processes the response
        print("\nStep 5: 📊 AI Analysis Phase")
        success, summary = process_weather_response(forecast_response, location)
        
        if not success:
            print(f"❌ Failed to process data: {summary}")
            continue
            
        # Step 6: Display results
        print("\nStep 6: 💬 Weather Forecast")
        print("=" * 60)
        print(summary)
        print("=" * 60)
        
        print(f"\n✅ Weather analysis complete for '{location}'!")

# Run the agent when the script is executed
if __name__ == "__main__":
    run_weather_agent()

  ```

### Complete Agent Workflow
Our agent now demonstrates the full agentic AI pattern:<br/>
* **Planning:** AI analyzes the location and plans the API calls
* **Action:** Agent executes the planned API calls in sequence
* **Processing:** AI analyzes the raw weather data and creates insights
* **Response:** Agent presents results to the user

### Test Your Agent!

Now, in the terminal window, run your completed agent:
 _python weather_agent_cli.py_

 ### Try these test cases:

  #### 1. "NewYork" (should work well - major city)
 
  ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/05.%20First%20test%20result-NewYork-I.png)
  
  ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/05.%20First%20test%20result-NewYork-II.png)
  
     
#### 2. "94085" (tests ZIP code recognition)
 
  ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/06.%20Second%20test%20result-ZipCode-I.png)
  
  ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/06.%20Second%20test%20result-ZipCode-II.png)
  
     
#### 3. "Chicago City" (tests city name variations)

   ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/07.%20Third%20test%20result-CityNameVariation-I.png)
   
   ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/07.%20Third%20test%20result-CityNameVariation-II.png)
   
 
#### 4.	"National park near Wizard Island in Oregon"

   ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/08.%20Fourth%20test%20result-InformalDescription-I.png
)

   ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/08.%20Fourth%20test%20result-InformalDescription-II.png
)

     
#### 5.	"Largest City in Texas”

   ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/09.%20Fifth%20test%20result-LargestCity-I.png)
   
   ![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/09.%20Fifth%20test%20result-LargestCity-II.png)


** Congratulations!** You've built a working AI agent that demonstrates all the key agentic AI patterns! 
 

 ### Running the Web Application
 <br/>Now experience your AI agent through an interactive web interface! We'll build a Streamlit app using the same agentic logic for a polished user experience

 #### Create the Web Application
  In the agentic-ai-workshop folder from your environment setup, create _weather_agent_web.py_ and paste the complete code below:
  ```
  import streamlit as st
import boto3
import subprocess
import json
import time
from datetime import datetime
from PIL import Image
import os

# Page configuration
st.set_page_config(
    page_title="Weather AI Agent",
    page_icon="🌤️",
    layout="wide",
    initial_sidebar_state="expanded"
)

# Custom CSS for better styling
st.markdown("""
<style>
    .step-container {
        border: 2px solid #e0e0e0;
        border-radius: 10px;
        padding: 20px;
        margin: 10px 0;
        background-color: #f9f9f9;
    }
    .step-header {
        font-size: 18px;
        font-weight: bold;
        color: #1f77b4;
        margin-bottom: 10px;
    }
    .success-box {
        border-left: 5px solid #28a745;
        background-color: #d4edda;
        padding: 10px;
        margin: 10px 0;
        color: #000000;
    }
    .error-box {
        border-left: 5px solid #dc3545;
        background-color: #f8d7da;
        padding: 10px;
        margin: 10px 0;
        color: #000000;
    }
    .info-box {
        border-left: 5px solid #17a2b8;
        background-color: #d1ecf1;
        padding: 10px;
        margin: 10px 0;
        color: #000000;
    }
</style>
""", unsafe_allow_html=True)

def call_claude_sonnet(prompt):
    """
    Connect to Claude 4.5 Sonnet via Amazon Bedrock
    """
    bedrock = boto3.client(
        service_name='bedrock-runtime',
        region_name='us-west-2'
    )
    
    try:
        response = bedrock.converse(
            modelId='us.anthropic.claude-sonnet-4-5-20250929-v1:0',
            messages=[
                {
                    "role": "user",
                    "content": [{"text": prompt}]
                }
            ],
            inferenceConfig={
                "maxTokens": 2000,
                "temperature": 0.7
            }
        )
        
        return True, response['output']['message']['content'][0]['text']
        
    except Exception as e:
        return False, f"Error calling Claude: {str(e)}"

def execute_curl_command(url):
    """
    Execute curl command to fetch API data
    """
    try:
        result = subprocess.run(
            ['curl', '-s', url],
            capture_output=True,
            text=True,
            timeout=30
        )
        
        if result.returncode == 0:
            return True, result.stdout
        else:
            return False, f"Curl command failed: {result.stderr}"
            
    except subprocess.TimeoutExpired:
        return False, "Request timed out after 30 seconds"
    except Exception as e:
        return False, f"Error executing curl: {str(e)}"

def generate_weather_api_calls(location):
    """
    Use Claude to generate NWS API calls
    """
    prompt = f"""
You are an expert at working with the National Weather Service (NWS) API.

Your task: Generate the NWS API URL to get weather forecast data for "{location}".

Instructions:
1. First, determine the approximate latitude and longitude coordinates for this location
2. Generate the NWS Points API URL: https://api.weather.gov/points/{{lat}},{{lon}}

For the coordinates, use your knowledge to estimate:
- Major cities: Use well-known coordinates
- ZIP codes: Estimate based on the area
- States: Use approximate center coordinates
- In case a location description is provided instead of a location name, please use the most likely city and state name as the location for the coordinates

Example for Seattle:
https://api.weather.gov/points/47.6062,-122.3321

Example for largest city in USA:
Based on your knowledge, you will establish location is New York City
https://api.weather.gov/points/40.7128,-74.0060

Now generate the API call (Points API) for the established location. 
Return ONLY the complete Points API URL, nothing else.
Format: https://api.weather.gov/points/LAT,LON
"""
    
    success, response = call_claude_sonnet(prompt)
    
    if success:
        api_url = response.strip()
        if api_url.startswith('https://api.weather.gov/points/'):
            return True, [api_url]
        else:
            return False, f"AI generated invalid URL: {api_url}"
    else:
        return False, response

def get_forecast_url_from_points_response(points_json):
    """
    Extract forecast URL from Points API response
    """
    try:
        data = json.loads(points_json)
        forecast_url = data['properties']['forecast']
        return True, forecast_url
    except (json.JSONDecodeError, KeyError) as e:
        return False, f"Error parsing Points API response: {str(e)}"

def process_weather_response(raw_json, location):
    """
    Use Claude to process NWS API response
    """
    prompt = f"""
You are a weather information specialist. I have raw National Weather Service forecast data for "{location}" that needs to be converted into a clear, helpful summary for a general audience.

Raw NWS API Response:
{raw_json}

Please create a weather summary that includes:
1. A brief introduction with the location
2. Current conditions and today's forecast
3. The next 2-3 days outlook with key details (temperature, precipitation, wind)
4. Any notable weather patterns or alerts
5. Format the response to be easy to read and understand

Make it informative and practical for someone planning their activities. Focus on being helpful and clear.
"""
    
    success, response = call_claude_sonnet(prompt)
    return success, response

# Sidebar with information
st.sidebar.title("🤖 About This Agent")
st.sidebar.markdown("""
This AI agent demonstrates **Agentic AI** principles:

**🧠 Intelligence**: Uses Claude 4.5 Sonnet to understand locations and plan API calls

**🔗 Action**: Automatically calls the National Weather Service API

**📊 Processing**: Converts complex weather data into readable forecasts

**💬 Response**: Provides helpful, practical weather information
""")

st.sidebar.markdown("---")
st.sidebar.markdown("### 🏗️ Architecture")
st.sidebar.markdown("""
1. **User Input** → Location name
2. **AI Planning** → Generate API calls
3. **Points API** → Get forecast office  
4. **Forecast API** → Get weather data
5. **AI Processing** → Create summary
6. **Display Results** → Show to user
""")

# Main application
st.title("🌤️ Weather AI Agent")
st.markdown("### Powered by Claude 4.5 Sonnet on Amazon Bedrock")

st.markdown("""
This intelligent agent helps you get weather forecasts using the National Weather Service API. 
Enter any location below and watch the AI agent work through its reasoning process!
""")

# Initialize session state for results
if 'show_results' not in st.session_state:
    st.session_state.show_results = False

# Input section
st.markdown("---")
location = st.text_input(
    "🔍 Enter a location name or description:",
    placeholder="e.g., Seattle, 90210, New York City, National park near Homestead in Florida",
    help="You can enter city names, ZIP codes, state names, or location descriptions"
)

# Create columns for the buttons
button_col1, button_col2 = st.columns([2, 1])

with button_col1:
    get_forecast = st.button("🚀 Get Weather Forecast", type="primary")

with button_col2:
    clear_results = st.button("🗑️ Clear Results", type="secondary")

# Clear results functionality
if clear_results:
    st.session_state.show_results = False
    st.success("🗑️ Results cleared! Enter a new location to get a fresh forecast.")

if get_forecast:
    st.session_state.show_results = True

if st.session_state.show_results and get_forecast:
    if not location:
        st.error("❌ Please enter a location name or description.")
    else:
        # Create columns for better layout
        col1, col2 = st.columns([2, 1])
        
        with col1:
            st.markdown(f"## Weather Analysis for: **{location}**")
            
            # Step 1: AI Planning
            with st.container():
                st.markdown('<div class="step-container">', unsafe_allow_html=True)
                st.markdown('<div class="step-header">🧠 Step 1: AI Planning Phase</div>', unsafe_allow_html=True)
                
                with st.spinner("Claude is analyzing the location and planning the API calls..."):
                    success, api_calls = generate_weather_api_calls(location)
                
                if success:
                    points_url = api_calls[0]
                    st.markdown('<div class="success-box">✅ Points API URL generated successfully!</div>', unsafe_allow_html=True)
                    st.code(points_url, language="text")
                else:
                    st.markdown(f'<div class="error-box">❌ Failed to generate API calls: {api_calls}</div>', unsafe_allow_html=True)
                    st.stop()
                
                st.markdown('</div>', unsafe_allow_html=True)
            
            # Step 2: Points API Execution
            with st.container():
                st.markdown('<div class="step-container">', unsafe_allow_html=True)
                st.markdown('<div class="step-header">🔗 Step 2: Points API Execution</div>', unsafe_allow_html=True)
                
                with st.spinner("Fetching location data from National Weather Service..."):
                    success, points_response = execute_curl_command(points_url)
                
                if success:
                    st.markdown('<div class="success-box">✅ Received location data from NWS</div>', unsafe_allow_html=True)
                    
                    # Show a preview of the raw data
                    with st.expander("🔍 View Raw Points API Response (first 500 characters)"):
                        st.code(points_response[:500] + "..." if len(points_response) > 500 else points_response, language="json")
                else:
                    st.markdown(f'<div class="error-box">❌ Failed to fetch points data: {points_response}</div>', unsafe_allow_html=True)
                    st.stop()
                
                st.markdown('</div>', unsafe_allow_html=True)
            
            # Step 3: Extract Forecast URL
            with st.container():
                st.markdown('<div class="step-container">', unsafe_allow_html=True)
                st.markdown('<div class="step-header">📍 Step 3: Extracting Forecast URL</div>', unsafe_allow_html=True)
                
                success, forecast_url = get_forecast_url_from_points_response(points_response)
                
                if success:
                    st.markdown('<div class="success-box">✅ Forecast URL extracted successfully!</div>', unsafe_allow_html=True)
                    st.code(forecast_url, language="text")
                else:
                    st.markdown(f'<div class="error-box">❌ Failed to extract forecast URL: {forecast_url}</div>', unsafe_allow_html=True)
                    st.stop()
                
                st.markdown('</div>', unsafe_allow_html=True)
            
            # Step 4: Forecast API Execution
            with st.container():
                st.markdown('<div class="step-container">', unsafe_allow_html=True)
                st.markdown('<div class="step-header">🌦️ Step 4: Forecast API Execution</div>', unsafe_allow_html=True)
                
                with st.spinner("Fetching weather forecast data..."):
                    success, forecast_response = execute_curl_command(forecast_url)
                
                if success:
                    st.markdown(f'<div class="success-box">✅ Received {len(forecast_response):,} characters of forecast data</div>', unsafe_allow_html=True)
                    
                    # Show a preview of the raw data
                    with st.expander("🔍 View Raw Forecast API Response (first 500 characters)"):
                        st.code(forecast_response[:500] + "..." if len(forecast_response) > 500 else forecast_response, language="json")
                else:
                    st.markdown(f'<div class="error-box">❌ Failed to fetch forecast data: {forecast_response}</div>', unsafe_allow_html=True)
                    st.stop()
                
                st.markdown('</div>', unsafe_allow_html=True)
            
            # Step 5: AI Processing
            with st.container():
                st.markdown('<div class="step-container">', unsafe_allow_html=True)
                st.markdown('<div class="step-header">📊 Step 5: AI Analysis Phase</div>', unsafe_allow_html=True)
                
                with st.spinner("Claude is processing the weather data and creating a summary..."):
                    success, summary = process_weather_response(forecast_response, location)
                
                if success:
                    st.markdown('<div class="success-box">✅ Weather analysis complete!</div>', unsafe_allow_html=True)
                else:
                    st.markdown(f'<div class="error-box">❌ Failed to process data: {summary}</div>', unsafe_allow_html=True)
                    st.stop()
                
                st.markdown('</div>', unsafe_allow_html=True)
            
            # Step 6: Results
            st.markdown("---")
            st.markdown("## 🌤️ Weather Forecast")
            st.markdown(summary)
            
        with col2:
            # Real-time status updates
            st.markdown("### 📊 Process Status")
            
            status_container = st.container()
            with status_container:
                st.markdown("""
                <div class="info-box">
                <strong>🔄 Agent Workflow:</strong><br>
                ✅ Planning Phase<br>
                ✅ Points API Call<br>
                ✅ URL Extraction<br>
                ✅ Forecast API Call<br>
                ✅ Data Processing<br>
                ✅ Results Generated
                </div>
                """, unsafe_allow_html=True)
            
            st.markdown("### 🎯 What Makes This Agentic?")
            st.markdown("""
            - **🧠 Reasoning**: AI understands location formats
            - **📋 Planning**: Generates appropriate API call sequences
            - **🔧 Action**: Executes real-world API requests
            - **📊 Processing**: Converts raw data to insights
            - **🔄 Adaptation**: Handles different location types
            """)

# Footer
st.markdown("---")
st.markdown("""
### 🔬 About This Demo

This application demonstrates **Agentic AI** principles using:
- **Amazon Bedrock** with Claude 4.5 Sonnet for intelligent reasoning
- **National Weather Service API** for real-time weather data
- **Streamlit** for interactive web interface

**⚠️ Important**: This uses official NWS data for educational purposes. For critical weather decisions, consult official sources.
""")

# Add some example queries
st.markdown("### 💡 Try These Examples:")
st.markdown("""
**Suggested locations to test:**
- **Seattle** - Major city (tests city name recognition)
- **90210** - ZIP code (tests postal code handling)  
- **New York City** - Multi-word city (tests complex location parsing)
- **Miami, FL** - City with state (tests state abbreviations)
- **Chicago** - Another major city (tests different coordinates)
- **National park near Homestead in Florida** - Location description (tests AI reasoning)
- **Largest City in California** - Descriptive query (tests knowledge-based location finding)

Simply copy any of these into the location input above and click "Get Weather Forecast"!
""")

  ```
 
## Run Your Web Application

  Now, in the terminal window, let's run the Streamlit web interface:
    _streamlit run weather_agent_web.py_

  You should see output like:

    You can now view your Streamlit app in your browser.
       _Local URL: http://localhost:8501
        Network URL: http://192.168.1.100:8501_


 #### Explore the Web Interface
🎯** Key Features to Try:**
  1. Interactive Input: Enter different locations and see real-time processing
  2. Step-by-Step Visualization: Watch each phase of the agentic workflow:

  * AI Planning Phase
  * Points API Execution Phase
  * URL Extraction Phase
  * Forecast API Execution Phase
  * AI Analysis Phase
  * Results Display
   
**3. Raw Data Inspection:** Expand the "View Raw API Response" sections to see the complex JSON data your agent processes

**4. Real-time Status:** Monitor the agent's progress in the sidebar

**5. Example Queries:** Try the suggested examples (New York, 94085, Seattle)



![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/10.Web-Application-II.png)

![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/10.Web-Application-IIIn.png)

![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/10.Web-Application-IV.png)

![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/10.Web-Application-V.png)

![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/10.Web-Application-VI.png)

![img](https://github.com/Awadheshks/Agentic-AI-Building-Blocks-Project/blob/0202d7fa0a58ffeb31f30efca03d32e264e52b22/assets/10.Web-Application-VII.png)


### What to Observe
<br/>Monitor how the AI seamlessly bridges natural language to technical execution:

**Planning Intelligence**: Watch the AI parse diverse inputs—"Seattle" → coordinates, "90210" → ZIP lookup—and dynamically generate location-specific API calls.

**Data Processing**: Observe raw JSON (multiple forecast periods, technical details) transformed into concise, human-readable summaries with relevant context.

**Error Handling**: Test resilience with invalid inputs (misspellings, non-US locations). Note helpful guidance instead of crashes, given NWS API's US-only scope.

### Compare CLI vs Web Experience
We now have both versions of your AI agent:

**Command-Line Version (weather_agent_cli.py)**<br/>
✅ Direct, focused interaction

✅ Ideal for automation/scripting

✅ Reveals raw workflow steps

✅ Developer-friendly

**Web Version (weather_agent_web.py)**<br/>
✅ Visual, interactive interface

✅ Better for non-technical users

✅ Real-time progress indicators

✅ Professional presentation

Both versions use identical agentic AI logic - the same Claude 4.5 Sonnet reasoning, the same NWS API integration, and the same data processing pipeline!

## Key Takeaways
**Agentic AI Definition** :Shifts AI from "Question → Answer" to **"Problem → Plan → Action → Result**." Defined by autonomy (independent decisions), reactivity (adapts to changes/errors), proactivity (multi-step initiative).<br/>
**AI as Reasoning Engine**: Claude 4.5 Sonnet interprets vague inputs ("largest city in Texas") → coordinates → API URLs, replacing hardcoded logic.<br/>
**Agentic Workflow**:<br/>
 * Planning: Analyze input, strategize
 * Action: Execute APIs
 * Processing: Extract insights from raw data
 * Response: Human-friendly output
   
**No-Framework Building**: Pure Python + boto3 builds resilient agents—Bedrock connections, error handling, network timeouts—revealing every component.<br/>
**Real-World Applications**: Weather patterns (coordinates, sequential APIs) apply to travel, emergencies, markets, agriculture.

## Conclusion
The workshop emphasizes that AI's future lies beyond chatbots—in agents that execute real-world actions. By combining real-time data with AI reasoning, developers deliver domain-agnostic value. Success demands starting simple, iterating rapidly, and scaling from core blocks to sophisticated multi-agent production systems.
  
