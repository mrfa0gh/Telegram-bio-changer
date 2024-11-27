from pyrogram import Client
from datetime import datetime
import time
import requests

#All keys - api u neew is in https://openweathermap.org/ || https://my.telegram.org/
# it will change bio for examble Date: 27:11:2024 Time: 10:44 PM Weather: Clear sky ☀️ Temp: 16°C
API_ID = ""  
API_HASH = ""  
SESSION_NAME = "my_account" # but any name u want

# إعدادات الطقس
WEATHER_API_KEY = ""  
CITY = "Cairo"  
WEATHER_API_URL = f"https://api.openweathermap.org/data/2.5/weather?q={CITY}&appid={WEATHER_API_KEY}&units=metric"

# إنشاء عميل تليجرام
app = Client(SESSION_NAME, api_id=API_ID, api_hash=API_HASH)

def get_weather():
    """إحضار حالة الطقس ودرجة الحرارة"""
    try:
        response = requests.get(WEATHER_API_URL)
        data = response.json()
        
        if response.status_code == 200 and 'weather' in data:
            weather = data['weather'][0]['description'].capitalize()  
            temp = int(data['main']['temp'])  
            if "rain" in weather.lower():
                emoji = "🌧️"
            elif "clear" in weather.lower():
                emoji = "☀️"
            elif "cloud" in weather.lower():
                emoji = "☁️"
            elif "snow" in weather.lower():
                emoji = "❄️"
            else:
                emoji = "🌡️"
            return f"{weather} {emoji}", temp
        else:
            print(f"خطأ في استجابة الطقس: {data}")
            return "Unknown", 0
    except Exception as e:
        print(f"خطأ أثناء جلب الطقس: {e}")
        return "Unknown", 0

def update_bio():
    """تحديث البايو بتفاصيل الطقس والتاريخ والوقت"""
    with app:
        while True:
            # جلب بيانات الطقس
            weather, temp = get_weather()
            
            # الحصول على التاريخ والوقت
            now = datetime.now()
            current_date = now.strftime("Date: %d:%m:%Y")  
            current_time = now.strftime("Time: %I:%M %p") 

            new_bio = f"{current_date}\n{current_time}\nWeather: {weather}\nTemp: {temp}°C"
            
            app.update_profile(bio=new_bio)
            print(f"تم تحديث البايو إلى:\n{new_bio}")
            
            time.sleep(3600)  # تحديث كل ساعة

if __name__ == "__main__":
    update_bio()
