# Sign-Language-Interpreter
Translate American Sign Language to text natively on android

We initially planned to work with Pakistani Sign Language although data availability was a huge
concern starting out. There are tutorial videos available on psl.org.pk but there are only single
samples of signs. To see if we could get data from real sign language speakers, we contacted
PSL on April 13th. The PSL correspondent briefed us that they do not have the data as of now
but can collect it with our coordination. The tentative time required to collect the data would be 6
to 8 months. This was not feasible for us given the time constraints for the project. Hence we
decided to ditch PSL for American Sign Language for the purposes of our project.

The MNSIT American Sign Language letter database of hand gestures represent a multi-class
problem with 24 classes of letters (excluding J and Z which require motion). The dataset format
is patterned to match closely with the classic MNIST. Each training and test case represents a
label (0-25) as a one-to-one map for each alphabetic letter A-Z (and no cases for 9=J or 25=Z
because of gesture motions). The total data is approximately half the size of the standard Digit
MNIST dataset. Details about the data are in the table below:

| Train Images | Test Images | Image Size |
| -------------|:-----------:| ----------:|
| 27,455       | 7172        | 28x28      |

The data is provided in the form of CSV file with all pixels stored against each image label. We
converted the array data to images using a python script. A bash script was used to sort out the
various categories into test and train sets. The images were already preprocessed and greyscaled.

## Classification Technique

We fine tuned InceptionV3 model to our dataset using Tensorflow. We achieved 93% top-1
accuracy with the test data. However, the performance of our model in real time was abysmal.
We attribute this low accuracy to three factors:
1. We used cropped out images of hands to train our model. However, in real time, we
were trying to classify the image containing the hand as well as the person's face and
arms. Obviously the model was not trained to detect hand signs in a complete image.
We plan to rectify this problem by performing detection of hand within the image instead
of plain classification of whole image.
2. We think that the low resolution of 28x28 was insufficient to detect the features required
to distinguish between various signs. We found the unprocessed data on which the
MNIST dataset was based on. But this data came with its own set of problems. The data
was unprocessed. The hands in the images had to be cropped out by us
3. Our real time accuracy improved somewhat using this original colored images but the
results were still not satisfactory. Many visually similar signs were being misclassified.
We think we don’t have sufficient data to distinguish between almost identical hand 
signs. To improve our results, we are experimenting with dropping out some visually
similar hand signs.

## Timeline

###### 23rd Mar - 29st Mar: Data Collection
Extraction of frames from videos available on PSL(psl.org.pk) or ASL.
These frames will then be treated as images. This would be our
initial dataset. However, we are looking into more sources for data
as this alone might not be enough.

###### 3st Apr - 14th April: A Working Classifier
We will train and finetune a basic classifier that takes a picture as
input and outputs the corresponding word. These words will not form
coherent sentences (that is a completely different domain).The words will
be fed to an existing text to speech converter.

###### 20th Apr - 30th Apr: Mobile Application
Work on a basic camera application will start in parallel with the
previous milestone. However in this timeframe, the classifier will be
integrated and tested with the basic camera app. The app will capture
live images and pass them to the classifier.

###### 1st May - ... ...: Attempt to Classify Video Input
In the final phase, we will try to process video input. Frames will be
extracted from the video at regular intervals and fed to the classifier. _<Milestone delayed due to ESEs>_
