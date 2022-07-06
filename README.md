
# Drishti 

The Smart Application 📱 for Blind Assistance.

# Demo
<img src="https://github.com/Sagarnaikg/sagarnaikg/blob/main/assets/drishti-test.png" alt="PS5"/>

##

## 🗂 Project folders 
 - 📱 [Drishti Mobile App](https://github.com/Sagarnaikg/drishti-app)
 - ⚙️ [Drishti Backend Api](https://github.com/Sagarnaikg/drishti-api)
 - 🤖 [Drishti DL Model ](https://github.com/Sagarnaikg/drishti/tree/main/drishti-dl-model)

##
## Drishti Objective

A person with low vision or vision impairment often needs a caregiver's assistance for their everyday needs. Because a helper is not always accessible to aid them, Drishti will function as an assistant to the visually impaired by captioning their surroundings and will convey the information in the form of speech.

##
## How's the app 📱 works is 
In a nutshell, the smart programme will guide the visually handicapped via audio communications. When a user launches the Drishti app, it will offer a brief introduction to the app, ask for their name, and then ask them to speak the command "START DRISHTI" to launch the application.


The app now redirects visitors to the main app screen, where the actual magic takes place. The user must utter the command "Drishti take a photo," and it will snap a picture and describe it through a voice message after a few seconds. The app will close when the user says "Drishti stop."

##
## App Features

- Voice commands are used to control everything 🗣.
- 📱 Both Android and iOS users may access the app.
- 📱 The app may be set up in a few simple steps.
- 📱 The app will turn an image into phrases in a matter of seconds.


##
## Tech Stack

📱 **Mobile App:** Flutter, Dart

⚙️ **Backend Api:** Flask, Python

🤖 **DL model:** TensorFlow, Keras, ResNet50, RNN

##
## Flow Diagram
![flowchart](https://github.com/Sagarnaikg/drishti/blob/main/flowchart.png)
##
## 🤖 Deep-Learning Model

Download the Flickr8k dataset from [here](https://www.kaggle.com/datasets/adityajn105/flickr8k). and copy it to 
`Flickr_Data` folder in-order to run the notebook file.

Our deep learning model contains main two steps.

I. Image feature extraction
```bash
## import ResNet50 library
from keras.applications import ResNet50
incept_model = ResNet50(include_top=True)

## Create a custom model by slicing off the last layer
last = incept_model.layers[-2].output
modele = Model(inputs = incept_model.input,outputs = last)

## extract the features from the image
for i in images:
    img = cv2.imread(i)
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    img = cv2.resize(img, (224,224))
    
    img = img.reshape(1,224,224,3)
    pred = modele.predict(img).reshape(2048,)
        
    img_name = i.split('/')[-1]
    
    images_features[img_name] = pred
```

II. Generate a sentence from features vector

```bash
def generator(photo, caption):
    n_samples = 0
    
    X = []
    y_in = []
    y_out = []
    
    for k, vv in caption.items():
        for v in vv:
            for i in range(1, len(v)):
                X.append(photo[k])

                in_seq= [v[:i]]
                out_seq = v[i]

                in_seq = pad_sequences(in_seq, maxlen=MAX_LEN, padding='post', truncating='post')[0]
                out_seq = to_categorical([out_seq], num_classes=VOCAB_SIZE)[0]

                y_in.append(in_seq)
                y_out.append(out_seq)
            
    return X, y_in, y_out
```

##
## ⚙️ Backend API

Our backend is built using `flask` library and 
deployed on `heroku` server

```bash
app = Flask(__name__)

@app.route('/upload', methods=['POST', 'GET'])
def upload_file():
    ....
```

## API Reference

Getting the API connection status

```http
  GET /
```
Retruns
| Key | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `message` | `string` | API connection status |

Post an Image and get a caption

```http
  PUT /upload
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `image`      | `file` | **Required** Image file  |

Retruns
| Key | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `result` | `string` | Image caption |

##
## 📱 Drishti Mobile App

Mobile App built using Flutter framework. 

App Folder Structure
```
app
├── resources
│   ├── app_media.dart 
│   ├── colors.dart
│   ├── font_weights.dart
│   └── theme_data.dart
├── screens
│   ├── home
│   │   ├── bloc.dart
│   │   └── screen.dart
│   └── init
├── utils
│    └── strring_msg_constants.dart
└── main.dart
```



📦 Libraries used in the project
- flutter_speech
- flutter_tts
- flare_flutter
- camera
- rxdart
- async
- http


For help getting started with Flutter, refer
[online documentation](https://flutter.dev/docs), which offers tutorials,
samples, guidance on mobile development, and a full API reference for flutter.
##

## Refrences
Please Have a loook at our papper work on drishti [here](https://github.com/Sagarnaikg/drishti/blob/main/DRISHTI%20-%20paper_publish.pdf).


