
<div dir=auto>
  
# שיעורי הרב שרקי
## הקדמה
אהלן :)

הפתרונות הקיימים לשמיעת השיעורים של הרב (במובייל) לא נוחים מספיק לטעמי.
הסיבות לכך רבות ואין לי פנאי לפרטם כרגע. אני מאמין שמי שמשתמש בהם יודע על מה אני מדבר.

מבלי לזלזל כמובן במפתחים שבוודאי עשו עבודה חשובה ורבה, אני מציע פתרון נוסף שיכול לשפר את [חווית המשתמש](https://he.wikipedia.org/wiki/%D7%97%D7%95%D7%95%D7%99%D7%99%D7%AA_%D7%9E%D7%A9%D7%AA%D7%9E%D7%A9).

עלה לי רעיון להכניס סדרות שלמות של שיעורים מתוך אתר "ערוץ מאיר" לתוך פורמט של פודקאסט ([rss feed](https://podcasters.apple.com/support/823-podcast-requirements)).
בכך יהיה לטעון במקלות סדרה לתוך כל [קליינט פודקאסט](https://en.wikipedia.org/wiki/List_of_podcatchers), למשל AntennaPod החינמית, ולקבל את כל היתרונות שיש לקליינט פודקאסט out-of-the-box.

אם רוצים לשפר את זה אפילו עוד יותר, האפליקציה שהזכרתי קודם, [AntennaPod](https://antennapod.org/), כתובה בקוד פתוח ואפשר ליצור ממנה אפליקציה יעודית שפידים של השיעורים טעונים בה מראש.

*([למשל, הפודקאסט "עושים היסטוריה" השתמש באפליקציה הנ"ל כדי ליצור לעצמו אפליקציה](https://play.google.com/store/apps/details?id=de.danoeh.antennapod_mh)).*


## פרטים טכניים

### קבלת הנתונים

נכון לעכשיו, קיים סוג של API בערוץ מאיר שמאפשר לקבל רשימה עם mp3 של שיעורים בפורמט JSON.

המבנה:
<div dir=auto>

```
http://82.80.198.104/MidrashServices/api/Lessons?pageSize=0&page=1&catId=0&rabbiId=0&setId=0
```
</div>

**הסבר קצר על הפרמטרים:**
* `pageSize` - מספר הרשומות לעמוד. עד כמה שאני מבין זה מתאים לJSON ולא לפיד של פודקאסט כי שם לא מחלקים לעמודים אלא הכל בפיד אחד. צריך להיות לפחות באורך של מספר השיעורים כדי שיציג הכל בעמוד אחד.
* `page` - מספר העמוד - רלוונטי רק אם מחלקים את הפיד לכמה עמודים. כאמור, בRSS זה לא רלוונטי.
* `catId` - קטגוריה בערוץ מאיר, כלומר נושא מסוים. לדעתי עדיף לחלק לפי סדרות ולא לפי קטגוריות.
* `rabbiId` - המספר של הרב בערוץ מאיר. המספר של הרב שרקי הוא `3774`.
* `setId` - מספר הסידרה. הפרמטר החשוב ביותר.

למשל, אם רוצים לקבל את ה־JSON של הסדרה "[איגרת תימן - התשעד](https://meirtv.com/shiurim-series/22619)":

<div dir=auto>
  
```
https://meirtv.com/shiurim-series/22619
```
</div>

אפשר לראות שה־ID שלה הוא `23019` ושיש לסדרה 26 פרקים. אני רוצה לקבל את כל הפרקים בעמוד אחד ולכן הבקשה צריכה להיות:

<div dir=auto>
  
```
http://82.80.198.104/MidrashServices/api/Lessons?setId=22619&pageSize=26
```
</div>

אז קיבלנו JSON שמכיל 25 רשומות (0-24) כשהשיעור האחרון (החדש ביותר) נמצא הכי למעלה.

*(הערה: הקובץ החזיר רק 25 רשומות למרות שלכאורה יש 26 פרקים. הסיבה היא שמשום מה באתר חסר את פרק 12).*

### המרה לפורמט המתאים לפודקאסט
עכשיו בגדול צריך להמיר את הקובץ ל־XML ומשם לפורמט של פודקאסט. אני אראה עכשיו דרך ידנית לעשות את זה, אבל כמובן שזה לא פרקטי לעשות את זה על עשרות סדרות ולכן צריך לכתוב קוד שיעשה את זה.

הנה דוגמא ל"פוסט" של שיעור אחד. לאחר ניקוי התגיות הלא רלוונטיות לפודקאסט זה מה שמתקבל:

<div dir=auto>
  
```json
     {
        "Description": "לקראת סיום האיגרת לחכמי תימן הרמב\"ם מבקש הרמב\"ם להעביר את האגרת בין כל הקהילות וה' יצילנו ויקבץ נדחינו מכל קצוות הארץ.",
        "LessonLength": 1332,
        "Mp3Path": "https://storage.googleapis.com/meirtvmp3/archive/hebrew/mp3/sherki/126/idx_12644.mp3",
        "RecordDate": "2015-02-01T10:35:54",
        "Title": "כיצד ניתן לדעת אם החלה הגאולה?"
      },
```
</div>

השתמשתי [בכלי הזה](https://jsonformatter.org/json-to-xml) כדי להמיר מ־JSON ל־XML:



<div dir=auto>


```xml
    <0>
        <Description>לקראת סיום האיגרת לחכמי תימן הרמב"ם מבקש הרמב"ם להעביר את האגרת בין כל הקהילות וה' יצילנו ויקבץ נדחינו מכל קצוות הארץ.</Description>
        <LessonLength>1332</LessonLength>
        <Mp3Path>https://storage.googleapis.com/meirtvmp3/archive/hebrew/mp3/sherki/126/idx_12644.mp3</Mp3Path>
        <RecordDate>2015-02-01T10:35:54</RecordDate>
        <Title>כיצד ניתן לדעת אם החלה הגאולה?</Title>
    </0>
```
</div>

עכשיו צריך להתאים לתקן של פודקאסט. אני השתמשתי רק בתגיות המינימליות שהיה צריך בשביל שזה יעבוד. זה לא כולל את כל התגיות לפי התקן של iTunes. [התיעוד המלא של iTunes נמצא כאן](https://podcasters.apple.com/support/823-podcast-requirements).

<div dir=auto>
  
```xml
<item>
    <title>כיצד ניתן לדעת אם החלה הגאולה?</title>
    <description>לקראת סיום האיגרת לחכמי תימן הרמב"ם מבקש הרמב"ם להעביר את האגרת בין כל הקהילות וה' יצילנו ויקבץ נדחינו מכל קצוות הארץ.</description>
    <itunes:image href="put_podcast_cover_art_here.jpg"/>
    <enclosure url="https://storage.googleapis.com/meirtvmp3/archive/hebrew/mp3/sherki/126/idx_12644.mp3"  type="audio/mpeg"/>
    <itunes:duration>13:32</itunes:duration>
    <pubDate>2015-02-01T10:35:54</pubDate>
</item>
```
</div>

וזהו בגדול :)

מקווה שיצא הסבר מובן. דברו איתי אם לא הבנתם משהו.

מצ"ב דוגמא לRSS כזה:
https://dl3.pushbulletusercontent.com/ZDGQKTnJwRK7N8r4YhvpeVOlwNFSVN6E/pirkeyavot.xml

<img src="https://user-images.githubusercontent.com/18702632/117589485-738c7c80-b132-11eb-820a-4de84cdce9ae.jpg" width="300px">





