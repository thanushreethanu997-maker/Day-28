import requests
import os
from dotenv import load_dotenv

load_dotenv()

API_KEY = os.getenv("API_KEY")


def get_weather(city):

    if not API_KEY:
        print("API key is missing. Check your .env file.")
        return

    geo_url = "http://api.openweathermap.org/geo/1.0/direct"

    geo_params = {
        "q": city,
        "limit": 1,
        "appid": API_KEY
    }

    try:
        geo_response = requests.get(
            geo_url, params=geo_params, timeout=10
        )
        geo_response.raise_for_status()
        locations = geo_response.json()

        if not locations:
            print("Invalid city name!")
            return

        lat = locations[0]["lat"]
        lon = locations[0]["lon"]

        weather_url = "https://api.openweathermap.org/data/2.5/weather"

        weather_params = {
            "lat": lat,
            "lon": lon,
            "appid": API_KEY,
            "units": "metric"
        }

        response = requests.get(
            weather_url, params=weather_params, timeout=10
        )
        response.raise_for_status()

        data = response.json()

        print("\n----- WEATHER REPORT -----")
        print("City:", data["name"])
        print("Temperature:", data["main"]["temp"], "°C")
        print("Weather:", data["weather"][0]["description"])
        print("Humidity:", data["main"]["humidity"], "%")
        print("Wind Speed:", data["wind"]["speed"], "m/s")
        print("--------------------------")

    except requests.exceptions.HTTPError:
        print("API error. Check your API key or city name.")

    except requests.exceptions.RequestException:
        print("Network error. Please check your internet.")

    except (KeyError, IndexError, ValueError):
        print("Unable to process weather information.")


def main():
    print("Welcome to Weather Data CLI!")

    city = input("Enter city name: ").strip()

    if not city:
        print("City name cannot be empty!")
        return

    get_weather(city)


if __name__ == "__main__":
    main()
