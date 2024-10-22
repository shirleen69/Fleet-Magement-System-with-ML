# Fleet-Magement-System-with-ML
An IOT and ML based project. Hardware components substituted out to create a working prototype.

Create a thingspeak account and get the write API key.
https://fledge-iot.readthedocs.io/en/develop/plugins/fledge-north-ThingSpeak/index.html

**Create a Telegram Bot and get a Bot Token**
1. Open Telegram application then search for @BotFather
2. Click Start
3. Click Menu -> /newbot or type /newbot and hit Send
4. Follow the instruction until we get message like so
    Done! Congratulations on your new bot. You will find it at t.me/new_bot.
    You can now add a description.....
    
    Use this token to access the HTTP API:
    63xxxxxx71:AAFoxxxxn0hwA-2TVSxxxNf4c
    Keep your token secure and store it safely, it can be used by anyone to control your bot.

5. For a description of the Bot API, see this page: https://core.telegram.org/bots/api
    So here is our bot token 63xxxxxx71:AAFoxxxxn0hwA-2TVSxxxNf4c (make sure we don't share it to anyone).

**Get Chat ID for a Private Chat**
1. Search and open our new Telegram bot
2. Click Start or send a message
3. Open this URL in a browser https://api.telegram.org/bot{our_bot_token}/getUpdates
    See we need to prefix our token with a word bot
    Eg: https://api.telegram.org/bot63xxxxxx71:AAFoxxxxn0hwA-2TVSxxxNf4c/getUpdates
4. We will see a json like so
          {
            "ok": true,
            "result": [
              {
                "update_id": 83xxxxx35,
                "message": {
                  "message_id": 2643,
                  "from": {...},
                  "chat": {
                    "id": 21xxxxx38,
                    "first_name": "...",
                    "last_name": "...",
                    "username": "@username",
                    "type": "private"
                  },
                  "date": 1703062972,
                  "text": "/start"
                }
              }
            ]
          }
5. Check the value of result.0.message.chat.id, and here is our Chat ID: 21xxxxx38
6. Let's try to send a message: https://api.telegram.org/bot63xxxxxx71:AAFoxxxxn0hwA-2TVSxxxNf4c/sendMessage?chat_id=21xxxxx38&text=test123
7. When we set the bot token and chat id correctly, the message test123 should be arrived on our Telegram bot chat.

The project is an integrated face recognition and classification system that leverages IoT technology, webcam functionality, and data visualization through Thingspeak. Here’s a detailed summary of its components and functionalities:

### Overview of the Project

1. **Face Recognition and Classification:**
   - The core functionality of the project revolves around recognizing employees and classifying them as either employees or non-employees using deep learning models.
   - The project uses a face recognition model trained on employee images and a classification model that helps distinguish between employees and non-employees.

2. **Data Capture:**
   - The system allows for capturing employee data through a user-friendly interface. Employees can be registered by capturing their images, which are stored in a designated directory for further training of the face recognition model.
   - This data is essential for continuously improving the accuracy of the recognition and classification models.

3. **Webcam Integration:**
   - A webcam is used for real-time monitoring and capturing images. It detects faces in the video feed and processes these images using the trained models to identify individuals.
   - The system can trigger alerts when a non-employee is detected, enhancing security measures within the monitored area.

4. **Alert Mechanism:**
   - The system sends alerts via Telegram when a non-employee is detected. The alert includes an image of the detected individual, the alert message, and the occupancy location (though occupancy data is collected separately).
   - This real-time notification system ensures immediate action can be taken when unauthorized individuals are identified.

5. **IoT Aspect:**
   - The project utilizes IoT principles by connecting the webcam and the processing unit (likely a Raspberry Pi or similar device) to form a smart monitoring system.
   - A GPS system is also used to send realtime locations of the vehicle.
   - The ability to send alerts and potentially control physical systems (like locks or alarms) demonstrates the project's IoT capabilities.

6. **Thingspeak Integration:**
   - Thingspeak is used for visualizing and monitoring occupancy data and system performance over time.
   - Data from the monitoring system can be sent to Thingspeak, where it can be visualized through various graphs and dashboards, providing insights into occupancy trends and security events.

7. **User Interaction:**
   - The system offers a command-line interface that allows users to capture employee data, train models, or start monitoring.
   - This interactive approach ensures that users can easily manage the system and respond to different operational needs.

### Summary
Overall, it combines face recognition, deep learning, IoT technology, and data visualization to create a robust employee monitoring system that enhances security and operational efficiency. By integrating real-time alerts and using a webcam for monitoring, the project effectively addresses potential security concerns while providing valuable insights through Thingspeak.

****Detailed summary****


This project is a face recognition and classification system designed for monitoring occupancy and identifying employees in specific locations. It primarily uses computer vision techniques, machine learning models, and Telegram for real-time alerts. Here’s a detailed summary of the key components and functionality:


### 1. **Face Recognition and Classification System**

- **Purpose**: The system is designed to identify whether a person captured by a camera feed is an employee or a non-employee. This identification helps in monitoring occupancy and ensuring that only authorized personnel are in designated areas.

- **Components**:

- **Face Recognition Model**: Uses a Support Vector Machine (SVM) classifier to recognize employee faces from a dataset of labeled employee images.

- **Face Classification Model**: A convolutional neural network (CNN) model, built with Keras, classifies detected faces as either employees or non-employees. This model is trained on employee data and supplemented with synthetic or random non-employee data for binary classification.


### 2. **Data Capture and Training Pipeline**

- **Employee Data Capture**: The system provides a utility to capture employee face data using a camera. This captured data is stored in a structured directory format to facilitate training.

- **Model Training**:

- **Face Recognition Model Training**: The SVM model is trained with the captured employee images. The model assigns a unique label to each employee based on their identity, which is later used for identification.

- **Face Classification Model Training**: A CNN model is trained to differentiate between employees and non-employees. Training can be triggered by the user, with options to specify the number of epochs for each training session.


### 3. **Monitoring and Real-Time Alerts**

- **Occupancy Monitoring**: After the models are trained, the system continuously monitors the camera feed. It detects faces in real-time using OpenCV’s Haar Cascade classifier and then uses the SVM and CNN models to identify and classify these faces.

- **Alert System with Telegram Integration**:

- If a non-employee is detected, the system sends an alert via Telegram.

- The alert includes details such as:

- **Location**: The specific monitored area where the non-employee was detected.

- **Occupancy Count**: The number of individuals detected at that moment.

- **Image**: A photo of the non-employee for verification.

- The alert system is designed to notify designated personnel quickly, allowing for immediate action if necessary.


### 4. **User Interaction and Control**

- **Interactive Command Options**:

- **Capture Employee Data**: Prompts the user to input employee details and capture face images for training.

- **Train Models**: Initiates the training of both the face recognition and classification models, allowing for model retraining when new employee data is added.

- **Start Monitoring**: Starts the real-time monitoring process. This option checks that the models are loaded, and if they aren’t, it prompts the user to train them first.

- **Configuration via Environment Variables**: The project uses a configuration file or environment variables to manage sensitive information such as the Telegram bot token, API keys, and other configurations, ensuring that these are not hardcoded in the main codebase.


### 5. **Deployment and Execution**

- **Modular Design**: The project is structured with multiple Python modules, each handling a specific aspect, such as data capture, model training, and monitoring. This modularity makes it easy to maintain and extend.

- **Scalability and Customization**: The system is designed to be flexible, allowing for the addition of new employees, retraining of models, and customization of alert messages and formats. The project’s design could also be expanded to include more sophisticated models, additional alert mechanisms, or integration with other systems.


### 6. **Use Case and Applications**

- **Security and Access Control**: Helps in restricting access to certain areas by alerting authorities when non-employees are detected.

- **Occupancy Management**: Useful for facilities management, ensuring that only authorized personnel are present in sensitive areas.

- **Real-Time Surveillance**: Provides a continuous monitoring solution with instant alerts, which is beneficial for security personnel and facility managers who require quick responses.


Overall, it offers a robust framework for face recognition and classification with a real-time alerting system, making it ideal for various applications where access control and security are essential.

![Untitled Diagram drawio(7)](https://github.com/user-attachments/assets/99550a43-45fd-425c-9ef2-8be731998d73)


