# Meraki SDWAN Extended Automation AUNR

## Internal Structure and File Organization
The web application structure is based on a clean architecture template based on the Flask Web Development 2nd edition book by Miguel Grinberg:
* **.env:** Stores the API keys for Meraki Dashboard and HERE Maps. This file also defines the organization number.
  * **Static:** Contains static files like the CSS, JavaScript, images, fonts and logos. (HERE Maps integration relies heavily on javascript, jquery and ajax)
  * **Templates:** Contains the HTML files for every page of the app. 
  * **API/endpoints.py:** Endpoints for the API and webhooks
  * **flaskapp.py:** The main python function of the program.
  * **flaskapp.wgsi:** Used for deployment on production server. 
  * **meraki_api_V0.py:** Python methods used to get and set data into the Meraki Dashboard. Methods in this file use the Meraki API v0.
  * **meraki_api_V1.py:** Python methods used to get and set data into the Meraki Dashboard. Methods in this file use the Meraki API v1.
  * **organization_data.py:** All the organization data (networks, hubs, etc) is represented using global variables and accessed through this file.
###### A note on HTML Files
For simplicity, HTML files use inheritance. The base template defines the sidebar and other items that remain the same accross all pages. The other HTML files within the templates folder extend the base file, and therefore redefine the body of the HTML.

## Personalization
This app can be deployed for different companies. To adapt the app for different companies the following components must be modified in the App Settings tab:
* Loss tolerance
* Latency tolerance
* Company name
* Company logo

After wirting the information required and uploading the company logo, it is necessary to submit the changes for the web app to change its template with the personalized information. 


## How to run:
The following list ilustrates how to run the code locally.

1. Clone this repository https://github.com/AtenasPerezCruz/meraki-webapp-flask.git
* Install Python 3.9.7
2. Create a virtual environment and activate it
    * Create virtual environment with venv
      ```sh
      python -m venv venv
      ```
      
    Activate using Powershell
      * Open Powershell as an Administrator and run the following command:
        ```powershell
        Set-ExecutionPolicy Unrestricted –Force
        ```
        
      * In the project folder, ensure the venv directory was created
      * Activate the virtual environment using       
        ```powershell
        . .\venv\Scripts\Activate.ps1
        ```        
    Activate using Linux
      * In a terminal window, cd to the project directory and run:        
        ```bsh
        . venv/bin/activate
        ```

3. Upgrade pip and install project's requirenments
   ```sh
    pip install --upgrade pip
    pip install -r requirements.txt 
    ```
4. Create the following environment variables:
  * It is important not to hardcode the api keys directly in any source file

    Name | Description
    ----------------|----------------------------
    SECRET_KEY | Hard to guess string (for Flask)
    MERAKI_API_KEY | API key used to access the Dashboard 
    MERAKI_ORG_ID | Organization ID  
    HERE_MAPS_API_KEY | API key issued from the HERE MAPS developer account. Must be a JAVASCRIPT API KEY. https://developer.here.com
    LOGO_URL | Organization logo path
    ADMIN_MAIL | Default admin email
    ADMIN_PASSWORD | Default admin password  
    ADMIN_NAME | Default admin name

  * Create a .env file which contains all the environment variables in order to keep the API_KEY information and the organization ID secure.
    We are using dotenv to read environment variables from a .env file, which is a local file that does not goes up into the github cloud in orden to keep them safe.
    We also store here the information about the API Key needed for Here Maps, the application that shows the map on the map monitoring module.
    The file must be written in the next format:
    
    .env contents:
    ```sh
    SECRET_KEY=SuperSecureSecretKeyForFlask
    MERAKI_API_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxx
    MERAKI_ORG_ID=xxxxxxx
    HERE_MAPS_API_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxx
    LOGO_URL=https://www.xxxxxxxxx.com/xxxxxxx.png
    ADMIN_MAIL=admin@xxxxx.com
    ADMIN_PASSWORD=UltraSuperSecurePassword    
    ADMIN_NAME=AdminName  
    ```
  
  5. Create FLASK_APP environment variable
  * To run the application with localhost, create another environment variable called FLASK_APP and assign the main    Python application, which is flaskapp.py

      Powershell:
      ```powershell
      $env:FLASK_APP=”flaskapp.py”    
      ```
      Linux:
      ```bash
      export FLASK_APP=”flaskapp.py”
      ``` 
      
  6. Create the database
      ```sh
      flask shell
      ```
      
      ```python
      db.create_all()
      Role.insert_roles()
      User.insert_admin_user()
      exit()
      ```
      
  7. Run de application on localhost (Only for development)
      ```sh
      flask run
      ```

      The application will run most likely on http://localhost:5000





