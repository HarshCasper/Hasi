# Hasi
[![forthebadge](https://forthebadge.com/images/badges/built-by-developers.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/built-for-android.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/built-with-love.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/check-it-out.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/just-plain-nasty.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/made-with-java.svg)](https://forthebadge.com)

Hasi is an Android Application that utilizes the Google Vision Application Programming Interface (API) to predict the 
happiness of a person which is ascertained whether the person is smiling or not. This is achieved using Classification which is determined if a certain facial characteristic is present.

## Concept Behind Vision API 

A landmark is a point of interest within a face. The left eye, right eye, and nose base are all examples of landmarks. The figure below shows some examples of landmarks:

![image](https://developers.google.com/vision/images/landmarks.jpg)

Rather than first detecting landmarks and using the landmarks as a basis of detecting the whole face, the Face API detects the whole face independently of detailed landmark information. For this reason, landmark detection is an optional step that could be done after the face is detected. Landmark detection is not done by default, since it takes additional time to run. You can optionally specify that landmark detection should be done.

Classification determines whether a certain facial characteristic is present. The Android Face API currently supports two classifications: eyes open and smiling. Classification is expressed as a certainty value, indicating the confidence that the facial characteristic is present. For example, a value of 0.7 or more for the smiling classification indicates that it is likely that a person is smiling.

Both of these classifications rely upon landmark detection.

Also note that “eyes open” and “smiling” classification only works for frontal faces, that is, faces with a small Euler Y angle (at most about +/- 18 degrees).

## Creating a Face Detector 

A face detector is created in the onCreate method of the app’s main activity. It is initialized with options for detecting faces with landmarks in a photo:

```java
FaceDetector detector = new FaceDetector.Builder(context)
    .setTrackingEnabled(false)
    .setLandmarkType(FaceDetector.ALL_LANDMARKS)
    .build();
```
Setting “tracking enabled” to false is recommended for detection for unrelated individual images (as opposed to video or a series of consecutively captured still images), since this will give a more accurate result. But for detection on consecutive images (e.g., live video), having tracking enabled gives a more accurate and faster result.

Note that facial landmarks are not required in detecting the face, and landmark detection is a separate (optional) step. By default, landmark detection is not enabled since it increases detection time. We enable it here in order to visualize detected landmarks.

Given a bitmap, we can create Frame instance from the bitmap to supply to the detector:

```java
Frame frame = new Frame.Builder().setBitmap(bitmap).build();
```

The detector can be called synchronously with a frame to detect faces:

```java
SparseArray<Face> faces = detector.detect(frame);
```
The result returned includes a collection of Face instances. We can iterate over the collection of faces, the collection of landmarks for each face, and draw the result based on each landmark position:

```java
for (int i = 0; i < faces.size(); ++i) {
    Face face = faces.valueAt(i);
    for (Landmark landmark : face.getLandmarks()) {
        int cx = (int) (landmark.getPosition().x * scale);
        int cy = (int) (landmark.getPosition().y * scale);
        canvas.drawCircle(cx, cy, 10, paint);
    }
}
```

## References 

[Google Developers](https://developers.google.com/)<br>
[Google Codelabs](https://codelabs.developers.google.com/)

## LICENSE

This project is licensed under the Apache-2.0 License - see the [LICENSE](https://github.com/HarshCasper/Hasi/blob/master/LICENSE) file for details.



