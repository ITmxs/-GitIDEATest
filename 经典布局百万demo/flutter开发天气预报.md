https://pub.dev/packages/weather





https://github.com/huang-weilong/d9l_weather



今天接到新的需求，需要给app添加一个天气预报功能

懂城

**48fbadf80bbc43ce853ab9a92408373e**



公共id

 HE2103031446151451



```dart
  Widget _threeDayWeather(DailyForecast dailyForecast) {
    String date =
        DateFormat('EE', D9l().lang).format(DateTime.parse(dailyForecast.date));
    return Expanded(
      child: Column(
        children: <Widget>[
          Text(date, style: TextStyle(color: Colors.white)),
          Padding(
            padding: EdgeInsets.symmetric(vertical: 4.0),
            child: Image.asset(
                'assets/images/weather/${dailyForecast.condCodeD}.png',
                color: Colors.white),
          ),
          Text(dailyForecast.condTxtD, style: TextStyle(color: Colors.white)),
          Padding(
            padding: EdgeInsets.only(top: 4.0),
            child: Text(
                dailyForecast.tmpMin + '℃~' + dailyForecast.tmpMax + '℃',
                style: TextStyle(color: Colors.white)),
          ),
        ],
      ),
    );
  }
```



可访问



https://devapi.qweather.com/v7/weather/now?location=101010100&key=48fbadf80bbc43ce853ab9a92408373e

https://devapi.qweather.com/v7/weather/now?location=121.27,30.19&key=48fbadf80bbc43ce853ab9a92408373e





公司：

https://devapi.qweather.com/v7/weather/now?location=121.27,30.19&key=993ca7e0a5524a7c8e18117180f42827

basurl=https://devapi.qweather.com/v7/weather/now

key=993ca7e0a5524a7c8e18117180f42827