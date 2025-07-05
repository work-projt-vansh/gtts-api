from flask import Flask, request, send_file
from gtts import gTTS
import os

app = Flask(__name__)

@app.route('/tts', methods=['POST'])
def tts():
    data = request.get_json()
    text = data.get("text", "").strip()

    if not text:
        return {"error": "No text provided"}, 400

    tts = gTTS(text)
    filename = "output.mp3"
    tts.save(filename)

    return send_file(filename, mimetype="audio/mpeg")

if __name__ == '__main__':
    port = int(os.environ.get("PORT", 10000))
    app.run(host='0.0.0.0', port=port)
