---
title: "Secure Website Development for Migrant Shelter"
excerpt: "The project revolves around developing a secure system to track and manage all migrants while keeping data safe. <br/><img src='https://www.iom.int/sites/g/files/tmzbdl2616/files/styles/large_900_image_crop/public/press_release/media/2023-09/20230703_pacaraima_roraima_brasil_36_nueva.jpg?h=bc09f3d1&itok=5Y4hmkAk'>"
collection: portfolio
---
The project revolves around developing a secure system to track and manage all migrants while keeping data safe.

# Problem statement
------------------------------------
Migrant Shelters are crucial for attending to and providing services for migrants who have traveled a long way to reach a safe destination. The shelters contain sensitive information, including migrants' identities and needs, and catastrophic consequences may occur if data is not protected.

The shelter wanted to combine technology and cryptography to develop a platform that ensures data safety and integrity. 

# Introduction
-----------------------
Massive data circulation has caused uncertainty among organizations. It has raised discussions of the importance of implementing solutions to protect personal data; cryptographic algorithms have been created to safeguard data, and they have been around for many years. For instance, Julius Caesar is known for encrypting military messages.

The migrant shelter, is aware of the implications of data leakage and the disastrous consequences it can bring to the migrants and their families, so they are taking action to implement a robust platform at a low cost. 

The platform's primary goal is to keep track of registered migrants while protecting their information. The functionalities of the platform are registering a new migrant with a registration form provided, looking at a migrant to determine if they passed by the shelter, registering additional services that can be required by migrants, such as psychological, legal, and educational services, generating reports to know how many migrants are currently in the house and generate a prediction to estimate the food and staff required, change migrant status to inactive when they leave the shelter, and finally modify users in case they gain or lose privileges.

# Contributions
-------------------
My contributions included building the backend functions in Python, which were later connected to JS through an API. Due to security reasons, some parts of the code can't be posted, but screenshots of the functioning platform can be viewed in the results section.

My main contributions were connecting to the PostgreSQL database functions to insert and edit data and the functions to encrypt and decrypt data. Also, when looking up a migrant, I used Levenshtein distance to reduce errors in name spelling. I also created a PowerBI dashboard that uses Python to connect and decrypt data and get relevant data. Finally, I created a machine learning model to predict the number of migrants in the next few days.

# Background
-----------------------
Before planning and making an interface, a cybersecurity framework needs to be established and followed; since much information can be displayed, we need to segment and determine user permissions. The framework chosen was the CIA Triad, which sets three primary rules:
- **Confidentiality**: Only authorized users can access and modify data.
- **Integrity**: Data correctness is a must. No data should be modified by accident or with malicious intentions.
- **Availability**: Privileged users can access data whenever it is needed.

With the framework in mind, the first approach to determining the roles of users:
- **User (level 1)**: Only Migrant registration and migrant lookup.
- **Service (level 2)**: Migrant registration, migrant lookup, and register additional services.
- **Admin (level 3)**: Migrant registration, migrant lookup, registering additional services, generating reports, modifying user roles, and changing migrant status.

After determining the framework, research was made to determine a suitable, robust encryption algorithm. AES was the most appropriate. It is a symmetric encryption algorithm, and because of its rapid encryption and decryption, it has proven resistant to many attacks. It is considered the most secure encryption algorithm available today.


# Functions explanation 
---------------------------
## Database connection 
The project was developed using PostgreSQL because of the excellent capabilities and the facilitation of integration with Python using the psycopg2 library and .ini files to keep the configuration hidden from the code. Once the connection has been made, queries can be made to the desired DB. I proposed having different databases in case one gets leaked, and the hackers access a tiny part of the information. With the idea of having different databases, I had to generate a random ID to establish their relationship and make the necessary joins. Due to random IV (initialization vector), the encryption will turn out differently even though the ID is the same.

## ID generation
As mentioned earlier, the ID generation was used for backend purposes. To achieve this without having a sequential counter, I used the random library to generate a 6-character random ID, including lower-case and capital letters and numbers.
```python 
def gen_id():
 c = string.ascii_letters + string.digits  
 id_gen= ''.join(random.choices(c, k=6))  
    return id_gen
```
## Key generation
AES can handle different keys, but it is more secure if the key has more bits. So, to apply the AES, a 256-bit key was created by defining a 50-character long password, followed by applying SHA-256 for the password, ensuring that the resulting hash can't be reversed to get the password. Afterward, PBKDF2 with 10,000 iterations was applied to the hash and a salt to generate a 256-bit key, which was stored in a .env file as a base64 to hide it from the code.

## Encryption function
The encryption function was created on a separate script and then imported to the main code to hide it. The function receives the plain text or it can also recieve images and transforms them into bytes, and the generated key. After receiving both parameters, it generates a random initialization vector of 16 bytes. It adds padding to the plain text to ensure it is 16 bytes. Then, the cipher is created with the key and initialization vector and utilized to encrypt the text. It returns both the initialization vector and the ciphertext. The IV is insufficient to decrypt the text, so there is no need to hide it.

## Decryption function
This function was also created separately and imported into the main script. It receives the key and the encrypted text (IV + ciphertext), which are then separated. A decryptor is created to retrieve the padded plaintext, which is then removed to obtain the plain text. Since we are dealing with large volumes of encrypted data, this function has to decrypt every row and column from the data frame to decrypt everything.


