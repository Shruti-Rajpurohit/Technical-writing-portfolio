# Building a SpaceX Launch Tracker with Python

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Understanding the SpaceX API](#understanding-the-spacex-api)
- [Project Structure](#project-structure)
- [Building the Tracker](#building-the-tracker) 
    - [Formatting Launch Data](#formatting-launch-data) 
    - [Fetching Launches](#fetching-launches) 
    - [Displaying the Tracker](#displaying-the-tracker)
- [Running the Application](#running-the-application)
- [Error Handling](#error-handling)
- [What's Next](#whats-next)
- [Conclusion](#conclusion)

## Introduction
SpaceX offers a free, public API that allows you to access information about their launches, rockets, capsules, and more. The API does not require authentication and has no rate limits, making it ideal for learning how to work with REST APIs and build real-world applications.

In this guide, you will build a command-line SpaceX Launch Tracker that prints:
- The latest SpaceX launch with mission details
- Upcoming launches with dates and information
- Correctly formatted dates and mission status
- Error handling for API failures

By the end of this guide, you will have a functional Python application and understand how to consume REST APIs, parse JSON responses, and handle errors in a graceful manner.

**Technologies used:**
- Python 3.8+
- `requests` library for HTTP requests
- SpaceX API v4

## Prerequisites 
Before you start, ensure you have the following:
- **Python 3.8 or higher** installed on your machine
- Familiarity with Python (functions, dictionaries, lists) 
- Knowledge of HTTP requests and JSON
-  A code editor (VS Code, PyCharm, or even Google Colab)

**Install the required library:**
```python
pip install requests
```
The requests library is the only external dependency we need. It takes care of all HTTP requests to the SpaceX API.

## Understanding the SpaceX API
The SpaceX API is a REST API that responds with JSON data. Here are the main endpoints that we will be using:
| Endpoint | Description |
|----------|-------------|
| `https://api.spacexdata.com/v4/launches` | List of all launches (past and future) |
| `https://api.spacexdata.com/v4/launches/latest` | Latest launch |
| `https://api.spacexdata.com/v4/launches/upcoming` | List of all upcoming launches |

**No authentication required.**
 You can call the API directly without API keys or tokens.
 **Example API Response:** 
 When you make a request for the latest launch, you receive a JSON response that looks something like this:
 ```json
 {  
    "name": "Crew-5",  
    "date_utc": "2022-10-05T16:00:00.000Z",  
    "success": true,  
    "details": "Mission description here",  
    "links": {"webcast": "https://youtu.be/5EwW8ZkArL4"},  
    "upcoming": false}
```
    
**The main fields that we will be using:**
- `name`: Mission name
- `date_utc`: Date of the launch in UTC time zone
- `success`: Whether the launch succeeded (null if upcoming)
- `details`: Mission description (null if no description)
- `links.webcast`: YouTube link to watch the launch
- `upcoming`: Boolean value indicating if the launch is in the future

You can test the API directly in your browser by visiting:
`https://api.spacexdata.com/v4/launches/latest`

## Project Structure
Our SpaceX Launch Tracker project has three primary functions:
```
spacex_tracker.py
├── format_launch_info()     # Converts raw API data to readable format
├── get_launches()           # Retrieves data from the API
└── display_launch_tracker() # Prints the formatted data to the terminal
```
**How it works:**
1. `get_launches()` sends HTTP requests to the SpaceX API
2. The raw JSON data is then fed into `format_launch_info()`
3. `display_launch_tracker()` then prints all of this information to the terminal in a clean format

This modular design makes each function easy to test and reuse independently.

## Building the Tracker
### Formatting Launch Data
First, we need a function to convert the API's raw data into a readable format:
```python
from datetime import datetime
def format_launch_info(launch):
    """
    Format a launch dictionary into readable information
    Args:
        launch: Raw launch data from SpaceX API
    Returns:
        Dictionary with formatted launch information    
    """    
    name = launch['name']
    date_str = launch['date_utc']
    date_obj = datetime.fromisoformat(date_str.replace('Z', '+00:00'))
    date_formatted = date_obj.strftime('%B %d, %Y at %H:%M UTC')
    
    details = launch.get('details', 'No details available')
    success = launch.get('success')
    
    if success is None:
        status = "Upcoming"
    elif success:
        status = "Success"
    else:
        status = "Failed"
            
    webcast = launch['links'].get('webcast', 'No webcast available')
    
    return {
        'name': name,
        'date': date_formatted,
        'details': details,
        'status': status,
        'webcast': webcast    
}
```
**What this function does:**
- Converts the UTC timestamp into a human-readable format like "October 05, 2022 at 16:00 UTC"
- Determines mission status: "Upcoming", "Success", or "Failed"
- Handles missing data gracefully using `.get()` with default values
- Returns a clean dictionary with only the fields we care about

**Why use `.get()` instead of direct access?**
The SpaceX API sometimes returns `null` for fields like `details`. Using `.get('details', 'No details available')` prevents errors if the field is missing or null.

### Fetching Launches
Next, we require a function to fetch launches from the API:
```python
import requests
def get_launches(limit=10, upcoming_only=False, latest_first=True):
    """
    Fetch SpaceX launches from the API
    Args:
        limit: Maximum number of launches to return (default: 10)
        upcoming_only: If True, only return upcoming launches (default: False)
        latest_first: If True, return most recent launches first (default: True)
    Returns:
        List of formatted launch information dictionaries
    """    
    if upcoming_only:
        url = "https://api.spacexdata.com/v4/launches/upcoming"    
    else:
        url = "https://api.spacexdata.com/v4/launches"

    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        launches = response.json()
        
        # Reverse the list if we want latest first
        if latest_first and not upcoming_only:
            launches = launches[::-1]
        # Format and return limited number
        formatted = [format_launch_info(launch) for launch in launches[:limit]]
        return formatted
        
    except requests.exceptions.RequestException as e:
        print(f"Error fetching launches: {e}")
        return []
```
**Understanding the parameters:**
- `limit=10`: Specifies the number of launches to fetch. This avoids overwhelming results.
- `upcoming_only=False`: If `True`, it will only fetch the upcoming launches.
- `latest_first=True`: This reverses the list so that the latest launches appear first.

**Error handling:**
- The `try-except` block catches network errors, timeouts, and HTTP errors. If the API is unreachable, the function returns an empty list instead of crashing.
- `response.raise_for_status()` raises an exception for HTTP errors like 404 or 500, which is then caught and handled.

### Displaying the Tracker
Finally, we create the main function that displays everything:
```python
def display_launch_tracker():
    """
    Display a formatted SpaceX launch tracker in the terminal
    """
    print("=" * 60)
    print("SpaceX Launch Tracker".center(60))
    print("=" * 60)
    print()

    # Show latest completed launch
    print("📍 LATEST LAUNCH")
    print("-" * 60)
    try:
        response = requests.get("https://api.spacexdata.com/v4/launches/latest", timeout=10)
        response.raise_for_status()
        latest = format_launch_info(response.json())
        print(f"Mission: {latest['name']}")
        print(f"Date: {latest['date']}")
        print(f"Status: {latest['status']}")
        if latest['details'] != 'No details available':
            print(f"Details: {latest['details']}")
        print(f"Watch: {latest['webcast']}")
    except requests.exceptions.RequestException as e:
        print(f"Error fetching latest launch: {e}")
    print()
    # Show upcoming launches
    print("🚀 UPCOMING LAUNCHES")
    print("-" * 60)
    upcoming = get_launches(limit=5, upcoming_only=True)
    if upcoming:
        for i, launch in enumerate(upcoming, 1):
            print(f"{i}. {launch['name']}")
            print(f"   Launch Date: {launch['date']}")
            if launch['details'] != 'No details available':
                print(f"   Details: {launch['details']}")
            print()
    else:
        print("No upcoming launches found.")

    print("=" * 60)
# Run the tracker when script is executed directly
if __name__ == "__main__":
    display_launch_tracker()
```

**How it works:**
1. Prints a header for visual clarity
2. Fetches and prints the most recent completed launch
3. Fetches and displays the next 5 upcoming launches
4. Handles errors separately for each API call (one failure doesn't bring down the whole program)

**The `if __name__ == "__main__":` check:**
This ensures `display_launch_tracker()` only runs when you execute the script directly, not when you import it as a module in another program.

## Running the Application
Now, save all the above code into a file named `spacex_tracker.py` and execute it in your terminal:
```bash
python spacex_tracker.py
```
**Expected output:**
```
============================================================
                  SpaceX Launch Tracker
============================================================
📍 LATEST LAUNCH
------------------------------------------------------------
Mission: Crew-5
Date: October 05, 2022 at 16:00 UTC
Status: Success
Watch: https://youtu.be/5EwW8ZkArL
🚀 UPCOMING LAUNCHES
------------------------------------------------------------
1. USSF-44
   Launch Date: November 01, 2022 at 13:41 UTC
2. Starlink 4-36 (v1.5)
   Launch Date: October 20, 2022 at 14:50 UTC
3. Galaxy 33 (15R) & 34 (12R)
   Launch Date: October 08, 2022 at 23:05 UTC
4. Hotbird 13F
   Launch Date: October 15, 2022 at 05:22 UTC
5. Hotbird 13G
   Launch Date: November 03, 2022 at 03:24 UTC
   ============================================================
```
**Running in Google Colab:**
If you are working on Google Colab, you can simply paste all the code into a single cell and run it. The output will appear directly in the notebook.

## Error Handling
This tracker implements a variety of error handling techniques:

**1. Network Timeouts**
```python
response = requests.get(url, timeout=10)
```
The `timeout=10` argument causes the request to fail after 10 seconds rather than hanging indefinitely. This will stop your program from freezing if the API is slow or unreachable.

**2. HTTP Error Codes**
```python
response.raise_for_status()
```
This will raise an exception for HTTP errors (404, 500, etc.), which we handle in the `except` block. Without this code, a 404 error would return empty data silently.

**3. Graceful Degradation**
```python
except requests.exceptions.RequestException as e:
    print(f"Error fetching launches: {e}")
    return []
```
If the API is unreachable, we print an error message but return an empty list instead of crashing. The program will keep running even if one API call fails.

**4. Missing Data Handling**
```python
details = launch.get('details', 'No details available')
```
The `.get()` method with a default value avoids `KeyError` exceptions when dealing with missing or null data in the API response.

**Why this matters:**
APIs will fail. Networks will be down. Data will sometimes be missing. Proper error handling will make your program robust and pleasant to use rather than fragile and confusing.

## What's Next?
You now have a functional SpaceX launch tracker now. Here are some ways you could enhance and expand it:
### Implement Rocket Type Filtering
The SpaceX API provides data about rockets. You could implement filtering for launches by particular rockets (Falcon 9, Falcon Heavy, Starship):
```python
def get_launches_by_rocket(rocket_name, limit=10):
    """Filter launches by rocket type"""
    all_launches = get_launches(limit=100, latest_first=True)
    filtered = [l for l in all_launches if rocket_name.lower() in l['name'].lower()]
    return filtered[:limit]
```
### Build a Web Interface
You could turn this into a simple Flask web app so users can see the launches in a browser, not in the terminal.
### Implement Launch Notifications
You could use the API to monitor new upcoming launches and send email or SMS notifications when they're announced.
### Implement Saving Launch History
You could save the launch data to a local SQLite database so you could monitor launches over time and see success rates.
### Visualize Launch Data
You could use `matplotlib` or `plotly` to make graphs showing:
- Launch success rates over time
- Number of launches per year
- Most used launch pads
### Investigate Other API Endpoints
The SpaceX API has endpoints for rockets, capsules, crew, and more. You can expand this tracker to show detailed rocket specifications or crew information.

## Conclusion
You have successfully built a SpaceX Launch Tracker that retrieves real-time data from the SpaceX API and displays it in a clean, readable format.

**What you learned:**
- How to work with REST APIs using the `requests` library
- How to parse and format JSON data
- How to handle errors gracefully in network requests
- How to structure a Python project with modular functions
- Best practices for working with external APIs

This project demonstrates core skills that apply to any API integration work. Whether you're building data dashboards, automation tools, or integrations with third-party services, the patterns you learned here will be useful.
The SpaceX API is just one of thousands of public APIs available. You can apply these same techniques to weather APIs, news APIs, stock market data, and more.

**Complete Code:**
The full code for this project is available on GitHub:
[Shruti-Rajpurohit/SpaceX_Launch_tracker](https://github.com/Shruti-Rajpurohit/SpaceX_Launch_tracker)

**Questions or Feedback?** 
Feel free to reach out:
- GitHub: [Shruti-Rajpurohit](https://github.com/Shruti-Rajpurohit)
- LinkedIn: [Shruti Rajpurohit](https://www.linkedin.com/in/shruti-rajpurohit-976b44226)
- Email: rajpurohitshruti20@gmail.com
