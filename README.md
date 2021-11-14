## Программа для распознавания различных объектов в видеопотоке

Программа предназначена для распознавания лиц и глаз на изображении, получаемом с веб-камеры.

**Что нужно?**
+ Веб-камера (встроенная или внешняя)
+ Установленная версия python > 3.9
+ пакет opencv-python
+ opencv, установленный локально (инструкции по установке OpenCV для каждой из систем)
  - https://losst.ru/ustanovka-opencv-v-ubuntu-18-04 - для Linux
  - https://coderoad.ru/47332282/Как-установить-OpenCV-Python-на-Mac - для MacOs
  - https://pysource.com/2019/03/15/how-to-install-python-3-and-opencv-4-on-windows/ - для Windows

После установки OpenCV клонируйте этот репозиторий себе на компьютер или скачайте его, создайте новое виртуальное окружение в папке с репозиторием командой
```bash
python -m venv env
```  
Активируйте виртуальное командой
```bash
source env/bin/activate
```
Установите пакет opencv-python
```bash
pip install -r requirements.txt
```  

Если на вашем компьютере есть встроенная веб-камера, то ничего менять не надо, просто запускайте скрипт командой
```bash
python run.py
```

**Если веб-камера внешняя, то необходимо внести изменения в run.py.**   
Откройте файл run.py
```bash
vim run.py
```

Найдите участок
```python
if __name__ == "__main__":
    cascades = get_cascades()
    video_capture = cv2.VideoCapture(0)
```
Замените
```python
video_capture = cv2.VideoCapture(0)
```
на
```python
video_capture = cv2.VideoCapture(1)
```

Теперь можно работать. Запустите скрипт. 
```bash
python run.py
```
Программа обнаружит на видео лицо и глаза, и обведет их в синий и зеленый квадраты соответсвенно

Если программа не работает при запуске через ide, стоит попробовать запустить ее через терминал

### Настройка дополнительных параметров

Откройте файл config.py

В нем можно настроить, что именно будет выделять веб-камера
Для примера разберем первый элемент ```CASCADES```
```python

CASCADES = {
  "Face front": {
    "path": "haarcascades/faces/haarcascade_frontalface_default.xml",
    "color": (255, 0, 0),
    "draw": True
  },
```

Face front - анфас, присваивая переменной   
```"draw"``` значение ```True``` или ```False```, можно регулировать распознавание анфаса  
```"color"``` - параметры цвета рамки для выделения  
```"path"``` - путь к xml файлам для распознавания


Аналогичным образом настраиваются и:  
- Face profile - параметры для профиля лица (вид сбоку)  
- Smile - параметры для улыбки  
- Eyes - параметры для глаз  
- Full body - параметры для всего тела  
- Cat face - параметры для кошачьих мордочек  

**Важно**
Не стоит пытаться распознавать все и сразу. Это достаточно сильно нагружает систему

Стоит обратить внимание на содержимое папки ```haarcascades/faces```  
Там есть и альтернативные способы детекции лиц, глаз, глаз в очках или кошачьих мордашек.
Еще больше каскадов Хаара здесь:
https://github.com/opencv/opencv/tree/master/data/haarcascades  
Не забудьте внести изменения в```"path"``` - путь к xml файлам для распознавания

Опционально можно настроить распознавание бананов или автомобилей. Тогда в ```CASCADES``` необходимо добавить блок
```python
  "Banana": {
    "path": "haarcascades/banana_classifier.xml",
    "color": (255, 0, 0),
    "draw": True
  },
```
Теперь код распознает бананы и может их классифицировать.

Также скрипт можно настроить на распознавание 
- знаков ограничения скорости 
- стоп-сигналов
- светофоров
- знаков "Уступи дорогу"  
  ```"path": ``` будет меняться соответственно изменению пути до файла .xml

Давайте научим скрип распознавать светофоры. Добавьте следующий участок кода в ```CASCADES```

```python
  "traffic_light": {
    "path": "haarcascades/road-signs/Speed Limit Signs/HAAR/Speedlimit_HAAR_ 17Stages.xml",
    "color": (100, 100, 0),
    "draw": True
  },
```

Теперь на изображении светофоры будут обведены рамкой. Можете попробовать разные способы детекции, отличные от каскадов Хаара.
метод LBP (подробнее здесь: https://www.pyimagesearch.com/2015/12/07/local-binary-patterns-with-python-opencv/)

```"path": haarcascades/road-signs/Speed Limit Signs/LBP_Size_20/Speedlimit_24_15Stages.xml```