## Migrant lookup with Levenshtein distance
A key function of the platform is looking up a migrant, whether to add services (psychological, educational, legal), register departure, or check if they passed through the shelter. This is because they usually disappear while arriving, and their relatives want to check where they got lost. 

In the lookup function, two fields are required: complete name and birth date. However, a problem arises: names and surnames are problematic when typing them down because of accents or the way they are written. To solve this problem, I implemented the Levinstein distance with all the records from the database to obtain the name with the minimum distance; this is helpful because it will return the most similar names registered, if any. After finding the "correct" name, the birth date is the final filter to determine if that migrant is registered.

## Add services
The services required by migrants may vary and are very specific, ranging from person to person. To implement this, it combines the lookup function to add services to a registered migrant. The selected services are then encrypted and sent to a different database using the ID as the key to establish relations between databases. 

## Change status
Another key feature is keeping track of the migrants inside the shelter; in the backend, a column named active is set to 1, but users must change the status when they leave the shelter. To achieve this, there's an option that changes the status and adds the exit date by using the migrant  lookup. If the migrant is found, the column changes the status to 0, adding the exit date. Those values are encrypted, and a UPDATE query is made to replace the active and exit date values. 

## PowerBI
To register all the migrants who used the shelter and provide descriptive statistics in real time, an aggregated value was added with a PowerBI dashboard that connects to the encrypted DB. With Python integration, the decryption function was used to produce a report.

## Migrant Forecast
Another helpful feature for future planning was incorporating Facebook's forecasting model, Prophet, which estimates how many migrants will come to the shelter in the following days to plan and be prepared with the necessary food and staff. To achieve this, I made a function that counts all the registered migrants on a given day and uses the migrant count per population type (male adults, female adults, male children, female children) as an input for the model to predict $$X$$ given days. Returning an interactive HTML graph with the population type.

```python 
def fit_prophet_model(data, column, dias):
  if column != 'hombres': 
      data[column] += np.random.normal(0, data[column].std() * 0.1, size=len(data))
      df_prophet = data.reset_index().rename(columns={'Fecha ': 'ds', column: 'y'})

      cap_value = df_prophet['y'].max() * 1.5  
      df_prophet['cap'] = cap_value

      model = Prophet(
          growth="logistic",
          seasonality_mode='multiplicative',  
          changepoint_prior_scale=0.5,  d
          interval_width=0.95  
      )
      model.fit(df_prophet)
      future = model.make_future_dataframe(periods=dias)  
      future['cap'] = df_prophet['cap'].iloc[0]  
      forecast = model.predict(future)
  elif column == 'hombres': # Male behavior is different
      data[column] += np.random.normal(0, data[column].std() * 0.1, size=len(data))
      df_prophet = data.reset_index().rename(columns={'Fecha ': 'ds', column: 'y'})

      model = Prophet(
          growth="linear")
      model.fit(df_prophet)
      future = model.make_future_dataframe(periods=dias)  
      forecast = model.predict(future)

  forecast = forecast[['ds', 'yhat']]
  return model, forecast

# Store models and predictions
models = {}
forecasts = {}

dias = 47 # Number of days

# Train for each column
for column in df.columns[1:-1]:
    model, forecast = fit_prophet_model(df, column, dias)
    models[column] = model
    forecasts[column] = forecast
total_forecast = pd.DataFrame({'ds': forecasts[list(forecasts.keys())[0]]['ds']})
total_forecast['yhat'] = sum(forecast['yhat'] for forecast in forecasts.values())f
```


# Results


## Log in
To log in, one must have an account, and an administrator must add his password to authorize and create it.

![login](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture.PNG?raw=true)


## User view
The user with the lowest permissions can only register migrants and look them up.
![user](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture2.PNG?raw=true)

When clicking on register new migrant, a form must be filled out to register them.
![form](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture3.PNG?raw=true)

When clicking on migrant lookup and filling in both parameters, for example, lookup for "Danyel," it retrieves "Daniel" because of the Levenshtein function. If found, it will return the decrypted information on the person.

![lookup](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture4.PNG?raw=true)

![result](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture45.jpg?raw=true)



## Service view
Moving on to the second type of user, it has one more option available: the service tab, which can add additional required services: therapy, education, legal, and jobs.
![level2](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture6.PNG?raw=true)

 An example is shown of adding services for somebody.

First, lookup.
![service_menu](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture7.PNG?raw=true)

Confirming its the right person.
![confirm](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture8.PNG?raw=true)

Second, add more services.
![more services](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture9.PNG?raw=true)

Third, slect the type of service.
![selected](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture10.PNG?raw=true)

Finally checked that the changes were applied.
![added](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture11.PNG?raw=true)

## Admin view
The admin has many more options, including descriptive statistics, predictions, user management, and migrant checkout.

![adminview](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture12.PNG?raw=true)

Embeded PowerBI report.

![powerbi](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture13.PNG?raw=true)

Embeded predictions.
![prophet](https://github.com/axelqc/helper_ozone/blob/main/capturas/Capture14.PNG?raw=true)