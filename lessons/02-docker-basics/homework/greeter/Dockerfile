

# תשובות — Docker Basics

## 1. בניית תמונה

מתיקיית השורש של המאגר:

```bash
docker build -t greeter:v1 lessons/02-docker-basics/homework/greeter
```

## 2. הרצת הקונטיינר ברקע

```bash
docker run -d -p 9000:8000 --name greeter greeter:v1
```

## 3. שינוי הברכה ל-Shalom

מוסיפים את משתנה הסביבה `GREETING`:

```bash
docker run -d -p 9000:8000 --name greeter -e GREETING=Shalom greeter:v1
```

## 4. התוצאה של הבקשה

בכתובת `http://localhost:9000/hello/Rivka` תתקבל התשובה:

```json
{"message":"Shalom, Rivka!"}
```

## 5. שימוש ב-cache לאחר שינוי app.py

שלבי `FROM`, `WORKDIR`, `COPY requirements.txt` ו-`RUN pip install` יכולים להישאר `CACHED`, כל עוד תמונת הבסיס וקובץ התלויות לא השתנו.

שלב `COPY app.py` ירוץ מחדש כי תוכן הקובץ השתנה. גם שלבים שאחריו עשויים להיבנות מחדש, בהתאם ל-cache של Docker.
```

