---
title: "Secure Website Development for Migrant Shelter"
excerpt: "The project revolves around developing a secure system to track and manage all migrants while keeping data safe. <br/><img src='https://pbs.twimg.com/media/Ff7h37qXEAM9Yvc.jpg'>"
collection: portfolio
---

# PROBLEM STATEMENT
------------------------------------
Migrant Shelters are crucial for attending to and providing services for migrants who have traveled a long way to reach a safe destination. The shelters contain sensitive information, including migrants' identities and needs, and catastrophic consequences may occur if data is not protected.

Casa Monacarca wanted to combine technology and cryptography to develop a platform that ensures data safety and integrity. 

# INTRODUCTION
-----------------------
Massive data circulation has caused uncertainty among organizations. It has raised discussions of the importance of implementing solutions to protect personal data; cryptographic algorithms have been created to safeguard data, and they have been around for many years. For instance, Julius Caesar is known for encrypting military messages.

Casa Monarca, a migrant shelter, is aware of the implications of data leakage and the disastrous consequences it can bring to the migrants and their families, so they are taking action to implement a robust platform at a low cost. 

The platform's primary goal is to keep track of registered migrants while protecting their information. The functionalities of the platform are registering a new migrant with a registration form provided, looking at a migrant to determine if they passed by the shelter, registering additional services that can be required by migrants, such as psychological, legal, and educational services, generating reports to know how many migrants are currently in the house and generate a prediction to estimate the food and staff required, change migrant status to inactive when they leave the shelter, and finally modify users in case they gain or lose privileges.

# CONTRIBUTIONS
-------------------
My contributions included building the backend functions in Python, which were later connected to JS through an API. Due to security reasons, some parts of the code can't be posted, but screenshots of the functioning platform can be viewed in the results section.

My main contributions were connecting to the PostgreSQL database functions to insert and edit data and the functions to encrypt and decrypt data. Also, when looking up a migrant, I used Levenshtein distance to reduce errors in name spelling. I also created a PowerBI dashboard that uses Python to connect and decrypt data and get relevant data. Finally, I created a machine learning model to predict the number of migrants in the next few days.

# BACKGROUND
-----------------------
Before planning and making an interface, a cybersecurity framework needs to be established and followed; since much information can be displayed, we need to segment and determine user permissions. The framework chosen was the CIA Triad, which sets three primary rules:
Confidentiality: Only authorized users can access and modify data.
Integrity: Data correctness is a must. No data should be modified by accident or with malicious intentions.
Availability: Privileged users can access data whenever it is needed.

With the framework in mind, the first approach to determining the roles of users:
**User (level 1)**: Only Migrant registration and migrant lookup.
**Service (level 2)**: Migrant registration, migrant lookup, and register additional services.
**Admin (level 3)**: Migrant registration, migrant lookup, registering additional services, generating reports, modifying user roles, and changing migrant status.

After determining the framework, research was made to determine a suitable, robust encryption algorithm. AES was the most appropriate. It is a symmetric encryption algorithm, and because of its rapid encryption and decryption, it has proven resistant to many attacks. It is considered the most secure encryption algorithm available today.

# Theoretical framework
-----------------------------
empty


# Function explanation 
---------------------------
## Database Connection 
The project was developed using PostgreSQL because of the excellent capabilities and the facilitation of integration with Python using the psycopg2 library and .ini files to keep the configuration hidden from the code. Once the connection has been made, queries can be made to the desired DB. I proposed having different databases in case one gets leaked, and the hackers access a tiny part of the information. With the idea of having different databases, I had to generate a random ID to establish their relationship and make the necessary joins. Due to random IV (initialization vector), the encryption will turn out differently even though the ID is the same.

## ID Generation
As mentioned earlier, the ID generation was used for backend purposes. To achieve this without having a sequential counter, I used the random library to generate a 6-character random ID, including lower-case and capital letters and numbers.
```python 
def ge_id():
 c = string.ascii_letters + string.digits  
 id _gen= ''.join(random.choices(c, k=6))  
    return id_gen
```
## Key Generation
AES can handle different keys, but it is more secure if the key has more bits. So, to apply the AES, a 256-bit key was created by defining a 50-character long password, followed by applying SHA-256 for the password, ensuring that the resulting hash can't be reversed to get the password. Afterward, PBKDF2 with 10,000 iterations was applied to the hash and a salt to generate a 256-bit key, which was stored in a .env file as a base64 to hide it from the code.

## Encryption Function
## Decryption Function

## Migrant lookup with Levenshtein distance
A key function of the platform is being able to look up a migrant, whether to add services (psychological, educational, legal), register departure, or just check if they passed through the shelter. This is because they usually disappear while getting to their destination, and their relatives want to check where they got lost. 

In the lookup function, two fields are required: complete name and birth date. However, a problem arises: names and surnames are problematic when typing them down because of accents or the way of writing them. To solve this problem, I implemented the Levinstein distance with all the records from the database to obtain the name with the minimum distance. After finding the name, the birth date is the final filter to determine if that migrant is registered.

## Add Services
The services required by migrants may vary and are very specific, ranging from person to person. To implement this, it combines the lookup function to add services to a registered migrant. The selected services are then encrypted and sent to a different database using the ID as the key to establish relations between databases. 

## Change Status
Another key feature is keeping track of the migrants inside the shelter; in the backend, a column named active is set to 1, but users must change the status when they leave the shelter. To achieve this, there's an option that changes the status and adds the exit date by using the migrant  lookup. If the migrant is found, the column changes the status to 0, adding the exit date. Those values are encrypted, and a UPDATE query is made to replace the active and exit date values. 

## PowerBI
To register all the migrants who used the shelter and provide descriptive statistics in real time, an aggregated value was added with a PowerBI dashboard that connects to the encrypted DB. With Python integration, the decryption function was used to produce a report.
 
<iframe title="MIGRANTES_FINAL (1)" width="600" height="373.5" src="https://app.powerbi.com/view?r=eyJrIjoiOTg5NDE0MDctNjI3YS00YzlkLTlhYTgtYWMyZGU0Nzc5MzM2IiwidCI6ImM2NWEzZWE2LTBmN2MtNDAwYi04OTM0LTVhNmRjMTcwNTY0NSIsImMiOjR9" frameborder="0" allowFullScreen="true"></iframe>

## Migrant Forecast
Another helpful feature for future planning was incorporating Facebook's forecasting model, Prophet, which estimates how many migrants will come to the shelter in the following days to plan and be prepared with the necessary food and staff. To achieve this, I made a function that counts all the registered migrants on a given day and uses the migrant count per population type (male adults, female adults, male children, female children) as an input for the model to predict $$X$$ given days. Returning an interactive HTML graph with the population type.

<iframe src="https://raw.githubusercontent.com/axelqc/helper_ozone/refs/heads/main/evolucion_ingresos%20(2).html" width="600" height="373.5"></iframe>

# RESULTS
------------------------------
empty
