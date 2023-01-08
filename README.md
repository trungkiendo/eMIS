
"""
Deploy Flask App in IIS Server
"""from flask import Flaskapp = Flask(__name__)
@app.route("/")
def home():
    return "Hello, This is the Flask App in IIS Server."if __name__ == "__main__":
    app.run()
icacls . /grant "NT AUTHORITY\IUSR:(OI)(CI)(RX)"
icacls . /grant "Builtin\IIS_IUSRS:(OI)(CI)(RX)"
[gg](https://github.com/mayurvkorde/Flask-Application-On-IIS)
