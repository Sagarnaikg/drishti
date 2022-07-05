
# Drishti 

The Smart Application 📱 for Blind Assistance.

##

## 🗂 Project folders 
 - 📱 [Drishti Mobile App](https://awesomeopensource.com/project/elangosundar/awesome-README-templates)
 - ⚙️ [Drishti Backend Api](https://github.com/matiassingers/awesome-readme)
 - 🤖 [Drishti DL Model ](https:/bulldogjob.com/news/449-how-to-write-a-good-readme-for-your-github-project)

##
## Drishti Objective

A person 🧑🏻 with vision impairment or low vision generally requires a helper for their daily necessities. It's not practically possible that a helper is available most of the time to help them, so Drishti will acts as an assistant to the visually impaired by captioning the images 🏞 and assisting the information with voice 🗣.

##
## How's the app 📱 works is 
In a nutshell, the smart app will guide the visually 
impaired through voice messages 🗣. When user opens drishti app 📱
it will give short introduction about it 🙋‍♀️ and will ask the 
users to give their name and then ask them to say the command 🗣
“START DRISHTI” to 
start the application.

Now, the app 📱 takes users to the main page, where 
the real magic happens. User need to say the command "Drishti take 
a picture 🏞" then it will take a picutre and after few seconds 
it will describe the taken image through voice message 🗣.When 
user says "Drishti stop", tha app 📱 will close.

When the app takes a picutre 🏞, it will sent it to drishti backend ⚙️ 
where it is processed and will send a caption text back to app 📱 within 
fraction of sections. Finally app recives the text message and 
will convey it to user through voice message 🗣. This processe repeats 
whenever user takes a picutre.

##
## App Features

- Fully controlled using voice commands 🗣.
- 📱 App is avialable for both android and ios users.
- 📱 App can be setuped with simple few steps.
- 📱 App will take just a few seconds to convert image into sentences


##
## Tech Stack

📱 **Mobile App:** Flutter, Dart

⚙️ **Backend Api:** Flask, Python

🤖 **DL model:** TensorFlow, Keras, ResNet50, RNN

##
## Flow Diagram
![flowchart](https://github.com/Sagarnaikg/drishti/blob/main/flowchart.png)
##
## Deep-Learning Model

Download the Flickr8k dataset from [here](). and copy it to 
`Flickr_Data` folder inorder to run the notebook file.

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
## Backend API

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
## Drishti Mobile App

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



Libraries used in the project
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
Please Have a loook at our papper work on drishti [here]().


