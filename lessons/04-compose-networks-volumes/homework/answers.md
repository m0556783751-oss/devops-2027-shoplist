# שיעור 4 — תשובות

## 1. איך מגיעים ל-backend בלי ports?
stats מגיע ל-backend דרך הרשת הפנימית ש-Compose יוצר, לפי שם ה-service: `http://backend:5000`. הדפדפן לא מגיע אליו ישירות: הוא פונה ל-Nginx ב-`localhost:8080/api/...`, ו-Nginx (שנמצא באותה רשת) מעביר את הבקשה ל-`backend:5000`.

## 2. down ו-up, ומה עם down -v?
אחרי `down` ו-`up` המוצרים נשארים, כי הם שמורים ב-named volume (`postgres-data`), ו-`down` מוחק רק containers ורשת, לא volumes. `down -v` מוחק גם את ה-volume, ולכן כל המוצרים נמחקים והמסד נוצר מחדש ריק.

## 3. השורה ../sql/init.sql:/docker-entrypoint-initdb.d/init.sql
זה bind mount: הצד השמאלי הוא נתיב לקובץ במחשב (מתחיל ב-`../`), ולא שם של volume. אם משנים את `init.sql` אחרי שהמסד כבר נוצר, לא יקרה כלום, כי Postgres מריץ את הקבצים בתיקייה הזו רק כשתיקיית הנתונים ריקה, כלומר ביצירה הראשונה. כדי שהשינוי ייכנס צריך `down -v` ואז `up`.

## 4. שמות הרשת וה-volume
Compose לוקח את שם הפרויקט משם התיקייה שבה נמצא הקובץ, כלומר `docker`. לכן הרשת נקראת `docker_default` וה-volume נקרא `docker_postgres-data` (שם הפרויקט, קו תחתון, והשם שנתנו בקובץ).

## 5. מה יש ב-image הסופי של Dockerfile.multistage?
יש בו: Nginx, שלושת הקבצים הבנויים מ-`dist`, ו-`nginx.conf`. אין בו: Node, `build.js`, ואת קובצי המקור. אין `CMD` כי יורשים אותו מ-image של nginx, שכבר מגדיר הרצה של nginx. ל-stats לא צריך multi-stage כי אין לו שלב בנייה: הוא רץ ישירות ב-Node, ובלי חבילות חיצוניות, כך שאין "כלי מטבח" שאפשר להשאיר מאחור.

## 6. הגדלים
<!-- להשלים לפי הפלט של docker images shoplist-frontend -->
- single: ____
- multi: ____
- kitchen: ____

`multi` כמעט בגודל של `single` כי שניהם מבוססים על אותו image של Nginx עם אותם שלושה קבצים קטנים. Node, שהוא החלק הכבד, נשאר רק ב-`kitchen` ולא עבר ל-image הסופי.

## 7. למה /api/stats מגיע ל-stats, ומה עושה הלוכסן?
Nginx בוחר את ה-`location` עם הקידומת הארוכה והמדויקת ביותר, ו-`/api/stats` ארוך יותר מ-`/api/`, ולכן הבקשה הולכת ל-stats. הלוכסן בסוף `http://backend:5000/` גורם ל-Nginx להחליף את החלק `/api/` בלוכסן, כך שבקשה ל-`/api/products` מגיעה ל-backend בתור `/products`.

## בונוס: איך מוודאים ש-.env לא ייכנס ל-Git?
מוסיפים את `.env` לקובץ `.gitignore`, ואז Git מתעלם ממנו ולא מציע להוסיף אותו. בודקים עם `git status` שהוא לא מופיע ברשימה.
