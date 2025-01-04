# Berry Classifier – Upload Fast.ai Model to Streamlit

This guide contains the key commands for how to upload a fast.ai model to Streamlit. See this [blog post](https://medium.com/ai-mind-labs/deploy-your-fast-ai-model-on-streamlit-32d9e2effa5d) for a full walkthrough.

## Step #1: Export your Model

When you have finished training your model, you need to export it. This is easily done with the learn.export() command:

```python
learn.export()
```

This saves your model as the default name export.pkl. If you are using Kaggle Notebooks, you can download your file to disk by going to the data section.

## Step #2: Create a new Streamlit Project

Create a new folder with your project name.

Drag and drop your model into this folder and add a new file called requirements.txt. This files tells the computer what packages you need to run this project. Copy and paste this into your file:
```
fastai==2.7.12
fastcore==1.5.29
streamlit==1.22.0
altair<5
```

Then, open up a new terminal window and navigate to your folder. Running `pip install -r requirements.txt` will make sure you download all required packages.
```shell
pip install -r requirements.txt
```

Next, create a new file called `app.py`.

First you need to import streamlit to test that it works. Import the library and add a title.

```python
import streamlit as st

## STREAMLIT
st.title("Berry Classifier! 🍓")
To start the streamlit app, run the following command in your terminal:
```

```shell
streamlit run app.py
```

You should see a title popping up!

## Step #3: Use Your Model to Make Predictions

Next, let’s add the code to upload an image to our model! Here is the full code:

```python
from fastcore.all import *
from fastai.vision.all import *
import streamlit as st

## LOAD MODEl
learn_inf = load_learner("export.pkl")
## CLASSIFIER
def classify_img(data):
    pred, pred_idx, probs = learn_inf.predict(data)
    return pred, probs[pred_idx]
## STREAMLIT
st.title("Berry Classifier! 🍓")
bytes_data = None
uploaded_image = st.file_uploader("Choose your berry:")
if uploaded_image:
    bytes_data = uploaded_image.getvalue()
    st.image(bytes_data, caption="Uploaded image")   
if bytes_data:
    classify = st.button("CLASSIFY!")
    if classify:
        label, confidence = classify_img(bytes_data)
        st.write(f"It is a {label}! ({confidence:.04f})")
```

This app uses several components:

- First, we load the model using `learn_inf = load_learner("export.pkl")`
- `st.file_uploader` lets us upload an image.
- `st.image` takes our image and display it to the user.
- Only if we have uploaded an image will the st.button named “CLASSIFY” appear. This button triggers the function `classify_img` with our image as parameter.
- The `classify_img` function uses the fastai learn_inf.predict.
- The returned prediction and probability is then written to the screen.

That was something! Hopefully we got a working local copy!

## Step #4: Deploy on Streamlit Cloud

Streamlit offers free deployment of Streamlit apps!

The first step is to upload your code to GitHub. I recommend using GitHub Desktop to handle and push your repository to the website! Next, head over to the streamlit website and connect your GitHub account. There, you will be able to add a new app. Fill in the details and hit “Deploy!” to spin up your Streamlit project.

After a couple of minutes, you should have your streamlit app up and running! 🥳

## Step #5: Share your Creation

Now, you have a link you can share to show everyone your awesome project! Here is my demo [Berry Classifier](https://berry-classifier.streamlit.app/).

Amazing job!!!

Thanks for reading this post! I wish you very well on your continued journey!
