## 🤖 Deep-Learning Model

Download the Flickr8k dataset from [here](https://www.kaggle.com/datasets/adityajn105/flickr8k). and copy it to 
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
