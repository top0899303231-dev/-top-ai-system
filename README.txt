import requests
from flask import Flask, render_template_string, request, jsonify
import os
from datetime import datetime

app = Flask(__name__)

# ดึงรหัสจาก Cloud (Environment Variables)
LINE_ACCESS_TOKEN = os.environ.get('LINE_TOKEN')
USER_ID = os.environ.get('LINE_USER_ID')

def send_line_push(text):
    url = 'https://api.line.me/v2/bot/message/push'
    headers = {
        'Content-Type': 'application/json',
        'Authorization': f'Bearer {LINE_ACCESS_TOKEN}'
    }
    payload = {'to': USER_ID, 'messages': [{'type': 'text', 'text': text}]}
    try:
        res = requests.post(url, headers=headers, json=payload)
        return res.status_code == 200
    except:
        return False

@app.route('/')
def home():
    return "<h1>TOP AI SYSTEM ONLINE 24/7</h1><p>ระบบของช่างพร้อมทำงานบน Cloud แล้วครับ!</p>"

@app.route('/api/push', methods=['POST'])
def api_push():
    msg = request.json.get('message')
    send_line_push(f"☁️ จาก Cloud:\n{msg}")
    return jsonify({"status": "success"})

if __name__ == '__main__':
    app.run()
