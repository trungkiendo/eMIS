
"""
Deploy Flask App in IIS Server
"""from flask import Flaskapp = Flask(__name__)
@app.route("/")
def home():
    return "Hello, This is the Flask App in IIS Server."if __name__ == "__main__":
    app.run()
