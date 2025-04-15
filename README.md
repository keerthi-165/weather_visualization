# weather_visualization
import requests
import matplotlib.pyplot as plt
import seaborn as sns

cities = ['New York', 'London', 'Tokyo', 'Paris', 'Sydney', 'Mumbai']
API_KEY = '6c3d7bfbf24d17cdac77cbc7916bcca'  # 🔑 Replace this with your actual API key
BASE_URL = 'http://api.openweathermap.org/data/2.5/weather'

def get_temperature(city):
    params = {
        'q': city,
        'appid': API_KEY,
        'units': 'metric'
    }
    response = requests.get(BASE_URL, params=params)
    data = response.json()
    if response.status_code == 200:
        return data['main']['temp']
    else:
        print(f"Failed to get data for {city}: {data.get('message', 'No message')}")
        return None

temperatures = []
for city in cities:
    temp = get_temperature(city)
    if temp is not None:
        temperatures.append(temp)
    else:
        temperatures.append(0)

sns.set(style="whitegrid")
plt.figure(figsize=(10, 6))
sns.barplot(x=cities, y=temperatures, palette="coolwarm")
plt.title("Current Temperature in Cities (°C)")
plt.ylabel("Temperature (°C)")
plt.xlabel("City")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
