## חבילות ומכולות

החלקים הראשונים במערכת המודולים בהן נדון הם חבילות ומכולות.

*מכולה* (crate) היא כמות הקוד המינימלית שהקומפיילר של ראסט מעבד בזמן נתון. אפילו כשמריצים `rustc` במקום `cargo` ומעבירים רק קובץ בודד (כפי שעשינו בתחילת דרכנו בסעיף "כתיבת והרצת תכנית ראסט" בפרק 1), הקומפיילר מתייחס לקובץ זה כאל מכולה. מכולות יכולות להכיל מודולים, והמודולים יכולים להיות מוגדרים בקבצים אחרים שעוברים בעצמם קומפילציה יחד עם המכולה, כפי שנראה בסעיפים הבאים.

מכולה יכולה להיות באחת משתי הצורות הבאות: מכולה בינארית או מכולת ספריה. *מכולות בינאריות* הן תכניות שניתן לקמפל לקובץ הרצה, כמו תכנית שורת פקודה, או שרת. במקרה כזה חייבת להימצא פונקצית `main` שמגדירה מה קורה ברגע שמריצים את הקובץ. כל המכולות שיצרנו עד כה היו מכולות בינאריות.

*במכולות ספריה* אין פונקצית `main` והן לא עוברות קמפול לכדי קובץ הרצה. במקום זאת, הן מגדירות פונקציונאליות המיועדת לשימוש בפרוייקטים אחרים. למשל, המכולה `rand` בה השתמשנו [בפרק 2][rand]<!-- ignore --> מספקת פונקציונאליות לייצור מספרים מקריים. ברוב הפעמים בהם רסטיונארים משמתשים במינוח "מכולה" הם מתכוונים למכולת ספריה, והם משתמשים ב-"מכולה" באופן שקול למינוח הכללי "ספריה" מעולם התכנות.

*שורש המכולה* הוא קובץ מקור שמסמן לקומפיילר של ראסט מקום התחלה ומהווה את מודול השורש של המכולה שלכם (נרד לעומקם של מודולים בסעיף ["הגדרת מודולים לשליטה על מתחמים ופרטיות"][modules]<!-- ignore -->
).

*חבילה* היא אוסף של מכולה אחת או יותר שיחדיו מספקות פונקציונאליות מסויימת. כל חבילה מכילה קובץ *Cargo.toml* שמתאר כיצד לבנות את המכולות האלה. למעשה, קארגו בעצמו הוא חבילה שמכילה את המכולה הבינארית עבור כלי שורת-הפקודה בו אתם משתמשים כדי לבנות את הקוד שלכם. כחבילה, קארגו גם מכיל את מכולת הספריה עליה המכולה הבינארית מסתמכת. פרוייקטים אחרים יכולים גם הם להסתמך על מכולת הספריה של קארגו על מנת להשתמש באותה הלוגיקה בה כלי שורת-הפקודה קארגו משתמש.

חבילה יכולה להכיל כמה מכולות בינאריות שרק תרצו, אבל לכל היותר מכולת ספריה אחת. חבילה חייבת להכיל לפחות מכולה אחת, בין אם זו מכולת ספריה או מכולה בינארית.

הבה נראה מה קורה כאשר אנו יוצרים חבילה. ראשית, נזין את הפקודה `cargo new`:

```console
$ cargo new my-project
     Created binary (application) `my-project` package
$ ls my-project
Cargo.toml
src
$ ls my-project/src
main.rs
```

לאחר הרצת `cargo new`, אנו מפעילים את `ls` כדי לראות מה קארגו יצר עבורנו. בתוך תיקיית הפרוייקט נמצא הקובץ *Cargo.toml*, ובכך מקנה לנו חבילה. יש גם את התיקייה *src* שמכילה את הקובץ *main.rs*. פתחו את *Cargo.toml* באדיטור שלכם, ושימו לב שאין זכר ל-*src/main.rs*. קארגו עוקב אחר הקונבנציה ש-*src/main.rs* הוא שורש המכולה של מכולה בינארית עם אותו שם כמו החבילה. באופן דומה, קארגו יודע שאם תיקיית החבילה כוללת את הקובץ *src/lib.rs*, אז החבילה כוללת מכולת ספריה עם אותו שם כמו החבילה, וכמו כן ש- *src/lib.rs* מהווה את שורש המכולה. קארגו מעביר את קבצי שורש המכולה ל- `rustc` למטרת בנית הספריה או הבינארי.

במקרה שלנו, יש לנו חבילה שמכילה רק את *src/main.rs*, ומשמעות הדבר היא שהחבילה מכילה רק מכולה בינארית, וששמה הוא `my-project`. במידה וחבילה מכילה גם קובץ *src/main.rs* וגם קובץ *src/lib.rs*, אז יש לה שתי מכולות: מכולה בינארית ומכולת ספריה, שתיהן בעלות אותו שם כמו החבילה. חבילה יכולה להכיל מכולות בינאריות מרובות ע"י מיקום קבצים בתיקייה *src/bin*: כל קובץ יהווה מכולה בינארית נפרדת. 

[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
