НАЧАЛО РАБОТЫ С beautifulsoup

`pip install beautifulsoup4`

`pip install requests`

`import requests`
`from bs4 import BeautifulSoup`

создаем объект bs
но перед этим получаем данные веб-страницы

`response = requests.get("http://www.example.com").text`

`soup = BeautifulSoup(response, 'lxml')`

`pip install lxml`

ОСНОВНЫЕ МЕТОДЫ
find()

`soup.find("div", class_ = "example-class").text` - указываем сначала основной тэг, потом его класс

find_all
аналогично, но вернет список, поэтому text применять нельзя
