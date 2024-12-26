from flask import Flask, jsonify, request
import speech_recognition as sr
import pyttsx3

app = Flask(__name__)
tasks = []

# Sesli yanıt için ayarlar
engine = pyttsx3.init()

def speak(text):
    engine.say(text)
    engine.runAndWait()

@app.route('/tasks', methods=['GET', 'POST'])
def manage_tasks():
    if request.method == 'POST':
        task = request.json.get('task')
        if task:
            tasks.append(task)
            speak(f"Görev eklendi: {task}")
            return jsonify({'message': 'Görev eklendi!'}), 201
        return jsonify({'message': 'Görev boş olamaz!'}), 400
    else:
        return jsonify({'tasks': tasks})

@app.route('/speak', methods=['POST'])
def speak_text():
    text = request.json.get('text')
    if text:
        speak(text)
        return jsonify({'message': 'Konuşma gerçekleştirildi!'}), 200
    return jsonify({'message': 'Metin boş olamaz!'}), 400

if __name__ == '__main__':
    app.run(debug=True)
