conda activate forecasting

python central_server.py

python client_server.py 3 client3_train.csv client3_pred.csv

python client_server.py 2 client2_train.csv client2_pred.csv

python client_server.py 1 client1_train.csv client1_pred.csv


Then go to notebook and run:

#to register clients on central server
import requests

central_server_url = "http://localhost:8000"
client_configs = [
    {"client_id": 1, "central_server_url": central_server_url},
    {"client_id": 2, "central_server_url": central_server_url},
    {"client_id": 3, "central_server_url": central_server_url}
]

for config in client_configs:
    response = requests.post(f"http://localhost:{8000 + config['client_id']}/configure", json=config)
    print(f"Client {config['client_id']} configuration response:", response.json())


#to train- change epoch number
response = requests.post("http://localhost:8000/train", json={"epochs": 1})
print("Training response:", response.json())


#prediction:
user_id = 880547  # Replace with the actual user ID you want to predict for
response = requests.post("http://localhost:8000/predict", json={"client_id": user_id})
print(f"Prediction for user {user_id}:", response.json())

#client prediction

(requests.post("http://localhost:8002/predict", json={"client_id": 880547}).json())
