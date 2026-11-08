from flask import Flask, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask(__name__)


@app.route("/")
def home():
    return "Welcome to the Emotion Detection Application!"


@app.route("/emotionDetector")
def sent():

    text_to_analyze = request.args.get('textToAnalyze')

    if not text_to_analyze or not text_to_analyze.strip():
        return "Please provide a statement to analyze.", 400

    response = emotion_detector(text_to_analyze)

    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    return (
        f"For the given statement, the system response is "
        f"'anger': {anger}, "
        f"'disgust': {disgust}, "
        f"'fear': {fear}, "
        f"'joy': {joy} "
        f"and 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)