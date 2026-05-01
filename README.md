# Collage_Predictor
The ML-Based College Predictor Telegram Bot for Indian Engineering Colleges is designed to assist students in selecting suitable colleges based on their entrance exam ranks. In India, the counselling process can be confusing due to the large number of colleges, varying cutoff trends, and lack of clear guidance.

from flask import Flask, request,
jsonify
import pickle
app = Flask(__name__)
# Load model
model =
pickle.load(open("../model/model.
pkl", "rb"))
le_category =
pickle.load(open("../model/categor
y_encoder.pkl", "rb"))
le_output =
pickle.load(open("../model/output_
encoder.pkl", "rb"))
@app.route("/")
def home():
return "College Predictor API
Running "
@app.route("/predict",
methods=["GET"])
def predict():
try:

rank =
int(request.args.get("rank"))

23

category =
request.args.get("category")
category_encoded =
le_category.transform([category])[
0]

prediction =

model.predict([[rank,
category_encoded]])

result =
le_output.inverse_transform(predi
ction)[0]
return jsonify({"prediction":
result})
except Exception as e:
return jsonify({"error": str(e)})
if __name__ == "__main__":
app.run(debug=True)
